请不要忘记docker本身带了最好的命令使用方法：

docker --help    搜索镜像的详细使用方法

docker run/commit/attach/logs/login.........等 --help   搜索容器操作的详细方法

可根据docker的变迁图来理解

 ![image](https://github.com/p2ptest/docker-p2p-servertest/blob/master/images/4.jpg)

归纳总结为：

1. 容器生命周期管理 — docker [run|start|stop|restart|kill|pause|unpause]
2. 容器操作运维 — docker [ps|inspect|top|attach|events|logs|wait|export|port|rm]
3. 容器rootfs命令 — docker [commit|cp|diff]
4. 镜像仓库 — docker [login|pull|push|search]
5. 本地镜像管理 — docker [images|rmi|tag|build|history|save|load|import]
6. 其他命令 — docker [info|version]

以下简单的说一下常用的，其他细节性的可以查看--help

1. 列出机器本地的镜像：

docker images

 ![image](https://github.com/p2ptest/docker-p2p-servertest/blob/master/images/5.jpg)

可以根据REPOSITORY来判断这个镜像是来自哪个服务器，类似regsistory.example.com:5000/repos\_name则表示的是私服。也可以看到镜像的ID和tag（通过tag可以识别仓库中不同的镜像）

2. docker hub中搜索image

docker search 镜像名

搜索的范围是官方镜像和个人所有镜像（个人镜像由docker社区成员创建和维护，这些镜像的前缀都是用户名标记）：

 ![image](https://github.com/p2ptest/docker-p2p-servertest/blob/master/images/6.jpg)

3. 从docker hub中下拉镜像

docker pull [options] NAME[:TAG]

4. 推送镜像到docker hub

docker push NAME[:TAG]

但是仅是上面做是不够的，需要先登录docker hub再打tag，之后才能push

step1——找到本地镜像的ID：docker images

step2——登陆Hub：docker login --username=username --password=password --email=email

step3——tag：docker tag &lt;imageID&gt; &lt;namespace&gt;/&lt;image name&gt;:&lt;version tag eg latest&gt;

step4——push镜像：docker push &lt;namespace&gt;/&lt;image name&gt;

5. 把机器上的镜像迁移到另一台

保存一个镜像到一个tar包：

docker save \*.tar  image\_name

加载一个tar包格式的镜像:

docker load -i file\_path

6. 在提交更改或镜像之后，可以为镜像添加新的标签(tag)

docker tag 5db5f8471261 ouruser/sinatra:devel

tag后面分别是镜像ID，用户名称，镜像源名，新的标签名（tag）

7. 删除镜像

docker rmi image

8.从image启动一个container

sudo docker run -t -i centos6.8-bfp2p:3.0   /bin/sh

-i :表示交互模式运行

-t：表示启动后会进入命令行

-v：表示需要将本地哪个目录挂载到容器中（对于内外传送文件比较方便）

-d: 表示守护模式，即后台运行

-p:表示宿主机与容器的端口映射

--name 表示容器启动后的名字

-w：设置容器的工作目录

--rm=true:表示容器操作完后删除

/bin/sh:表示容器启动后进入命令行模式

9. 查看容器的信息：

docker ps：查看正在运行的容器

docker ps -a：查看容器包括已经停止的

docker ps -l：显示最新启动的容器，包括已经停止的

10. 查看容器中正在运行的进程（top）

docker top &lt;container ID/contianer\_name&gt;

如果没有终端命令行来交互或者容器中没有top命令，那这个就很有用了

11. 删除容器：

docker rm &lt;container ID/container\_name&gt;

docker rm $(docker ps -a -q):删除所有已停止运行的容器，比较有用

12. 连接正在运行中的容器（attach）

docker attach &lt;container\_ID&gt;:可以同时连接上同一个containerID来共享刷屏,因为所有窗口都同步显示

13.开始-停止-重启 容器

docker start containerID：启动一个退出的容器

docker stop containerID：停止一个正在运行的容器

docker restart containerID:重新启动容器

14： 查看容器的标准输出

docker logs containerID

可以使用-f参数，与tail -f的原理一样

15. 查看容器或镜像的详细信息：

docker inspect containerID/imageID：可以看到容器的IP地址及端口

16. 从容器里拷贝文件或目录到本地

docker cp Name:/container\_path to\_path

docker cp ID:/container\_path to\_path

16. 查看容器映射到宿主主机的哪个端口上

docker port containerID [private\_port]

17. 将容器提交到镜像

docker commit [options] containerID

-a:作者信息

-m: commit message
