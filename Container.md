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
  可以使用 docker [container] stop 来终止一个运行中的容器。 该命令的格式为 docker [container] stop [-t | --time [=10]][CONTA工NER...]
  该命令会首先向容器发送SIGTERM信号，等待一段超时时间后（默认为 10 秒），再发送SIGKILL信号来终止容器：$docker stop ce5  
  此时，执行 docker container prune 命令，会自动清除掉所有处于停止状态的容器。  
  此外，还可以通过 docker [container] kill 直接发送 SIGKILL 信号来强行终止容器。  
## 进入容器
在使用-d参数时，容器启动后会进入后台，用户无法看到容器中的信息，也无法进行操作。至于，操作官方推荐attach或者exec  
1. attach命令  
   `docker [container] attach [--detach-keys[=[]]] [--no-stdin] [--sig-proxy[=true]] CONTAINER`  
   三个options:
   - --detach-keys [=[]]: 指定退出 attach 模式的快捷键序列， 默认是CTRL+p,CTRL+q;
   - --no-stdin=true|false: 是否关闭标准输入，默认是保持打开;  
   - --sig-proxy=true|false: 是否代理收到的系统信号给应用进程，默认为 true;  
2. exec命令  
   docker [container] exec [-d|--detach] [--detach-keys[=[]]] [-i|--interactive][--piivileged] [-t|-tty] 
   [-u|--user [=USER]] CONTAINER COMMAND [ARG...]   
   - -d, --detach： 在容器中后台执行命令；
   - --detach-keys＝""：指定将容器切回后台的按键；
   - -e, --env=[]：指定环境变量列表；
   - -i, --interactive=true|false：打开标准输入接受用户输入命令,默认值为false;
   - -privileged=trueifalse:是否给执行命令以高权限，默认值为 false;
   - -t, --tty=trueifalse:分配伪终端，默认值为 false;
   - -u, --user＝＂＂：执行命令的用户名或 ID.
## 删除容器
可以使用 docker [container] rm 命令来删除处于终止或退出状态的容器，命令格式为docker [container] rm [-f | --force]
[-l| --link] [-v | --volumes] CONTAINER [CONTAINER ...]   
选项内容：  
* -f --force=false:是否强行终止并删除一个运行中的容器;
* -l --link=false:删除容器的连接，但保留容器;
* -v --volumes=false:删除容器挂载的数据卷.
默认情况下， docker rm 命令只能删除已经处于终止或退出状态的容器，并不能删除还处于运行状态的容器.  
如果要直接删除一个运行中的容器，可以添加 -f 参数。 Docker会先发送 SIGKILL 信号给容器，终止其中的应用.  
## 导入与导出容器
1. 导出容器  
   导出容器是指，导出一个已经创建的容器到一个文件，不管此时这个容器是否处于运行状态。可以使用docker [container] export命令  
   格式:`docker [container] export [-o|--output[=""]] CONTAINER`  
2. 导入容器  
   导出的文件又可以使用 docker [container] import 命令导人变成镜像，该命令格式为:  
   docker [container] import [-c|--change[=[]]] [-m|--message[=MESSAGE]] file|URL|-[REPOSITORY[:TAG]]   
   比如test_for_run.tar是导出的容器，那么接下来导入容器docker import test_for_run.tar -test/ubuntu:vl.O
   实际上，既可以使用 docker load 命令来导入镜像存储文件到本地镜像库，也可以使用 docker [container] import 命令来导入一个容器快
   照到本地镜像库。这两者的区别在于：容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），
   而镜像存储文件将保存完整记录，体积更大。  
## 查看容器
1. 查看容器详情  
   docker container inspect [OPTIONS] CONTAINER [CONTAINER...]子命令  
2. 查看容器内进程  
   查看容器内进程可以使用 docker [container] top [OPTIONS] CONTAINER [CONTAINER...] 子命令。
   这个子命令类似于 Linux 系统中的 top 命令，会打印出容器内的进程信息，包括 PID、用户、时间、命令等.  
3. 查看统计信息
   查看统计信息可以使用 docker [container] stats [OPTIONS] [CONTAINER...]子命令，会显示 CPU 、内存、存储、网络等使用情况的统计信息.  
   选项如：
   * -a, -all:输出所有容器统计信息，默认仅在运行中；
   * -format string:格式化输出信息；
   * -no-stream：不持续输出，默认会自动更新持续实时结果；
   * -no-trunc:不截断输出信息。
