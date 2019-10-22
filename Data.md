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
