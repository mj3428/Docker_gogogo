# 访问Docker仓
有时候容易把仓库与注册服务器（Registry）混淆。实际上注册服务器是存放仓库的具体服务器，一个注册服务器上可以有多个仓库，而每个仓库下面可以有多个镜像。  
用docker login登录、注册。本地用户目录下会自动创建.docker/config.json 文件,保存用户的认证信息。  
**基本操作** 用户无须登录即可通过 docker search 命令来查找官方仓库中的镜像，并利用docker [image] pull命令来将它下载到本地.   
**自动创建**  
1 ）创建并登录 Docker Hub ，以及目标网站如 Github;
2 ）在目标网站中允许 Docker Hub 访问服务；
3 ）在 Docker Hub 中配置一个“自动创建”类型的项目；
4 ）选取一个目标网站中的项目（需要含 Dockerfile ）和分支；
5 ）指定 Dockerfile 的位置，并提交创建。
