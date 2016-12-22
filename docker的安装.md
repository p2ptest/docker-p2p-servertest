1. 安装环境：

操作系统：centos7，必须是64位，建议内核在3.8以上

查看centos内核命令：

uname -r

2. 安装

yum install docker

docker version查看是否安装成功，输出版本号，证明已经安装成功,(同时可以看到docker使用的go语言版本)

 ![image](https://github.com/p2ptest/docker-p2p-servertest/blob/master/images/3.jpg)



3. 启动

启动docker的命令：

service docker start

chkconfig docker on

或者centos7上的新命令：

systemctl start docker.service

systemctl enable docker.service
