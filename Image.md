# 镜像
## 创建镜像
创建镜像的方法主要有三种： 基于已有镜像的容器创建、 基千本地模板导入、 基于Dockerfile 创建。  
1. 基于已有容器创建  
  该方法主要是使用 docker [container] commit命令。命令格式为 docker [container] commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
  
