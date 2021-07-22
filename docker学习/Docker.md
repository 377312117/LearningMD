# Docker

## Docker历史

- 2010年诞生 dotCloud   pass云计算服务 LXC相关容器技术
- 容器化技术 命名为Docker
- 2013年 开源   （从此开始火爆起来）
- 2014年 Docker1.0发布

## Docker简介

#### Docker为什么会出现？

- 环境配置麻烦（应用环境，应用配置,跨平台）
- 各个服务器存在差异
- 部署容易出错

#### Docker作用：

- 项目带上环境安装打包
- 开发打包部署上线一套流程做完

#### Docker核心思想与底层原理：

- 来源于集装箱的创意
- 隔离，打包装箱，每个箱子是互相隔离的
- Docker 是一个Client-Sever结构的系统
  - Docker的守护进程运行在主机上，通过Socket访问。
  - DockerServer接收到Dock-Client的指令，就会执行这个命令
- ![image-20210721215942862](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721215942862.png)

#### 编写语言

Golang

#### 官方文档

https://docs.docker.com/

#### 仓库地址DockerHub

https://hub.docker.com

#### Docker优点

- 应用更快的交付和部署,打包镜像发布测试，一键运行
- 更便捷的升级和扩容,项目打包成镜像,通过docker compose,docker Swarm, k8s编排容器即可快速集群化部署
- 更简单的系统运维
- 在容器化后，我们的开发测试，发布环境高度一致
- 更高效的计算资源利用
- Docker是内核级的虚拟化，可以在一个物理机上运行 很多的容器实例

#### 关键概念

##### 镜像(image)

好比一个模板，通过模板创建容器服务

##### 容器(container)

简易的linux系统

##### 仓库(repository)

存放镜像的地方，分共有仓库和私有仓库，阿里云 容器服务器（配置镜像加速）

#### 系统要求

```shell
uname -r # 查看linux内核版本， 安装docker需内核3.10以上
```



## 虚拟机（如Vmware）   docker 差异

​	docker  轻巧，虚拟机相当于使用了固定的资源创建了一台真实的电脑

### 形象解释

物理机

![image-20210721235233492](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721235233492.png)

虚拟机

![image-20210721235247589](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721235247589.png)

容器

![image-20210721235258826](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721235258826.png)

### 专业解释

**理解虚拟机**

使用**虚拟机**运行多个相互隔离的应用时，如下图:

<img src="C:\Users\Administrator\Documents\工作文档\容器.jpg" alt="容器" style="zoom:67%;" />

**从下到上理解上图:**

- **基础设施(Infrastructure)**。它可以是你的**个人电脑**，数据中心的**服务器**，或者是**云主机**。
- **虚拟机管理系统(Hypervisor)**。利用Hypervisor，可以在**主操作系统**之上运行多个不同的**从操作系统**。类型1的Hypervisor有支持MacOS的**HyperKit**，支持Windows的**Hyper-V、Xen**以及**KVM**。类型2的Hypervisor有VirtualBox和VMWare workstation。
- **客户机操作系统(Guest Operating System)**。假设你需要运行3个相互隔离的应用，则需要使用Hypervisor启动3个**客户机操作系统**，也就是3个**虚拟机**。这些虚拟机都非常大，也许有700MB，这就意味着它们将占用2.1GB的磁盘空间。更糟糕的是，它们还会消耗很多CPU和内存。
- **各种依赖。**每一个**客户机操作系统**都需要安装许多依赖。如果你的应用需要连接PostgreSQL的话，则需要安装**libpq-dev**；如果你使用Ruby的话，应该需要安装gems；如果使用其他编程语言，比如Python或者Node.js，都会需要安装对应的依赖库。
- **应用**。安装依赖之后，就可以在各个**客户机操作系统**分别运行应用了，这样各个应用就是相互隔离的。

**理解Docker容器**

使用**Docker容器**运行多个相互隔离的应用时，如下图:

<img src="C:\Users\Administrator\Documents\工作文档\容器.jpg" alt="容器" style="zoom:67%;" />

不难发现，相比于**虚拟机**，**Docker**要简洁很多。因为我们不需要运行一个臃肿的**客户机操作系统**了。

**从下到上理解上图:**

- **基础设施(Infrastructure)**。
- **主操作系统(Host Operating System)**。所有主流的Linux发行版都可以运行Docker。对于MacOS和Windows，也有一些办法”运行”Docker。
- **Docker守护进程(Docker Daemon)**。Docker守护进程取代了Hypervisor，它是运行在操作系统之上的后台进程，负责管理Docker容器。
- **各种依赖**。对于Docker，应用的所有依赖都打包在**Docker镜像**中，**Docker容器**是基于**Docker镜像**创建的。
- **应用**。应用的源代码与它的依赖都打包在**Docker镜像**中，不同的应用需要不同的**Docker镜像**。不同的应用运行在不同的**Docker容器**中，它们是相互隔离的。

### **对比虚拟机与Docker**

**Docker守护进程**可以直接与**主操作系统**进行通信，为各个**Docker容器**分配资源；它还可以将容器与**主操作系统**隔离，并将各个容器互相隔离。**虚拟机**启动需要数分钟，而**Docker容器**可以在数毫秒内启动。由于没有臃肿的**从操作系统**，Docker可以节省大量的磁盘空间以及其他系统资源。

说了这么多Docker的优势，大家也没有必要完全否定**虚拟机**技术，因为两者有不同的使用场景。**虚拟机**更擅长于彻底隔离整个运行环境。例如，云服务提供商通常采用虚拟机技术隔离不同的用户。而**Docker**通常用于隔离不同的应用，例如**前端**，**后端**以及**数据库**。

**服务器虚拟化** **vs Docker**

**服务器**好比运输码头：拥有场地和各种设备（服务器硬件资源）

**服务器虚拟化**好比作码头上的仓库：拥有独立的空间堆放各种货物或集装箱

(仓库之间完全独立，独立的应用系统和操作系统）

**Docker**比作集装箱：各种货物的打包

(将各种应用程序和他们所依赖的运行环境打包成标准的容器,容器之间隔离)

Docker有着小巧、迁移部署快速、运行高效等特点，但隔离性比服务器虚拟化差：不同的集装箱属于不同的运单（Docker上运行不同的应用实例），相互独立（隔离）。但由同一个库管人员管理（主机操作系统内核），因此通过库管人员可以看到所有集装箱的相关信息（因为共享操作系统内核，因此相关信息会共享）。

服务器虚拟化就好比在码头上（物理主机及虚拟化层），建立了多个独立的“小码头”—仓库（虚拟机）。其拥有完全独立（隔离）的空间，属于不同的客户（虚拟机所有者）。每个仓库有各自的库管人员（当前虚拟机的操作系统内核），无法管理其它仓库。不存在信息共享的情况

![image-20210721215345291](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721215345291.png)

## docker安装步骤

```shell
#1.卸载
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 2.安装基础                  
 sudo yum install -y yum-utils
# 3. 更换镜像源
 sudo yum-config-manager \
    --add-repo \
    http:// mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
     # 上面阿里云，下面是默认源，都可以
    https://download.docker.com/linux/centos/docker-ce.repo
   
# 4. 安装yum软件包索引  
yum makecache fast
# 5. 安装docker
 yum install docker-ce docker-ce-cli containerd.io
 # 6.启动docker
 systemctl start docker
 # 7.查看是否安装成功
 docker version
 # 8. 创建并运行hello-world镜像
 docker run hello-world
 # 9.查看docker镜像
 docker images
 
 
 # 10.卸载docker
 remove docker-ce docker-ce-cli containerd.io
 # 11.删除资源
 rm -rf /var/lib/docker(docker默认工作路径)
 
 
 # 12.镜像加速
 #腾讯云
 	https://cloud.tencent.com/developer/article/1335788?from=14588
 #阿里云
 	https://www.cnblogs.com/allenjing/p/12575972.html
```

## docker基本命令

![image-20210721220014801](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220014801.png)

### docker的帮助文档

```shell
docs.docker.com
```

### docker帮助命令

```shell
# 查看版本 
docker version
# 系统信息， 包括docker的镜像数
docker info
# docker命令帮助
docker <命令> --help
```

### 镜像命令

```shell
docker images 
[root@VM-0-6-centos system]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   4 months ago   13.3kB

#解释：
REPOSITORY: 镜像仓库源
TAG:        镜像的标签
IMAGE ID :  镜像id
CREATED:    创建时间
SIZE:       镜像大小

# 选项
  -a, --all             显示所有镜像
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           只显示镜像的id

```

### 镜像搜索命令

```shell
docker search <镜像名>

# 参数
--filter 例如： docker search mysql --filter=STARS=3000 筛选星星大于3000的镜像

```

### 拉取镜像

```shell
docker pull <镜像>[:tag]  # tag可忽略，默认下载latest，需该镜像包含的tag，不然下载失败
# 解释
[root@VM-0-6-centos system]# docker pull mysql
Using default tag: latest                  #下载的tag
latest: Pulling from library/mysql  #分层下载，同样的不需重复下载
b4d181a07f80: Pull complete
a462b60610f5: Pull complete
578fafb77ab8: Pull complete
524046006037: Pull complete
d0cbe54c8855: Pull complete
aa18e05cc46d: Pull complete
32ca814c833f: Pull complete
9ecc8abdb7f5: Pull complete
ad042b682e0f: Pull complete
71d327c6bb78: Pull complete
165d1d10a3fa: Pull complete
2f40c47d0626: Pull complete
Digest: sha256:52b8406e4c32b8cf0557f1b74517e14c5393aff5cf0384eff62d9e81f4985d4b  # 唯一标识
Status: Downloaded newer image for mysql:latest    
docker.io/library/mysql:latest     # 源地址

```

### 删除镜像

```shell
docker rmi -f <镜像id>
# 全部删除, 组合命令
docker rmi -f $(docker images -aq)
```

## 容器命令

### 新建容器启动

![image-20210721215643351](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721215643351.png)

```shell
docker run [可选参数] image
# 常用参数说明，全部命令可用--help查看
--name="Name" 容器名字， tomcat01, tomcat02 用来区分容器
-d  后台启动
-it 使用交互方式运行，进入容器内部查看内容
-p 指定容器端口 -p 8080:8080
	-p ip:主机端口：容器端口
	-p 主机端口：容器端口（常用）
	-p 容器端口
-P 随机指定端口

#测试
[root@VM-0-6-centos system]# docker run -it centos /bin/bash
[root@193f5039ae76 /]# # 查看容器内部的centos
```

### 查看正在运行的容器

```shell
docker ps <参数>
	#参数
	-a 列出正在运行的容器和历史运行过的容器
	-n=? 列表近？小时中运行过的容器
	-q 只显示镜像id
```

### 退出容器

```shell
exit 停止并退出
Ctrl + P + Q 不停止退出
```

### 删除容器

```shell
docker rm 容器id  #删除指定容器. 不能删除正在运行的容器，强制删除 rm -f
docker rm -d $(docker ps -aq)  #删除所有容器
docker ps -a |xargs docker rm # 删除所有容器
```

### 启动和停止容器操作

```shell
docker start <容器id>   
docker restart <容器id> 
docker stop <容器id> 
docker kill <容器id> 
```

## 其他常用命令

### 后台启用容器

```shell
docker run -d 镜像名

[root@VM-0-6-centos system]# docker run -d centos                                             
9ec60056eade6dc7c8a5aff08e818020d8445288287f368d98efc485e0e62e32                              
[root@VM-0-6-centos system]# docker ps                                                        
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES                                                                  注意： 如果后台运行，需要有一个前台进程，如无应用则会自动停止     
# 扩展
docker run -d centos /bin/bash -c <sheel脚本>  （可执行一个shell脚本）
```

### 显示容器日志

```shell
docker logs 
	#常用参数
	-f 追踪日志输出 follow log output
		--tail n 与tail命令类似
	-t 显示时间戳 show timestamp
```

### 查看容器中的进程命令

```shell
docker top <容器id>
```

### 查看镜像原数据

```shell
docker inspect <容器id>
```

### 进入当前正在运行的容器

```shell
docker exec -it <容器id> bashShell
docker attach  <容器id>
# exec  进入容器后开启一个新的终端session
# attach 进入正在执行的终端，不会启动新的进程
```

### 从容器内拷贝到主机上

```shell
在主机上执行命令： docker cp <容器id><:文件路径> <主机路径>  # 无论这个容器是否正在运行
```

### 命令汇总

```shell
attach    Attach to a running container #当前she11下attach 连接指定运行镜像
build     Build an image from a Dockerfile #通过Dockerfile 定制镜像
commit    Create a new image from a container changes #提交当前容器为新的镜像
cp Copy   files/folders from the containers filesystem to the host path #从容器中拷贝指定文件或者目录到宿主机中
create    Create a new container #创建一个新的容器，同run，但不启动容器
diff      Inspect changes on a container's fi lesystem #查看docker容器变化
events    Get rea1 time events from the server #从docker服务获取容器实时事件
exec      Run a command in an existing container #在已存在的容器上运行命令
export    Stream the contents of a container as a tar archive #导出容器的内容流作为一个tar归档文件[对应import ]
history   Show the history of an image #展示一个镜像形成历史
images    List images #列出系统当前镜像
import    Create a new filesystem image from the contents of a tarba11 #从tar包 中的内容创建一个新的文件系统映像[对应export]
info      Disp1ay sys tem-wide information #显示系统相关信息
inspect   Return 1ow-leve1 information on a container #查看容器详细信息
kill      Kill a running container  # ki11 指定docker 容器
load      Load an image from a tar archive #从一个tar包中加载-一个镜像[对应save] 
login     Register or Login to the docker registry server  #注册或者登陆一个docker源服务器
logout    Log out from a Docker registry server    #从当前Docker registry 退出
1ogs      Fetch the logs of a container     #输出当前容器日志信息
port      Lookup the pub1ic- facing port which is NAT-ed to PRIVATE_ PORT  #查看映射端口对应的容器内部源端
pause     Pause all processes within a container  #暂停容器
PS        List containers   #列出容器列表
pull      Pu11 an image or a repository from the docker registry server  #从docker镜像源服务器拉取指定镜像或者库镜像
push      Push an image or a repository to the docker registry server    #推送指定镜像或者库镜像至docker源服务器
restart   Restart a running container  #重启运行的容器
rm        Remove one or more containers  #移除一个或者多个容器
rmi       Remove one or more i mages   #移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器
run       Run a command in a new container  #创建一个新的容器并运行一个命令
save      Save an image to a tar archive   #保存一个镜像为一个tar包[对应load]
search    Search for an image on the Docker Hub  #在dockerhub中搜索镜像
start     Start a stopped containers  #启动容器
stop      Stop a running containers  #停止容器
tag       Tag an image into a repository  #给源中镜像打标签
top       Lookup the running processes of a container  #查看容器中运行的进程信息
unpause   Unpause a paused container  #取消暂停容器
version   Show the docker version informati on  #查看docker 版本号
wait      B1ock until a container stops， then print its exit code    #截取容器停止时的退出状态值
```

### 示例

#### 示例1： 部署nginx

![image-20210721220214252](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220214252.png)

```shell
docker pull nginx
docker run -d --name nginx01 -p 3344:80 nginx  # 3344是外部暴露的端口好，80容器内部运行的nginx的端口号.nginx01是对容器的命名
# 进入容器内部查看
docker ps
docker exec -it <nginx01容器id> /bin/bash
# 注： 更改nginx配置文件可以在容器外部提供一个映射路径，达到容器修改文件名，容器内部可以自动修改
```

#### 示例2： 部署Tomcat

```shell
docker run -it --rm tomcat:9.0 # 官方提供的用完即删模式 
docker pull tomcat
docker run -d --name tomcat01 -p 3344:80 tomcat
# 测试访问没有问题，但是进入容器后少了linux 命令,是因为阿里云镜像的原因，默认最小镜像，将所有不必要的都剔除了，保证了最小可运行环境

拓展：tomcat和nginx的区别：https://www.zhihu.com/question/32212996
```

#### 示例3： 部署ES

```shell
# es暴露端口多
# 十分耗内存
#es的数据需要放置到安全目录挂载
# 网络配置 --net spmework

docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:tag

# 启动后卡顿：增加限制内存配置 -e ES_JAVA_OPTS="-Xms64m -Xmx512m"

```

### portainer  docker可视化面板（最好不要依赖）

```shell
docker run -d -p 8088:9000  --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer 
```

## Docker镜像讲解

### 镜像是什么

镜像是一种轻量级、 可执行的独立软件包,用来打包软件运行环境和基于运行环境开发的软件,它包含运行某个软件所需的所有内
容,包括代码、运行时、库、环境变量和配置文件。
所有的应用,直接打包docker镜像,就可以直接跑起来!

### 如何得到镜像: 

●从远程仓库下载

●拷贝

●自己制作一个镜像DockerFile

### Docker镜像加载原理

#### UnionFS (联合文件系统)

UnionFS (联合文件系统) : Union文件系统( UnionFS)是一种分层、 轻量级并且高性能的文件系统,它支持对文件系统的修改
作为-次提交来一层层的叠加,同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual
filesystem)。Union 文件系统是Docker镜像的基础。镜像可以通过分层来进行继承,基于基础镜像(没有父镜像) , 可以制作各
种具体的应用镜像。
特性: - -次同时加载多个文件系统,但从外面看起来,只能看到一个文件系统,联合加载会把各层文件系统叠加起来,这样最终的
文件系统会包含所有底层的文件和目录

#### Docker镜像加载原理

docker的镜像实际上由一层- -层的文件 系统组成,这种层级的文件系统UnionFS.
bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系
统,在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的,包含boo加载器和内核。当boo加载完成
之后整个内核就都在内存中了,此时内存的使用权已由bootfs转交给内核,此时系统也会卸载bootfs。（耗时长）
rootfs (root file system) , 在bootfs之上。包含的就是典型Linux系统中的/dev, /proc, /bin, /etc等标准目录和文件。rootfs就是
各种不同的操作系统发行版,比如Ubuntu , Centos等等。(耗时短)

### 分层理解

![image-20210721220411389](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220411389.png)

![image-20210721220420103](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220420103.png)

![image-20210721220439225](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220439225.png)

#### 特点

Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部

这一层是我们通常说的容器层，容器以下的都是镜像层

### commit镜像

```shell
# docker commit 提交容器成为一个新的副本
# 命令与git类似
docker commit -a="xxx" -m="add xxx" 容器id 目标镜像名：[tag]
```

## 容器数据卷

### 什么是容器数据卷？

- 数据持久化，同步化，容器内的目录挂载到宿主机上面，容器间也可以数据共享
- ![image-20210721220456864](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220456864.png)

#### 举例1

```shell
docker run -it -v /virus/test:/home centos /bin/bash
# 通过inspect命令查看同步信息
```

![数据容器卷同步](C:\Users\Administrator\Documents\工作文档\docker学习\数据容器卷同步.png)

#### 举例2

```shell
docker run -d -p 3310:3306  -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql_zzx mysql:5.7

注：数据库映射出来了，就算容器被删除也不会删除数据，挂载后宿主机操作映射的路径文件夹和宿主机上的文件夹是双向同步的
```

### 具名挂载和匿名挂载

```shell
# 查看容器卷
docker volume ls
# 匿名卷挂载（不指定名字）
docker run -d -P --name nginx02 -v /etc/nginx nginx
# 展示如下
local     fce4108394fd9f9be393eb61ca19ba32e0420115ffd048e15181f8a5ecba9c10


# 这个是指定的容器卷名字，以nginx为例。启动一个nginx容器，指定容器数据卷名
docker run -d -P --name nginx03 -v juming-nginx:/etc/nginx nginx
# 具名挂载（指定名字）
local     juming-nginx  

# 查看容器数据卷具体的挂载信息
docker volume inspect <容器数据卷名>

# 指定路径挂载（也是匿名挂载）
docker run -d -p 3310:3306  -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql_zzx mysql:5.7


#拓展1：修改读写权限
docker run -d -P --name nginx03 -v juming-nginx:/etc/nginx:ro nginx

ro : read only
rw : 可读可写（默认）
#拓展2: 所有的容器数据卷未指定目录都在/var/lib/docker/volumes/<数据卷名>/_data下
```

### 数据卷容器

![image-20210721220513156](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220513156.png)

![image-20210721220519037](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220519037.png)

#### 利用容器共享数据

```shell
--volumes-from   
#示例：
docker run -it --name docker03 --volumes-from   docker01 zzx_centos:1.0
#共享后，删除原容器，不会丢失数据，是双向复制的
```

#### 示例:多个mysql服务器实现数据共享

```shell
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

docker run -d -p 3310:3306  -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 mysql:5.7 volums-from mysql01 mysql:5.7
```



## DockerFile

也就是构建docker镜像的命令参数脚本，用来构建镜像。镜像是一层一层的，基本的每个命令都是一层构成

- DockerFile 构建文件，定义了一切的 步骤，源代码
- DockerImages：构建生成的镜像，最终的发布的产品
- Docker容器，容器是镜像运行起来后提供的服务器

### 一般流程

```shell
#1. 编写dockerfile

#2. docker build -f DockerFile -t <镜像名>[:tag]  本地路径

#3.docker run 运行

# 4.docker push 发布镜像
```

### Dockfile构建过程

#### 基础知识

1. 每个保留关键字(指令)都是必须是大写字母
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层,并提交!
5. dockerfile是面向开发的，以后发布项目，做镜像就是编写dockerfile文件，一般是企业交付的标准

#### 构成

DockerFile 构建文件，定义了一切的 步骤，源代码

DockerImages：构建生成的镜像，最终的发布的产品

Docker容器，容器是镜像运行起来后提供的服务器

### DockerFile常用指令

![image-20210721220605831](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220605831.png)

```shell
FROM          #基础镜镜像，一切从这里开始构建
MAINTAINER    #镜像是谁写的，姓名+邮箱
RUN           #镜像构建的时候需要运行的命令
ADD           #步骤: tomcat镜像，这个tomcat压缩包!添加内容
WORKDIR       #镜像的工作目录
VOLUME        #挂载的目录
EXPOSE        #保留端口配置
CMD           #指定这个容器启动的时候要运行的命令,只有最后一个会生效，可被替代
ENTRYPOINT    #指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD       #当构建一个被继承DockerFile 这个时候就会运行ONBUILD 的指令。触发指令。
COPY          #类似ADD，将我们文件拷贝到镜像中
ENV           #构建的时候设置环境变量!
```

#### 示例1 : 简单构建centos

```shell
# 创建一个dockerfile，根据centos镜像
FROM centos

# 挂载容器数据卷，匿名挂载
VOLUME ["volume01", "volume02"]

#执行命令
CMD echo  "------end-------"
CMD /bin/bash
```

#### 示例2:构建my_centos

```dockerfile
FROM centos
MAINTAINER zzx<377312117@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 8099

CMD echo $MYPATH
CMD echo "--------END--------"
CMD /bin/bash
```

获得img本地进行的变更历史

```shell
docker history <镜像id>
```

#### 示例3 ：DockerFile构建tomcat镜像

```dockerfile
FROM centos
MAINTAINER zzx<377312117@qq. com>

COPY readme. txt /usr/1ocal/readme. txt

ADD jdk-8u11-Linux-x64. tar.gz /usr/local/
ADD apache-tomcat-9. 0.22. tar.gz /usr/local/

RUN yum -y install vim 
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_ HOME /usr/1oca1/jdk1.8.0 _11
ENV CLASSPATH $JAVA HOME/lib/dt. jar:$JAVA_ HOME/lib/too1s. jar
ENV CATALINA_ HOME /usr/local/apache-tomcat-9.0. 22
ENV CATALINA BASH /usr/local/apache-tomcat-9.0.22
ENV PATH $PATH: $JAVA_ HOME/bin: $CATAL INA_ HOME/lib: $CATAL INA_ HOME/bin
EXPOSE 8080
CMD /usr/local/apache-tomcat-9. 0.22/bin/startup.sh && tail -F /url/local/apache-tomcat-
9.0.22/bin/logs/catalina.out
```

### 在DockerHub发布自己的镜像

1.注册账号

2.地址：hub.docker.com

3.在服务器提交自己的镜像（push）

```
docker login -u qe377312117
enter password>
# 新增一个tag,自己发布的最好都需要打上tag，镜像前需要带上自己的account
docker tag 55ea0baa3722 qe377312117/zzx_centos:1.0

docker push <作者名>/<镜像名>[:tag]
```

## 容器互联

### Docker0网络详解

1. 我们每启动一个docker容器, docker就会给docker容器分配一个ip ,我们只要安装了docker ,就会有-个网卡docker0
2. 桥接模式,使用的技术是evth-pair技术!
3. 容器和容器之间是可以互相ping通的!
4. docker 网络接口都是虚拟的,转发效率高，容器删除后，对应的网桥一对都没了
5. ![image-20210721220642506](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220642506.png)

```shell
ip addr 查看docker0网络地址

# 使用的evth-pair， 充当一个桥梁，连接各种虚拟网络设备
# openStack, Docker容器之间的连接，oVS的连接，都是使用evth-pair 技术
# docker0问题：不支持容器名链接访问
```

### --link（已经较少使用）

​	解决容器间网络联通问题

```shell
# 单项联通 ，在下面的语句执行后03 可以联通02，而反向不可以
docker run -d  -P --name tomcat03 --link tomcat02 tomcat
docker exec -it tomcat03 ping tomcat02
# 若如此做，会修改03中的/etc/hosts 添加02的映射
```

### 自定义网络

查看所有的docker网络 

```shell
docker network ps
```

#### 网络模式

bridge: 桥接docker(默认)

none:不配置网络

host:和宿主共享网络

container:和容器互联（用得少，局限较大）

#### 帮助文档

```shell
docker network --help
```

#### 一般流程：（举例说明）

```shell
# 我们直接启动的命令 --net bridge,使用的是docker0
docker run -d -P --name tomcat01 --net bridge tomcat

#docker0特点 默认，域名不能访问，--link可以打通

# 自定义网络，示例：
docker network create --driver bridge --subnet 192.168.0.1/16 --gateway 192.168.0.1 mynet
#查看自定义网络
docker network inspect
# 创建容器时,添加--net <网络名>，即可保证网络中的容器互联
docker run -d -P --name tomcat-net-04 --net mynet tomcat
```

自定义网络的优点：不同的集群使用不同的网络，保证集群是安全健康的

### 网络连通

测试打通自定义网络和docker0

![](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721221503205.png)

两个网段如何连接tomcat01

解决方法： 连通之后将tomcat01放到了mynet下，connect方法

一个容器两个网段的IP

```shell
docker network connect mynet tomcat02
# 使用docker network inspect mynet可看到，已经将docker0网络中的容器timcat01加入到了container中
```

![image-20210721221708485](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721221708485.png)

### redis实战

![image-20210721222110269](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721222110269.png)

```shell
docker network create redis --subnet 172.38.0.0/16

# 创建配置文件
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

#创建redis节点
for port in $(seq 1 6); \
do \
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf; 
done

#创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379  172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
```



## 拓展：Docker Compose

### 定义

 		使用 Docker Compose 可以轻松、高效的管理容器，它是一个用于定义和运行多容器 Docker 的应用程序工具

### 安装

- 安装 Docker Compose 可以通过下面命令自动下载适应版本的 Compose，并为安装脚本添加执行权限

  ```shell
  sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose
  
  # 查看是否安装成功
  docker-compose -v
  ```

### 工程、服务、容器

- Docker Compose 将所管理的容器分为三层，分别是工程（project）、服务（service）、容器（container）
- Docker Compose 运行目录下的所有文件（docker-compose.yml）组成一个工程,一个工程包含多个服务，每个服务中定义了容器运行的镜像、参数、依赖，一个服务可包括多个容器实例

### 常用命令

```shell
docker-compose ps # 列出所有运行容器
docker-compose logs # 查看日志输出
docker-compose port eureka 8761  # port：打印绑定的公共端口，左边命令可以输出 eureka 服务 8761 端口所绑定的公共端口
docker-compose build    # build：构建或者重新构建服务
docker-compose start eureka
docker-compose stop eureka
docker-compose rm eureka
docker-compose up    # build：构建、启动容器
docker-compose kill eureka
docker-compose scale user=3 movie=3  # pull：下载服务镜像, scale：设置指定服务运气容器的个数，以 service=num 形式指定
docker-compose run web bash # 在一个服务上执行一个命令
```

### docker-compose.yml 属性

version：指定 docker-compose.yml 文件的写法格式
services：多个容器集合
build：配置构建时，Compose 会利用它自动构建镜像，该值可以是一个路径，也可以是一个对象，用于指定 Dockerfile 参数

```xml
build: ./dir
---------------
build:
    context: ./dir
    dockerfile: Dockerfile
    args:
        buildno: 1
```

- **command**：覆盖容器启动后默认执行的命令

```xml
command: bundle exec thin -p 3000
----------------------------------
command: [bundle,exec,thin,-p,3000]
```

- **dns**：配置 dns 服务器，可以是一个值或列表

```xml
dns: 8.8.8.8
------------
dns:
    - 8.8.8.8
    - 9.9.9.9
```

- **dns_search**：配置 DNS 搜索域，可以是一个值或列表
- **environment**：环境变量配置，可以用数组或字典两种方式
- **env_file**：从文件中获取环境变量，可以指定一个文件路径或路径列表，其优先级低于 environment 指定的环境变量
- **expose**：暴露端口，只将端口暴露给连接的服务，而不暴露给主机
- **image**：指定服务所使用的镜像
- **network_mode**：设置网络模式
- **ports**：对外暴露的端口定义，和 expose 对应
- **links**：将指定容器连接到当前连接，可以设置别名，避免ip方式导致的容器重启动态改变的无法连接情况
- **volumes**：卷挂载路径
- **logs**：日志输出信息

### 更新容器

- 当服务的配置发生更改时，可使用 docker-compose up 命令更新配置
- 此时，Compose 会删除旧容器并创建新容器，新容器会以不同的 IP 地址加入网络，名称保持不变，任何指向旧容起的连接都会被关闭，重新找到新容器并连接上去

### links

- 服务之间可以使用服务名称相互访问，links 允许定义一个别名，从而使用该别名访问其它服务

```bash
version: '2'
services:
    web:
        build: .
        links:
            - "db:database"
    db:
        image: postgres
```

- 这样 Web 服务就可以使用 db 或 database 作为 hostname 访问 db 服务了

### 简单示例

- 打包项目，获得 jar 包 docker-demo-0.0.1-SNAPSHOT.jar

```go
mvn clean package
```

- 在 jar 包所在路径创建 Dockerfile 文件，添加以下内容

```bash
FROM java:8
VOLUME /tmp
ADD docker-demo-0.0.1-SNAPSHOT.jar app.jar
RUN bash -c 'touch /app.jar'
EXPOSE 9000
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","app.jar"]
```

- 在 jar 包所在路径创建文件 docker-compose.yml，添加以下内容

```bash
version: '2' # 表示该 Docker-Compose 文件使用的是 Version 2 file
services:
  docker-demo:  # 指定服务名称
    build: .  # 指定 Dockerfile 所在路径
    ports:    # 指定端口映射
      - "9000:8761"
```

- 在 docker-compose.yml 所在路径下执行该命令 Compose 就会自动构建镜像并使用镜像启动容器

```cpp
docker-compose up
docker-compose up -d  # 后台启动并运行容器
```

- 访问 http://localhost:9000/hello 即可访问微服务接口。

## 拓展2： Docker Swarm

　 Swarm是Docker公司推出的用来管理docker集群的平台，几乎全部用GO语言来完成的开发的，代码开源在https://github.com/docker/swarm， 它是将一群Docker宿主机变成一个单一的虚拟主机，Swarm使用标准的Docker API接口作为其前端的访问入口，换言之，各种形式的Docker

​    Client(compose,docker-py等)均可以直接与Swarm通信，甚至Docker本身都可以很容易的与Swarm集成，这大大方便了用户将原本基于单节点的系统移植到Swarm上，同时Swarm内置了对Docker网络插件的支持，用户也很容易的部署跨主机的容器集群服务。

　Swarm deamon只是一个调度器(Scheduler)加路由器(router),Swarm自己不运行容器，它只是接受Docker客户端发来的请求，调度适合的节点来运行容器，这就意味着，即使Swarm由于某些原因挂掉了，集群中的节点也会照常运行，放Swarm重新恢复运行之后，他会收集重建集群信息。

![image-20210721220817286](C:\Users\Administrator\Documents\工作文档\docker学习\image-20210721220817286.png)

### 与Docker compose的区别

​         Docker Swarm 和 Docker Compose 一样，都是 Docker 官方容器编排项目，但不同的是，Docker Compose 是一个在单个服务器或主机上创建多个容器的工具，而 Docker Swarm 则可以在多个服务器或主机上创建容器集群服务，对于微服务的部署，显然 Docker Swarm 会更加适合。

拓展文档：https://www.cnblogs.com/zhujingzhi/p/9792432.html#_label0



### Docker Swarm 与 K8S的对比

##### kubernetes：

​         kubernetes，是Google多年大规模容器管理技术的开源版本，是众多厂商推崇的docker管理优秀之作，随着越来越多的厂商不停地贡献代码，kubernetes功能也愈发完善。

###### kubernetes优点：

​	**1. 管理更趋于完善稳定**

kubernetes 集群管理更趋于完善稳定，同时pod功能上比swarm的service更加强大

​	2. **健康机制完善**

Replication Controllers可以监控并维持容器的生命

​	**3. 轻松应对复杂的网络环境**

kubernetes默认使用Flannel作为overlay网络。

Flannel是CoreOS 团队针对 Kubernetes 设计的一个覆盖网络（OverlayNetwork）工具，其目的在于帮助每一个使用 Kuberentes 的CoreOS 主机拥有一个完整的子网。

###### kubernetes劣势：

​	**1. 配置、搭建稍显复杂，学习成本高**

由于配置复杂，学习成本相对较高，对应运维的成本相对高点

​	**2. 启动速度稍逊**

kubernetes会有五层交互，启动是秒级，启动速度慢于swarm



#### swarm：

​         Swarm是Docker公司在2014年12月初发布的一套用来管理Docker集群的较为简单的工具，由于Swarm使用标准的Docker API接口作为其前端访问入口，所以各种形式的Docker Client(dockerclient in go, docker_py, docker等)都可以直接与Swarm通信。随着Swarm0.2发布，swarm增加了新的策略来调度集群中的容器方式，使得在可用的节点上传播它们，以及支持更多的Docker命令以及集群驱动。

###### **swarm**优点：

​	**1.架构简单，部署运维成本较低**

docker swarm 集群模式由于原生态集成到docker-engine中，所以首先学习成本低，对于使用docker-engine 1.12版本及以上可以平滑过渡，service服务可以满足动态增减容器个数，同时具备自身的负载均衡，swarm管理者多台设定保证了机器在出错后有一个良好的容灾机制

​	**2.启动速度快**

swarm集群只会有两层交互，容器启动是毫秒级

###### swarm劣势：

​	**1. 无法提供更精细的管理**

swarm API兼容docker API，所以使得swarm无法提供集群的更加精细的管理

​	**2. 网络问题**

在网络方面，默认docker容器是通过桥接与NAT和主机外网络通信，这样就出现2个问题，一个是因为是NAT，外部主机无法主动访问到容器内（除了端口映射），另外默认桥接IP是一样的，这样会出现不同主机的容器有相同的IP的情况。这样两容器更加不能通信。同时网络性能方面，有人测试经过桥接的网络性能只有主机网络性能的70%。当然以上问题可以通过其他工具解决，比如用 Flannel 或者 OVS网桥

​	**3. 容器可靠性**

在容器可靠性方面，相较于Kubernetes的Replication Controllers可以监控并维持容器的生命，swarm在启动时刻可以控制容器启动，在启动后，如果容器或者容器主机崩溃，swarm没有机制来保证容器的运行。

