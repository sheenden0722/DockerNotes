Docker 三剑客： Machine、Compose、Swarm

​	 **Docker Machine**：在多种平台上 **快速安装和维护** Docker 运行环境（例如 virtualbox， Microsoft Hyper-V）

​	 **Docker Swarm** : 作为容器集群管理器，提供 Docker 容器集群服务，**基于 Go 语言**。

​	 **Docker Compose**:  多个容器的编排工具，**基于 Python 语言**

这里只介绍 Docker Compose



### Docker Compose

​	编排（Orchestration）功能，是复杂系统是否具有灵活可操作性的关键，编排意味着用户可以灵活地对各种容器资源实现 **定义** 和 **管理**

​	Compose 作为 Docker 官方编排工具，它可以让用户通过 **编写一个简单的模板文件** ，快速地创建和管理基于 Docker 容器的应用集群

​	Compose 地位是 **”定义和运行多个 Docker 容器的应用“**。

​	在日常工作中，经常需要多个容器相互配合来完成某项任务，Compose 允许用户通过一个单独的 docker-compose.yml 模板文件来定义一组相关联的应用容器为一个服务栈（stack）

​	Compose中有几个重要概念；

- [ ] **任务**（task）: 一个容器被称为一个任务。任务拥有独一无二的 ID ,在同一个服务中的多个任务序号一次递增
- [ ] **服务**（service）: 某个相同应用镜像的容器副本集合，一个服务可以横向扩展为多个容器实例
- [ ] **服务栈**（stack）：由多个服务组成，相互配合完成特定业务。



Compose 的默认管理对象是服务栈，通过 **子命令** 对栈中的多个服务进行便捷的生命周期管理

Compose 项目由 **Python** 编写，实现上调用了 Docker 服务提供的 API 来对容器进行管理。

因此，只要操作的平台支持 Docker API ，就可以在其上利用 Compose 来进行编排管理。



### Compose 模板文件

docker-compose.yml

```
version:"3"
services:
	webapp:                     #  服务名称
		image: examples/web     #  容器服务的配置信息
		deploy:
			replicas: 2
			resource:
				limits:
					cpus: "0.1"
					memory: 100M
				restart_policy:
					condition: on-failure
		ports:
			- "80:80"
		networks:
			- mynet
		volumes:
			- "/data"
networks:
	mynet:
```

注意每个服务都必须通过 image 指令指定镜像或 build 指令（需要 Dockerfile）等来 **自动构建生成镜像**

如果使用 build 指令， 在 Dockerfile 中设置的选项（例如：CMD,EXPOSE,VOLUME等）将会自动被获取，无须在 docker-compose.yml 中再次配置