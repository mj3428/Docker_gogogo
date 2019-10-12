# 镜像
## 创建镜像
创建镜像的方法主要有三种： 基于已有镜像的容器创建、 基千本地模板导入、 基于Dockerfile 创建。  
1. 基于已有容器创建  
  该方法主要是使用 docker [container] commit命令。命令格式为 docker [container] commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
2. 基于本地模板导入
  用户也可以直接从一个操作系统模板文件导人一个镜像，主要使用 docker [container] import 命令。 命令格式为 docker [image] import [OPTIONS] file|URL|-[REPOSITORY[:TAG ]]
3 . 基于 Dockerfile 创建
基于 Dockerfile 创建是最常见的方式。Dockerfile 是一个文本文件，利用给定的指令描述基于某个父镜像创建新镜像的过程。
