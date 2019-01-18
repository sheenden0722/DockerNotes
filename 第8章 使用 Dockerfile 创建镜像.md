Dockerflle 主体内容分为四部分：

- [ ] 基础镜像信息                            FROM
- [ ] 维护者信息                                LABEL
- [ ] 镜像操作指令                            ……
- [ ] 容器启动时执行的命令            CMD    ENTRYPOINT

一般就是按照上面的顺序编写 Dockerfile，碰到 ADD、COPY、RUN 指令时，会生成一层新的镜像

### 8.1 基础镜像信息

▶ **FROM**

​	指定所创建镜像的 **基础镜像**

​	任何 Dockerfile 中第一条指令 **必须为 FROM 指令**

​	如果在同一个 Dockerfile 中创建多个镜像时，可以使用多个 FROM 指令（每个镜像一次）

> 为了保证镜像精简，可以选择体积较小的镜像如 **Alpine** 或者 **Debian** 作为基础镜像



### 8.2 维护者信息

▶ **LABEL**

​	LABEL 指令可以为生成的镜像添加元数据标签信息。这些信息可以用来辅助过滤出特定镜像。

> **LABEL 指令可以不写**

---

### 8.3 镜像操作指令

▶ **EXPOSE**

​	声明镜像内服务监听的端口

​	例如    EXPOSE 22 80 8443

【注意】：该指令只是起到声明的作用，并 **不会自动完成端口映射**

如果要映射端口出来，在启动容器时可以使用 ：

​	 **-P**              （ Docker主机晖自动分配一个宿主机的临时端口）

或者

​	 **-p HOST_PORT : CONTAINER_PORT**                      ( 具体指定所映射的本地端口)

---

▶ **ENV**

指定环境变量，在镜像生成过程中会被后续 RUN 命令使用

例如   ENV APP_HONE=/dfe/df/sd

---

▶ **WORKDIR**

为后续的 RUN、CMD、ENTRYPOINT 指令配置工作目录（即基于当前目录去到哪个目录）

---

▶ **USER** 

指定运行容器时的用户名或UID,后续 RUN 等命令也会使用指定的用户身份

---

▶ **ADD 和 COPY**

都是从本地复制文件到镜像

格式都是  **ADD / COPY <src> <dest>**

​	ADD：**添加**内容到镜像

​	COPY：**复制**内容到镜像

**区别：**

**ADD**： ADD 的 src 可以是：

​	 Dockerfile 所在目录的一个相对路径（文件或者目录）

​	 一个 URL

​	一个 tar 文件（**自动解压为目录**）

​	

​	ADD 的 dest 可以是：

​	1  镜像内**绝对路径**

​	 2  相对于工作目录（WORKDIR）的**相对路径**

**COPY** 就是简单的复制文件



**结论：**

> 从本地拷贝普通文件时，使用 COPY ，如果拷贝的是 url 或者  tar 文件，用 ADD

---



▶ **RUN**

运行指定命令（在**本机shell运行** ）

格式：

​	 **RUN command**               //在 shell 终端中运行命令，即 /bin/sh -c

​	或者

​	 **RUN ["executable","param1","param2"]**      //被解析成 JSON 数组，使用 exec 执行，不会启动 shell环境

> 指定 **使用其他终端** 类型可以通过第二种方式，例如 RUN ["/bin/bash", "-c", "echo hello"]



每条 RUN 指令将在当前镜像基础上执行指定命令，**并提交未新的镜像层**，当命令较长时，使用 \ 来换行

---



### 8.4 容器启动时执行的命令

容器启动后执行的命令 一般 为 **CMD** 和 **ENTRYPOINT**

在 Dockerfile 中一般写在最后

格式：

​	 **CMD / ENTRYPOINT command 参数1 参数2**     // 在 shell 中执行

或者

​	 **CMD / ENTRYPOINT  ["executable","param1","param2"]**          //使用 exec 执行

每个 Dockerfile 只能有一条 CMD / ENTRYPOINT 命令，如果指定了多条命令，只有最后一条会被执行

---



### 创建镜像

格式

​	 **docker build -t 仓库名：标签名 URL**

URL 是 Dockerfile 文件所在目录，该命令将读取指定URL下的Dockerfile，并将该路径下 **所有数据** 作为上下文发送给 Docker 服务器

---



### 选择父镜像

​	大部分情况下，生成新的镜像都需要通过  FROM 指令来指定父镜像。父镜像是生成镜像的基础，会直接影响到所生成镜像的大小和功能

 	用户可以选择 **两种镜像** 作为 **父镜像**：

​	 **1 基础镜像**  ： baseimage，基础镜像比较特殊，其 Dockerfile 中往往不用 FROM 命令，或者基于 scratch 镜像（FROM scratch），这意味着整个 **镜像树**中处于根的位置（**scratch 指 从零开始 的意思**）

​	 **2 普通镜像**  ： 往往有第三方创建，基于基础镜像，常见的有 ：

​		busybox      ( 1.2M )     **推荐使用**

​		alpine          ( 4.4M )

​		ubuntu        ( 80M )

​		debian         ( 100M )

​		

### 从一个简单的 Dockfile 开始创建一个镜像并启动容器

1 编写 Dockerfile 文件

```
FROM busybox

RUN echo helleword

```

2  在Dockerfile目录，运行 **docker build -t buyboxtest:test . **



3 运行 **docker run -i -d 镜像ID**       ,启动容器



4 **进入容器 docker exec -it 容器ID sh**



5 退出容器 exit 



