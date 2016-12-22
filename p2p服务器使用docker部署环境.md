一. 安装docker

1.操作系统：centos7

执行 yum install docker

2. docker version查看是否安装成功，输出版本号，证明已经安装成功,(同时可以看到docker使用的go语言版本)

 



3. 启动

启动docker的命令：

service docker start

chkconfig docker on

或者centos7上的新命令：

systemctl start docker.service

systemctl enable docker.service

二. 安装weave

1.wget -O /usr/local/bin/weave [https://raw.githubusercontent.com/zettio/weave/master/weave](https://raw.githubusercontent.com/zettio/weave/master/weave)

2. chmod a+x /usr/local/bin/weave

三. 下载镜像-部署容器

参考策略：一个被测服务器可以部署一个容器，mysql可单独部署一个容器，telegraf-influxdb-grafana若用到可单独部署一个容器，jenkins-jmeter-ant也可单独部署一个容器。

注：也可根据实际环境需要更改

| 镜像名称 | 操作系统 | 已安装内容 | 可部署容器 | 镜像所在地址 |
| --- | --- | --- | --- | --- |
| centos6.8-p2p-servertest | centos6.8 | nginx  rz  sz  vi vim tcpdump
 gc++  mysql-devel  openssl | 被测服务器 | docker hub
git hub |
| p2p-mysql | centos6.8 | mysql mysql-server
mysql-devel | mysql |
| telegraf-influxdb-grafana | centos6.8 | telegraf influxdb  grafana | telegraf
influxdb
grafana |
| jenkins-jmenter-ant | centos6.8 | jdk  tomcat jmeter
ant jenkins | jmeter
ant
jenkins |

1. 部署被测服务器

1. 使用镜像：

docker pull yangliyun2016/centos6.8-p2p-servertest:1.0

       或

docker build -t imagename dockerfile\_path

或

docker load -i centos6.8-p2p-servertest.tar(此镜像的打包文件)

1. 启动weave：weave launch
2. 启动服务器容器：docker run -t -i --name servername   --privileged=true -v /opt/server :/mnt/server  yangliyun2016/centos6.8-p2p-servertest:1.0，此时会得到一个containerID
3. 使用weave给容器分配IP地址：weave attach 10.0.0.3/24 containerID
4. 在容器里面执行ifconfig，可以看到两个ip，weave分配的IP用于容器间的通信，docker分配的IP用于外界服务器访问

    备注：weave分配的IP地址可用于同一台宿主主机间的容器访问，也可用于跨主机间的容器访问，此时另一台主机上要使用weave launch &lt;hostIP&gt;  hostIP代表上一台宿主机的IP地址

2. 部署mysql服务

1. 使用镜像：

docker pull yangliyun2016/p2p-mysql：1.0

        或

docker build -t imagename dockerfile\_path

或

docker load -i  p2p-mysql.tar(将image打包后的tar文件)

1. 启动mysql容器：docker run -t -i --name mysql5 --privileged=true  -v /home/yangliyun:/mnt/database  -P  yangliyun2016/p2p-mysql：1.0
2. 使用查看mysql的3306端口映射到宿主主机的那个端口，之后可使用navicate访问（宿主主机IP：映射后端口）

            查看方法：docker port contianerID 或者docker ps

1. 使用weave给mysql容器分配IP：weave attach 10.0.0.3/24 containerID，被测服务器使用weave分配的ip地址访问数据库，注：分配IP时要把被测试服务器容器ip与mysql容器ip要在同一网段

3. 部署telegraf influxdb grafana（如果需要）

1. 获取镜像：

docker pull yangliyun2016/telegraf-influxdb-grafana

或者

docker build -t imagename dockerfile\_path

或者

docker load -i  telegraf-influxdb-grafana.tar

1. 启动telegraf influxdb grafana容器：docker run -t -i --name telegraf-influxdb-grafana  -P  yangliyun2016/telegraf-influxdb-grafana(或者image ID)，容器启动后三个相应服务也已启动
2. 查看映射到宿主主机的端口

   docker port contianerID 或者docker ps

浏览器访问时使用宿主主机IP及映射到宿主主机的端口

4. 部署jenkins ant jmeter容器（如果需要）

1. 获取镜像：

docker pull yangliyun2016/jenkins-jmenter-ant:1.0

或

docker build -t imagename dockerfile\_path

或

docker load -i  jenkins-jmenter-ant.tar

1. 部署容器：docker run -t -i --name telegraf-influxdb-grafana  -P  yangliyun2016/jenkins-jmenter-ant:1.0(或者image ID)
2. 查看映射到宿主主机的端口

         docker port contianerID 或者docker ps

  浏览器访问时使用宿主主机IP及映射到宿主主机的端口
