创建镜像时执行的脚本，类似与makefile或Ant中的build.xml文件，即告诉docker创建镜像时执行哪些命令。

网上例子很多

举个例子简单说明：

 ![image]()

生成镜像：

docker build -t name path

-t 表示要生成的镜像名

path：表示Dockerfile所在的目录，注意是目录

此外注意执行COPY时，

复制本地主机的&lt;src&gt;（为Dockerfile所在目录的相对路径）下的文件到容器中的&lt;dest&gt;下。

当使用本地目录为源目录时，推荐使用COPY
