### 关于 Docker

​	Docker 是一个开源的应用引擎，基于 Go 语言实现，基于 Linux 容器技术

​	Docker 是一个跨平台的 **轻量级虚拟机**，可移植性非常高

​	一次部署，终生可用

​	Docker 可以在 Linux , Windows , MacOS 等平台上安装使用

​	Docker 启动快，占用资源小



> Mac 系统上的 Docker ， 有一个 **新的、本地的** 虚拟系统取代虚拟盒子（VirtualBox）系统
>
> **这个东西叫 HyperKit**
>
> 当启动了 Docker 后，使用 top 命令可以看见 hyperkit 进程，一般占用一个G的内存



### 本地的镜像文件都存放在哪里？

​	与 Docker 相关的本地资源都放在 **/var/lib/docker** 目录下，其中：

​	 **container** 目录存放 **容器信息**

​	 **graph** 目录存放 **镜像信息**

​	 **aufs** 目录存放具体的 **镜像层文件**



### 如何将一台宿主主机的 Docker 环境迁移到另外一台宿主主机

​	停止 Docker 服务，将整个 Docker 存储文件夹（/var/lib/docker）复制到另外一台宿主主机，修改配置



### 一些参数的全称

更多的命令选项可以通过  **man docker-run** 命令来查看

 -c  ,  --cpu

-d  ,  --detach                   //在容器中后台执行命令

-e  ,   --env=[]                   //指定环境变量列表

-f  ,  --force    或者   --filter     或者   --format

-i   ,   --interractive=true|false     //打开标准输入接受用户输入命令，默认值为false

-m , --memory

-p  ,   --publish

-t   ,   --tty=true|false                    //分配伪终端，默认值为false

-v  ,   --volumes                              //数据卷



