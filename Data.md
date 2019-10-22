# Docker数据管理
## 数据卷（Data Volumes）
**数据卷（Data Volumes）** 是一个可供容器使用的特殊目录，它将主机操作系统目录直接映射进容器，类似于Linux中的 mount 行为.  
数据卷可以提供很多有用的特性 ：
- 数据卷可以在容器之间共事和重用，容器间传递数据将变得高效与方便；    
- 对数据卷内数据的修改会立马生效，无论是容器内操作还是本地操作；  
- 对数据卷的更新不会影响镜像，解摘开应用和数据 ；  
- 卷会一直存在 ，直到没有容器使用，可以安全地卸载它.

1. 创建数据卷  
Docker提供了volume 子命令来管理数据卷，如下命令可以快速在本地创建一个数据卷：`docker volume create -d local test`
2. 绑定数据卷  
除了使用volume子命令来管理数据卷外，还可以在创建容器时将主机本地的任意路径挂载到容器内作为数据卷，这种形式创建的数据卷称为绑定数据卷。  
在用docker [container] run命令的时候，可以使用 -mount 选项来使用数据卷.  
-mount 选项支持三种类型的数据卷，包括:
- volume:普通数据卷，映射到主机/var/lib/docker/volumes路径下；
- bind:绑定数据卷，映射到主机指定路径下；  
- tmpfs:临时数据卷，只存在于内存中；  

下面使用 training/webapp 镜像创建一个Web容器，并创建一个数据卷挂载到容器的/opt/webapp 目录：  
`docker run -d -P --name web --mount type=bind,source=/webapp,destination=/opt/webapp training/webapp python app.py`  
上述命令等同于使用旧的 -v标记可以在容器内创建一个数据卷：  
`docker run -d -P --name web -v /webapp:/opt/webapp training/webapp python app.py`  
这个功能在进行应用测试的时候十分方便，比如用户可以放置一些程序或数据到本地目录中实时进行更新，然后在容器内运行和使用。  
另外，本地目录的路径必须是绝对路径，容器内路径可以为相对路径。 如果目录不存在，Docker 会自动创建。  
Docker 挂载数据卷的默认权限是读写（rw） ，用户也可以通过ro指定为只读：
docker run -d -P --name web -v /webapp: /opt/webapp:ro training/webapp python app.py加了：ro之后，容器内对所挂载数据卷内的数据就无法修改了.  
## 数据卷容器
如果用户需要在多个容器之间共享一些持续更新的数据，最简单的方式是使用数据卷容器。数据卷容器也是一个容器，但是它的目的是专门提供数据卷给其他容器挂载。  
首先，创建一个数据卷容器dbdata，并在其中创建一个数据卷挂载到/dbdata:
`docker run it -v /dbdata --name dbdata ubuntu`  
然后，可以在其他容器中使用--volumes-from来挂载dbdata容器中的数据卷，例如创建dbl和db2两个容器，并从dbdata容器挂载数据卷：  
```
$docker run -it --volumes-from dbdata -name dbl ubuntu
$docker run -it --volumes-from dbdata -name db2 ubuntu
```
此时，容器dbl和db2都挂载同一个数据卷到相同的/dbdata 目录，三个容器任何一方在该目录下的写入，其他容器都可以看到。  
可以多次使用--volumes-from参数来从多个容器挂载多个数据卷，还可以从其他已经挂载了容器卷的容器来挂载数据卷：
`docker run -d --name db3 --volumes-from dbl training/postgres`使用--volumes-from参数所挂载数据卷的容器自身并不需要保持在运行状态。  
如果删除了挂载的容器（包括 dbdata、db1和db2），数据卷并不会被自动删除。 如果要删除一个数据卷，必须在删除最后一个还
挂载着它的容器时显式使用 docker rm -v 命令来指定同时删除关联的容器。
## 利用数据卷来迁移数据
1. 备份  
   使用下面的命令来备份 dbdata 数据卷容器内的数据卷:
   `docker run -volumes-from dbdata -v $ (pwd) : /backup --name worker ubuntu tar cvf /backup/backup.tar /dbdata`  
   先利用 ubuntu 镜像创建了一个容器 worker。 使用--volumes-from dbdata 参数来让worker容器挂载dbdata容器的数据卷（即dbdata数据卷）；
   使用-v $(pwd):/backup参数来挂载本地的当前目录到 worker 容器的/backup 目录。
   worker容器启动后，使用 tar cvf /backup/backup.tar /dbdata 命令将/dbdata下内容备份为容器内
   的/backup/backup.tar,即宿主主机当前目录下的backup.tar
2. 恢复  
   如果要恢复数据到一个容器，可以按照下面的操作。首先创建一个带有数据卷的容器 dbdata2:
   `docker run -v /dbdata --name dbdata2 ubuntu /bin/bash`
   然后创建另一个新的容器，挂载 dbdata2 的容器，并使用 untar 解压备份文件到所挂载的容器卷中：
   `docker run --volumes-from dbdata2 -v $(pwd) :/backup busybox tar xvf /backup/backup.tar`
