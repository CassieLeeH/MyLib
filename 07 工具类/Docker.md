**devops：**

**编码开发微服务**

**上线部署容器化**

**时时刻刻要监控（可视化工具portainer）**

# 简介

## 为什么需要docker？

开发的项目需要在特定的环境下才能运行，软件移植的过程中，经常出现让人头痛的操作系统、编程语言版本、数据库版本等配置环境不同而导致的各种bug。此时，使用docker这类虚拟化容器技术，可以将整个项目和依赖环境打包在一起，平滑的移植到其他环境中运行。

## 什么是docker？

Docker是基于Go语言实现的云开源项目，是一种解决运行环境和配置问题的软件容器， 方便做持续集成并有助于整体发布的容器虚拟化技术。Docker将应用和依赖环境打包成镜像，只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作，而且Docker容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。

## 容器与虚拟机的区别

虚拟机也是一种带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在Windows10系统里面运行Linux系统CentOS7。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。 但是虚拟机也存在不少缺点：资源占用多 、冗余步骤多 、启动慢。

容器其实可以看做是一个简易版的linux环境，只包含必要的运行环境和依赖项还有应用程序。从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。

Linux 容器不是模拟一个完整的操作系统而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。与虚拟机相比，docker启动快，体积小。

## docker能用来做什么？

## Docker容器有几种状态？

四种状态：运行、已停止、重新启动、已退出

## Docker平台架构图解(架构版)

· 首次懵逼正常，后续深入，先有大概轮廓，混个眼熟

· 整体架构及底层通信原理简述

Docker 是一个 C/S 模式的架构，后端是一个松耦合架构，众多模块各司其职。

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648709293113-c34731be-0907-4a69-b3d7-86f3f5b0da0d.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648703843617-9aa7e721-0b0e-4e64-b1a8-324ba822bae3.png)

## Docker如何在非Linux系统中运行容器？

通过添加到Linux内核版本2.6.24的名称空间功能，可以实现容器的概念。容器将其ID添加到每个进程，

并向每个系统调用添加新的访问控制检查。它由clone（）系统调用访问，该调用允许创建先前全局命名

空间的单独实例。

如果由于Linux内核中可用的功能而可以使用容器，那么显而易见的问题是非Linux系统如何运行容器。

Docker for Mac和Windows都使用Linux VM来运行容器。Docker Toolbox用于在Virtual Box VM中运

行容器。但是，罪行的docker早Windows中使用Hyper-V，在MAC中使用Hypervisor.framework。

# 常用命令

## 帮助启动类命令

· 启动docker： systemctl start docker

· 停止docker： systemctl stop docker

· 重启docker： systemctl restart docker

· 查看docker状态： systemctl status docker

· 开机启动： systemctl enable docker

· 查看docker概要信息： docker info

· 查看docker总体帮助文档： docker --help

· 查看docker命令帮助文档： docker 具体命令 --help

## 镜像命令

· docker images

**· 列出本地主机上的镜像**

· -a :列出本地所有的镜像（含历史映像层）

· -q :只显示镜像ID。

· docker search 某个XXX镜像名字

**· 查找某镜像**

· docker search [OPTIONS] 镜像名字

· OPTIONS说明：

· --limit : 只列出N个镜像，默认25个

· docker search --limit 5 redis

· docker pull 某个XXX镜像名字

**· 下载镜像**

· docker pull 镜像名字[:TAG]

· 没有TAG就是最新版

· 等价于

· docker pull 镜像名字:latest

· docker system df **查看镜像/容器/数据卷所占的空间**

· docker rmi 某个XXX镜像名字ID

**· 删除镜像**

· 删除单个

· docker rmi -f 镜像ID

· 删除多个

· docker rmi -f 镜像名1:TAG 镜像名2:TAG

· 删除全部

· docker rmi -f $(docker images -qa)

sudo docker run IMAGE env **查看镜像支持的环境变量**

## 谈谈docker虚悬镜像是什么？

仓库名、标签都是<none>的镜像，俗称虚悬镜像dangling image

## 容器命令

· **有镜像才能创建容器， 这是根本前提(下载一个CentOS或者ubuntu镜像演示)**

**新建+启动容器** · docker run [OPTIONS] **IMAGE** [COMMAND] [ARG...]

OPTIONS说明（常用）：有些是一个减号，有些是两个减号

--name="容器新名字" 为容器指定一个名称；

-d: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；

-i：以交互模式运行容器，通常与 -t 同时使用；

-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；

也即启动交互式容器(前台有伪终端，等待交互)；

-P: 随机端口映射，大写P

-p: 指定端口映射，小写p

**#使用镜像centos:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。**

docker run -it centos /bin/bash

参数说明：

-i: 交互式操作。

-t: 终端。

centos : centos 镜像。

/bin/bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。

要退出终端，直接输入 exit:

**列出当前所有****正在运行的容器** docker ps [OPTIONS]

OPTIONS说明（常用）：

-a :列出当前所有正在运行的容器+历史上运行过的

-l :显示最近创建的容器。

-n：显示最近n个创建的容器。

-q :静默模式，只显示容器编号。

**退出容器**

· 两种退出方式

**· exit** ：run进去容器，exit退出，**容器停止**

**· ctrl+p+q**：run进去容器，ctrl+p+q退出，**容器不停止**

**启动已停止运行的容器** · docker start 容器ID或者容器名

**重启容器** docker **restart** 容器ID或者容器名

**停止容器** docker **stop** 容器ID或者容器名

**强制停止容器** docker **kill** 容器ID或容器名

**删除已停止的容器** docker **rm** 容器ID

· 一次性删除多个容器实例

· docker rm -f $(docker ps -a -q)

· docker ps -a -q | xargs docker rm

**启动守护式容器(后台服务器)**

· 在大部分的场景下，我们希望 docker 的服务是在后台运行的， 我们可以过 -d 指定容器的后台运行模式。

· docker run -d 容器名

· redis 前后台启动演示case

· 前台交互式启动 docker run **-it** redis:6.0.8

· 后台守护式启动 docker run **-d** redis:6.0.8

**查看容器日志** docker logs 容器ID

**查看容器内运行的进程** docker top 容器ID

**查看容器内部细节** docker inspect 容器ID

**进入正在运行的容器并以命令行交互**

· docker **exec** -it 容器ID bashShell

· 重新进入docker **attach** 容器ID

· **attach** 直接进入容器启动命令的终端，不会启动新的进程 用exit退出，**会导致容器的停止。**

· **exec** 是在容器中打开新的终端，并且可以启动新的进程 用exit退出，**不会导致容器的停止**。

· 推荐大家使用 docker exec 命令，因为退出容器终端，不会导致容器的停止。

· eg:

· 进入redis服务

· docker exec -it 容器ID /bin/bash

· docker exec -it 容器ID redis-cli

· 一般用-d后台启动的程序，再用exec进入对应容器实例

**从容器内拷贝文件到主机上** docker cp 容器ID:容器内路径 目的主机路径

**导入和导出容器**

· export 导出容器的内容留作为一个tar归档文件[对应import命令]

· import 从tar包中的内容创建一个新的文件系统再导入为镜像[对应export]

· 案例

· docker export 容器ID > 文件名.tar

· cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号

## 容器退出后，通过docker ps命令查看不到，数据会丢失么？

容器退出后会处于终止（exited）状态，此时可以通过docker ps -a查看，其中数据不会丢失，还可以通

过docker start来启动，只要删除容器才会清除数据。

# 镜像

Docker中的镜像分层，支持通过扩展现有镜像，创建新的镜像。类似Java继承于一个Base基础类，自己再按需扩展。

新镜像是从 base 镜像一层一层叠加生成的。每安装一个软件，就在现有镜像的基础上增加一层。

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648734535534-df0bdc37-4e90-4585-8f1b-1d7e3a2eaa2e.png)

## 镜像是什么

是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件。

只有通过这个镜像文件才能生成Docker容器实例(类似Java中new出来一个对象)。

镜像是分层的，以我们的pull为例，在下载的过程中我们可以看到docker的镜像好像是在一层一层的在下载

## UnionFS（联合文件系统）

Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

## Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

## 为什么docker镜像要使用分层结构？

镜像分层最大的一个好处就是共享资源，方便复制迁移，就是为了复用。

比如说有多个镜像都从相同的 base 镜像构建而来，那么 Docker Host 只需在磁盘上保存一份 base 镜像；

同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

## Docker镜像层都是只读的，容器层是可写的

当容器启动时，一个新的可写层被加载到镜像的顶部。 这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

所有对容器的改动 - 无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648734405668-d40e1cb7-d7cb-4c69-a92e-c84a7d033450.png)

## 本地的镜像文件都存放在哪里？

于docker相关的本地资源存在/var/lib/docker/目录下，其中container目录存放容器信息，graph目录存

放镜像信息，aufs目录下存放具体的镜像底层文件。

## docker commit提交容器副本使之成为一个新的镜像

· docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]

## 本地镜像发布到阿里云

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648734704583-ec73608f-5df9-425f-be56-ebe2f9704a1c.png)

## 本地镜像发布到私有库

![img](https://cdn.nlark.com/yuque/0/2022/png/26705216/1648735173496-ba164d63-0df5-407b-bd46-03e6c739213f.png)

## 构建docker镜像应该遵循哪些原则？

整体原则上，尽量**保持镜像功能的明确和内容的精简**，要点包括:

尽量选取**满足需求但较小**的基础系统镜像，建议选择debian:wheezy镜像，仅有86MB大小。

**清理**编译生成文件、安装包的缓存等**临时文件**。

安装软件时候要指定准确的版本号，并避免引入不需要的依赖。

从安全的角度考虑，应用尽量使用系统的库和依赖。

使用dockerfile创建镜像时候要添加.dockerignore文件或使用干净的工作目录。

# 容器数据卷

# DockerFile

## DockerFile中最常见的指定是什么?

指令 备注

FROM 指定基础镜像

LABEL 功能为镜像指定标签

RUN 运行指定命令

CMD 容器启动时要运行的命令

## DockerFile中的命令COPY和ADD命令有什么区别？

COPY和ADD的区别时COPY的SRC只能是本地文件，其他用法一致。

## 解释dockerfile的ONBUILD指令？

当镜像用作另一个镜像构建的基础时，ONBUILD指令像镜像添加将在稍后执行的触发指令。如果要构建

将用作构建其他镜像的基础的镜像（例如，可以使用特定于用户的配置自定义的应用程序构建环境或守

护程序），这将非常有用。

# docker Swarm

## 什么是docker Swarm?

Docker Swarm是docker的本地群集。它将docker主机池转变为单个虚拟docker主机。Docjer Swarm

提供标准的docker API，任何已经与docker守护进程通信的工具都可以使用Swarm透明地扩展到多个主

机。

# 微服务

# docker网络

## 启动nginx容器（随机端口映射），并挂载本地文件目录到容器html的命令？

Docker run -d -p --name nginx2 -v /home/nginx:/usr/share/nginx/html nginx

# 监控

## 如何在生产中监控docker？

Docker提供docker:stats和docker事件等工具来监控生产中的docker。我们可以使用这些命令获取重要

统计数据的报告。

Docker统计数据：当我们使用容器ID调用docker stats时，我们获得容器的CPU，内存使用情况等。它

类似于Linux中的top命令。

Docker事件：docker事件是一个命令，用于查看docker守护程序中正在进行的活动流。一些常见的

docker事件是：attach，commit，die，detach，rename，destroy等。我们还可以使用各种选项来限

制或过滤我们感性其的事件。

**19****、如何停止所有正在运行的容器？**

docker kill $(sudo docker ps -q)

**20****、如何清理批量后台停止容器？**

docker rm$(sudo docker ps -a -q)

**22****、很多应用容器都是默认后台运行的，怎么查看他们的**

**输出和日志信息？**

使用docker logs，后面跟容器的名称或者ID信息

**23****、使用docker port命令映射容器的端口时，系统报错**

**Error****：NO public port ‘80’ published for …,是什么意**

**思？**

创建镜像时dockerfile要指定正确的EXPOSE的端口，容器启动时指定PublishAllport=true

**24****、可以在一个容器中同时运行多个应用进程吗？**

一般不推荐在用以容器内运行多个应用进程，如果有类似需求，可以用过额外的进程管理机制，比如

supervisord来管理所运行的进程。

**25、如何控制容器占用系统资源（CPU，内存）的份额？**

在使用docker create命令创建容器或使用docker run 创建并运行容器的时候，可以使用-c|-spu

shares[=0]参数来调整同期使用SPU的权重，使用-m|-memory参数来调整容器使用内存的大小。

**26****、仓库（Repository）、注册服务器（Registry）、注**

**册 索引（****Index）有何关系？**

首先，仓库事存放一组关联镜像的集合，比如同一个应用的不同版本的镜像，注册服务器时存放实际的

镜像的地方，注册索引则负责维护用户的账号、权限、搜索、标签等管理。注册服务器利用注册索引来

实现认证等管理。

**27****、从非官方仓库（如：dl.dockerpool.com）下载镜像**

**的时候，有时候会提示****”Error:Invaild registry endpoint**

**[https://dl.docker.com:5000/v1/](https://dl.docker.com:5000/v1/)****…”?**

Docker自1.3.0版本往后以来，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。

DOCKER_ORTS=”-insecure-registry dl.dockerpool.com:5000”重启docker服务。

**28****、Docker的配置文件放在那里。如何修改配置？**

Ubuntu系统下Docker的配置文件是/etc/default/docker,CentOS系统配置文件存放

在/etc/sysconfig/docker。

**29****、如何更改docker的默认存储设置？**

Docker的默认存放位置是/var/lib/docker，如果希望将docker的本地文件存储到其他分区，可以使用

Linux软连接的方式来做。

**30****、docker与LXC（Linux Container）有何不同？**

LXC利用Linux上相关技术实现容器，docker则在如下的几个方面进行了改进:

容器特性备注

移植性 通过抽象容器配置，容器可以实现一个平台移植到另一个平台

镜像系统基于AUFS的镜像系统为容器的分发带来了很多的便利，通是共同的镜像层只需要存储一份，实

现高效率的存储

版本管理类似于GIT的版本管理理念，用户可以更方便的创建、管理镜像文件

仓库系统仓库系统大大降低了镜像的分发和管理的成本

周边工具各种现有的工具（配置管理、云平台）对docker的支持，以及基于docker的pass、Cl等系统，

让docker的应用更加方便和多样

**31****、Docker于Vagrant有何不同？**

两者的定位完全不同

Vagrant类似于Boot2Docker（一款运行Docker的最小内核），是一套虚拟机的管理环境，Vagrant可

以在多种系统上和虚拟机软件中运行，可以在Windows、Mac等非Linux平台上为Docker支持，自身具

有较好的包装性和移植性。原生Docker自身只能运行在Linux平台上，但启动和运行的性能比虚拟机要

快，往往更适合快速开发和部署应用的场景。

**32****、开发环境中Docker与Vagrant该如何选择？**Docker不是虚拟机，而是进程隔离，对于资源的消耗很少，单一开发环境下Vagrant是虚拟机上的封

装，虚拟机本身会消耗资源.

**33****、如何将一台宿主机的docker环境迁移到另外一台宿主**

**机？**

停止docker服务，将整个docker存储文件复制到另外一太宿主机上，然后调整另外一台宿主机的配置即

可。

**34****、Docker容器创建后，删除了/var/run/netns目录下**

**的网络名字空间文件，可以手动恢复它：**

查看容器进程ID，比如1234

到proc目录下，把对应的网络名字空间文字链接到/var/run/netns，然后通过正常的系统命令查看操作

容器的名字空间

————————————————

1-17题来自原文链接：[https://blog.csdn.net/qq_43286578/article/details/105160725](https://blog.csdn.net/qq_43286578/article/details/105160725)

**35****、什么是Docker镜像？**

答：Docker镜像是Docker容器的源代码。换句话说，Docker镜像用于创建容器。使用build命令创建镜

像，并且在使用run启动时它们将生成容器。镜像存储在Docker注册表中，registry.hub.docker.com因

为它们可能变得非常大，镜像被设计为由其他镜像层组成，允许在通过网络传输镜像时发送最少量的数

据。

**36****、解释基本的Docker使用工作流程是怎样的？**

答：（

1）从Dockerfile开始，Dockerfile是镜像的源代码；（

2）创建Dockerfile后，可以构建它以创建

容器的镜像。图像只是“源代码”的“编译版本”，即Dockerfile；（

3）获得容器的镜像后，应使用注册表

重新分发容器。注册表就像一个git存储库，可以推送和拉取镜像；接下来，可以使用该图像来运行容

器。在许多方面，正在运行的容器与虚拟机（但没有虚拟机管理程序）非常相似。

**37****、什么是docker-compose？**

简单点说就是，docker-compose就是一个编排同时管理多个容器的工具，与它配对使用的是一个

docker-compose.yaml文件，docker-compose命令必须在一个包含docker-compose.yaml文件目录下

才能使用。且当下docker-compose命令只能管理当前目录docker-compose文件中所涉及的容器，安装

在机器上的其他容器无法干扰。docker-compose的大部分命令基本和docker的命令重合，他们唯一的

区别是docker命令能管理机器上所有的容器和镜像文件，而docker-compose只能管理当前docker

compose文件所涉及的容器。

**38****、Docker镜像联合文件系统**

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分成，轻量级并且高性能的文件系统，

他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系

统下。Union文件系统的Docker镜像可以通过分层来进行继承，基于基础镜像，可以制作各种具体的应

用镜像。特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把

各层文件系统进行叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

Sudo docker inspect --format=’{{. State.pid}}’ $container_id 1234**39****、什么类型的应用程序无状态或有状态更适合Docker容**

**器？**

答：最好为Docker Container创建无状态应用程序。我们可以从应用程序中创建一个容器，并从应用程

序中取出可配置的状态参数。现在我们可以在生产环境和具有不同参数的QA环境中运行相同的容器。这

有助于在不同场景中重用相同的镜像。另外，无状态应用程序比有状态应用程序更容易使用Docker容器

进行扩展。

**40****、Docker** **和虚拟机有啥不同？**

Docker 是轻量级的沙盒，在其中运行的只是应用，虚拟机里面还有额外的系统。

**41****、Docker** **安全么？**

Docker 利用了 Linux 内核中很多安全特性来保证不同容器之间的隔离，并且通过签名机制来对镜像进行

验证。大量生产环境的部署证明，Docker 虽然隔离性无法与虚拟机相比，但仍然具有极高的安全性。

**42****、如何清理后台停止的容器？**

可以使用 sudo docker rm $sudo( docker ps -a -q) 命令。

**43****、如何查看镜像支持的环境变量？**

可以使用 docker run IMAGE env 命令。

**44****、当启动容器的时候提示：exec format error？如何解**

**决问题**

检查启动命令是否有可执行权限，进入容器手工运行脚本进行排查。

**45****、本地的镜像文件都存放在哪里？**

与 Docker 相关的本地资源都存放在/var/lib/docker/目录下，其中container目录存放容器信息，graph

目录存放镜像信息，aufs目录下存放具体的内容文件。

**51****、Docker容器退出时是否丢失数据？**

不、当Docker容器退出时，不会丢失数据。应用程序写入磁盘的所有数据都会保留在其容器中直到您

明确删除该容器为止。即使在容器停止后，该容器的文件系统仍然存在