# 镜像
## 创建镜像
创建镜像的方法主要有三种： 基于已有镜像的容器创建、 基千本地模板导入、 基于Dockerfile 创建。  
1. 基于已有容器创建  
  该方法主要是使用 docker [container] commit命令。命令格式为 docker [container] commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
2. 基于本地模板导入
  用户也可以直接从一个操作系统模板文件导人一个镜像，主要使用 docker [container] import 命令。 命令格式为 docker [image] import [OPTIONS] file|URL|-[REPOSITORY[:TAG ]]  
3. 基于 Dockerfile 创建
基于 Dockerfile 创建是最常见的方式。Dockerfile 是一个文本文件，利用给定的指令描述基于某个父镜像创建新镜像的过程。
## 存出和载人镜像
用户可以使用 docker [image] save 和 docker [image] load 命令来存出和载人镜像。
1. 存出镜像
  如果要导出镜像到本地文件，可以使用 docker [image] save 命令 。 该命令支持-o、－output string 参数，导出镜像到指定的文件中。  
2. 载入镜像
  可以使用docker [image] load 将导出的tar文件再导人到本地镜像库。支持 －Linput string选项，从指定文件中读人镜像内容。  
## 上传镜像
