# 操作Docker容器
## 创建Docker容器
1. 创建Docker容器  
  可以使用 docker create 命令新建一个容器，例如：$docker create -it ubuntu:latest
  使用 docker [container] create 命令新建的容器处于停止状态，可以使用 docker [container] start 命令来启动它。  
  由于容器是整个 Docker 技术拢的核心， create 命令和后续的 run 命令支持的选项都十分复杂。
2. 启动容器  
  使用 docker [container] start 命令来启动一个已经创建的容器。例如，启动刚创建的ubuntu容器：$ docker start af  
  此时，通过docker ps 命令，可以查看到一个运行中的容器：$ docke r ps  
3. 新建井启动窑器
  除了创建容器后通过 start 命令来启动 也可以直接新建并启动容器。所需要的命令主要为 docker [container］run，等价于先执行 docker [container]
  create 命令，再执行docker [container] start 命令。  
  例如，下面的命令输出一个“Hello World”之后容器自动终止：$ docker run ubuntu /bin/echo 'Hello world' 结果Hello world。  
  当利用 docker [container] run 来创建并启动容器时， Docker 在后台运行的标准  
  操作包括：  
    - 检查本地是否存在指定的镜像，不存在就从公有仓库下载；
    - 利用镜像创建一个容器，并启动该容器；
    - 分配一个文件系统给容器，并在只读的镜像层外面挂载一层可读写层 ；
    - 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去；
    - 从网桥的地址池配置一个 IP 地址给容器；
    - 执行用户指定的应用程序；
    - 口执行完毕后容器被自动终止 。
  可以使用 docker container wait CONTAINER [CONTAINER ...]子命令来等待容器退出，并打印退出返回结果。  
4. 守护态运行  
  需要让 Docker 容器在后台以守护态（Daemonized）形式运行。此时，可以通过添加 -d 参数来实现。例如，下面的命令会在后台运行容
  器：$docker run -d ubuntu/bin/sh -c "while true; do echo hello world; sleep l; done"
5. 查看容器输出
  要获取容器的输出信息，可以通过 docker [container] logs命令  
  该命令支持的选项包括：
   - -details ： 打印详细信息；
   - -f, - follow ：持续保持输出；
   - -since string ：输出从某个时间开始的日志；
   - -tail string ： 输出最近的若干日志；
   - -t, - timestamps ： 显示时间戳信息 ；
   - -until string ： 输出某个时间之前的日志。  
  查看某容器的输出可以使用如下命令：$docker logs ce554267d7a4
## 停止容器
1. 暂停容器
  可以使用 docker [container] pause CONTAINER [CONTAINER...] 命令来暂停一个运行中的容器。
  例如，启动一个容器，并将其暂停：
  $docker run --name test --rm -it ubuntu bash
  $docker pause test
  $docker ps
  处于paused状态的容器，可以使用 docker [container] unpause CONTAINER[CONTAINER...] 命令来恢复到运行状态。  
2. 终止容器
  可以使用 docker [container] stop 来终止一个运行中的容器。 该命令的格式为 docker [container] stop [-t I - -time [=10]][CONTA工NER...]
  该命令会首先向容器发送SIGTERM信号，等待一段超时时间后（默认为 10 秒），再发送SIGKILL信号来终止容器：$docker stop ce5  
  此时，执行 docker container prune 命令，会自动清除掉所有处于停止状态的容器。  
  此外，还可以通过 docker [container] kill 直接发送 SIGKILL 信号来强行终止容器。  
