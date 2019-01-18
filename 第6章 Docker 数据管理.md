容器中管理数据的主要方式有：

- [ ] **数据卷**（Data Volumes）: 

  容器内数据直接映射到本地主机环境

  一般是将本地的目录或者文件挂载到容器内的数据卷中

- [ ] **数据卷容器**（Data Volumes Containers）: 使用特定容器维护数据卷

  数据卷容器在 **容器和主机、容器和容器** 之间共享数据

  实现数据的备份和恢复

### 6.1 数据卷

数据卷可以提供很多有用的特性：

- 数据卷可以在容器之间共享和重用，容器间传递数据将变得高效与方便
- 对数据卷的修改会立马生效，**无论是容器内操作还是本地操作**
- 对数据卷的更新不会影响镜像
- 卷会一直存在，即使容器被删除，直到没有容器使用，可以安全地卸载它

▶**创建数据卷**

格式：

​	 **docker volume create -d local test**

此时，查看 **/var/lib/docker/volumes** 路径下，会发现所创建的数据卷位置



### 6.2 数据卷容器

首先，创建一个数据卷容器 dbdata , 并在其中创建一个数据挂载到 /dbdata

```
docker run -it -v /dbdata --name dbdata ubuntu
```

然后，可以在其他容器中使用 --volumes-from 来挂载 dbdata 容器中的数据卷

例如创建 db1 和 db2 两个容器，并从 dbdata 容器挂载数据卷

```
docker run -it --volumes-from dbdata --name db1 ubuntu
docker run -it --volumes-from dbdata --name db2 ubuntu
```

