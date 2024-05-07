

# **wsl重启**

```shell
# 先查看一下各个虚拟机的运行状态
wsl -l -v
# 关闭指定虚拟机
wsl -t name
# 关闭wsl
wsl --shutdown
# 启动wsl
wsl
```



# Docker概述

## Docker为什么出现

主要解决的问题是：我开发过程中将会用到很多的组件，可以直接使用docker镜像，然后项目部署的时候，直接将docker中的组件与代码程序一起打包部署。这样可以解决环境配置与版本不统一的问题。







![image-20240430130144186](D:/%E4%BD%A0%E5%A5%BDJava/image-20240430130144186.png)

> docker容器可以想象成一个进程
>
> 





![image-20240430130946949](D:/%E4%BD%A0%E5%A5%BDJava/image-20240430130946949.png)

 

## docker常用命令



### 1:帮助启动类命令

· 启动docker：

```
 systemctl start docker
```

· 停止docker：

```
 systemctl stop docker
```

· 重启docker：

```
 systemctl restart docker
```

· 查看docker状态： 

```
systemctl status docker
```

· 开机启动：

```
 systemctl enable docker
```

· 查看docker概要信息： 

```
docker info
```

· 查看docker总体帮助文档： 

```
docker --help
```

· 查看docker命令帮助文档： 

```
docker 具体命令 --help
```



### 2:镜像命令

```
docker images
```

列出本地主机上的镜像，-a：列出本地所有镜像（含历史映像层）；-q：只现实镜像id。
说明：
REPOSITORY：表示镜像的仓库源
TAG：镜像的标签版本号
IMAGE ID：镜像ID
CREATED：镜像创建时间
SIZE：镜像大小

```
docker search (image REPOSITORY)
```

```
docker search ubuntu
```

查询镜像信息，–limit : 只列出N个镜像，默认25个；–limit : 只列出N个镜像，默认25个

```
docker pull (image REPOSITORY) [:TAG]
```

```
docker pull ubuntu:6.2
```

下载镜像，若无 TAG，为最新版本:latest

```
docker system df
```

查看镜像/容器/数据卷所占的空间

```
docker rmi [image]
```

删除镜像，docker rmi -f 镜像名1:TAG 镜像名2:TAG
删除全部，docker rmi -f $(docker images -qa)

**什么是悬挂镜像？**
仓库名、标签都是的镜像。

![image.png](https://img-blog.csdnimg.cn/img_convert/6b46c2842acef3017425ad4a2df0f30c.png#averageHue=#fef4f4&clientId=u47714028-2a0a-4&from=paste&height=61&id=ud5facd39&originHeight=61&originWidth=577&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25027&status=done&style=none&taskId=uc18ab636-b703-4611-8373-5e975a4f80e&title=&width=577)





### 3:容器命令

只有镜像才能创建容器

![image.png](https://img-blog.csdnimg.cn/img_convert/1b71ffbc914b5534c50adbc4727ab2be.png#averageHue=#f0efef&clientId=u47714028-2a0a-4&from=paste&height=294&id=uc5b12b0c&originHeight=294&originWidth=665&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64994&status=done&style=none&taskId=u5d25660d-20e3-4f46-946c-2586f54a1d2&title=&width=665)

**新建并启动容器**

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG…]
```

OPTIONS说明：

```
–-name=“容器别名”
-d：后台运行容器并返回容器 ID，也即启动守护式容器（后台运行）；
-i：以交互模式运行容器，通常与 -t 同时使用；
-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-P: 随机端口映射，大写P
-p: 指定端口映射，小写p
```

![image.png](https://img-blog.csdnimg.cn/img_convert/82a6b54a34e66ab7d85d5e841ae7f3bb.png#averageHue=#d1dcba&clientId=u47714028-2a0a-4&from=paste&height=206&id=u3eae45a3&originHeight=206&originWidth=641&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92333&status=done&style=none&taskId=u0c3e9fd5-8eb8-477c-8bfe-a28f0a1243e&title=&width=641)

举例：

```
docker run -it --name=centos-1 centos:7.1 /bin/bash
```

 。使用镜像centos:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。退出终端，直接输入 exit。

**列出当前所有正在运行的容器**

```
docker ps [OPTIONS]
```

-a: 列出当前所有正在运行的容器和历是运行过的
-l：显示最近创建的容器
-n：显示最近创建的n个容器
-q：静默模式，只显示容器编号

**退出容器**

- exit，退出后容器停止
- ctrl+p+q，容器不停止

**启动已停止运行的容器**

```
docker start 容器id或容器名
```

**重启容器**

```
docker restart 容器id或容器名
```

**停止容器**

```
docker stop 容器id或容器名
```

**强制停止容器**

```
docker kill 容器id或容器名
```

**删除已停止容器**

```
docker rm 容器id

docker rm -f $(docker ps -a -q)  //删除所有的容器
```

**后台运行容器**

```
docker run -d 容器名:版本
```

有的容器后台启动后，会立即自杀，因为他觉得没有事可做，例如：centos、nginx.
解决方式：命令行模式，交互操作。这种特性的容器放到后台是会自杀的，因为放到后台压根发挥不出这种容器的作用。                         

除此，可以后台运行的容器有：
前台运行：docker run -it redis:6.0.8
后台运行：docker run -d redis:6.0.8

虽然redis可以前后台都行，但是还是推荐放到后台，因为前台一个ctrl c就退出了

**查看容器日志：**

```
docker logs 容器id
```

**查看容器内运行的进程**

```
docker top 容器id
```

**查看容器内部细节**

```
docker inspect 容器id
```

**进入正在运行的容器，并以命令行交互**

```
docker exec -it 容器id /bin/shell
```

exec 是在容器中打开新的终端，并且可以启动新的进程 用exit退出，不会导致容器的停止。【推荐】

重新进入

```
docker attach 容器id
```

直接进入容器启动命令终端，不会启动新的进程，用exit退出，会导致容器的停止。

**从容器内拷贝文件到主机上**

```
docker cp 容器ID:容器内路径 目的主机路径
```

```
[terrychen@terrychen ~]$ sudo docker cp 795c161779ba:/etc/adduser.conf  /home/terrychen
```

**导出和导入容器**
导出：export 导出容器的内容留作为一个 tar 归档文件

```
docker export 容器ID > 文件名.tar
```

**导入：从tar包中的内容创建一个新的文件系统再导入为镜像**

```
cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号
```

总结：

![image.png](https://img-blog.csdnimg.cn/img_convert/df9078b3b81a9034f960cf6ee07db9ae.png#averageHue=#2c75a3&clientId=u47714028-2a0a-4&from=paste&height=553&id=u3f6ae296&originHeight=553&originWidth=779&originalType=binary&ratio=1&rotation=0&showTitle=false&size=195357&status=done&style=none&taskId=ucf0a7c53-963b-4318-bfc8-322fd8b8a58&title=&width=779)





## Docker 镜像

### 1：镜像是什么？

镜像是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件。

```shell
首先镜像是一个软件包，是一个非常轻量的可执行的软件包。把应用程序与配置依赖打包成一个可以交付的运行环境（代码，依赖的库，环境变量，配置文件），这个打包好的运行环境就是镜像文件。
```



### 2：UnionFS（联合文件系统）

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

```
unionFS特性：
将多个文件系统叠加在一个统一的虚拟文件系统中，实质上是多个文件系统，但是从外面看是一个文件系统，这种方式可以允许不同的文件系统之间可以共享文件与目录。

unionFS的优点：
可以节省空间，更加的高效

unionFS如何实现镜像瘦身与管理的：
UnionFS允许我们创建镜像时只存储修改部分，而不是整个文件系统。它可以将不同的镜像层叠加在一起，形成一个虚拟文件系统，只需存储每个镜像的差异部分，从而大大减少了磁盘空间的占用。这种轻量化的存储方式使得我们可以更快速、更便捷地创建、分享和更新镜像。
另外，UnionFS还可以实现镜像的分层继承。我们可以基于一个基础镜像创建出多个不同的应用镜像，每个镜像只需要存储自己的差异部分，这样就可以大大减少重复的存储，提高了镜像的复用性和可维护性。
```



### 3：Docker 镜像加载原理：

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

![image.png](https://img-blog.csdnimg.cn/img_convert/f6681dac125481469c637153728e0bab.png#averageHue=#d1d2d0&clientId=u47714028-2a0a-4&from=paste&height=373&id=uca317aa1&originHeight=373&originWidth=549&originalType=binary&ratio=1&rotation=0&showTitle=false&size=202584&status=done&style=none&taskId=uc7718a67-b2fd-4a5a-ac23-53551ebba5b&title=&width=549)

```
bootfs（引导文件系统）就像是计算机的开关和大脑一样，它包含了引导程序和内核。当你启动 Docker 容器时，bootfs 就会被加载到内存中，引导程序会启动内核，然后整个系统就开始运行了。
接着是 rootfs（根文件系统），它就像是计算机的存储空间一样，包含了操作系统中的各种文件和目录，比如 /dev（设备）、/proc（进程）、/bin（二进制文件）等等。不同的 rootfs 就代表了不同的操作系统发行版，比如 Ubuntu、CentOS 等。
```

举个例子，想象一下你正在搭建一个玩具房子。首先你需要一个开关和一颗大脑来控制房子的运行，这就像是 bootfs。然后你需要各种房间和家具来布置房子，这就像是 rootfs，不同的布置方式就代表了不同的操作系统发行版。

### 4：为什么 docker 这么小（几百兆），而CentOS都是好几个G？

一个精简的OS，rootfs 可以很小，只需要最基本的命令、工具和程序库就可以了，因为底层直接用Host 的 kernel，自己只需要提供 rootfs 就行了。

想象一下你在搬家。你可以选择两种方式：一种是把所有家具、装修材料等都一起搬走，另一种是只带走一些必要的衣物和基本生活用品，其他的东西都留在原地。

Docker 和 CentOS 在某种程度上就像两种不同的搬家方式。CentOS 是一个完整的操作系统，就像把整个房子都搬走，包括了各种工具、材料、装饰等。而 Docker 则更像是只带走了一些必要的衣物和生活用品，剩下的家具、装修等都留在原地。

具体来说，Docker 在运行时只需要提供一个最基本的文件系统，称为 rootfs，它包含了一些基本的命令、工具和程序库。而底层的操作系统内核则直接使用宿主机的，不需要再额外提供。这样就大大减少了 Docker 镜像的大小。

举个例子，假设你想运行一个简单的网络应用程序，你可以使用 Docker 镜像来部署它。Docker 镜像只包含了运行该应用程序所需的最基本的文件和程序，因此它的大小很小，只有几百兆。而如果你选择使用 CentOS 来部署这个应用程序，那么你需要下载整个 CentOS 操作系统，它的大小可能是几个G。显然，使用 Docker 可以更快速、更轻量地部署应用程序。

```
docker在运行时，只需要提供一个最基本的文件系统rootfs，这个文件系统包含一些基本的命令，库，工具，至于最底层的操作系统内核，直接使用宿主机的就可以，不需要额外的提供，这样就大大减少了 Docker 镜像的大小。
```



### 5：为什么 Docker 镜像要采用这种分层结构呢？

优点是共享资源，方便复制迁移，为了复用。
比如说有多个镜像都从相同的 base 镜像构建而来，那么 Docker Host 只需在磁盘上保存一份 base 镜像；
同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

注意：Docker镜像层都是只读的，容器层是可写的。

所有对容器的改动 - 无论添加、删除、还是修改文件都只会发生在容器层中。

![image.png](https://img-blog.csdnimg.cn/img_convert/435273e507db3adeddf3c705f8271708.png#averageHue=#4ea6bd&clientId=u47714028-2a0a-4&from=paste&height=279&id=u7476b4c8&originHeight=469&originWidth=557&originalType=binary&ratio=1&rotation=0&showTitle=false&size=247980&status=done&style=none&taskId=ud998639d-0f62-49bf-9dc8-677d7801a60&title=&width=331)

#### Docker 镜像 commit 操作案例

docker commit -m=“提交的描述信息” -a=“作者” 容器ID 要创建的目标镜像名:[标签名]

以 Ubuntu 举例，在 Ubuntu 增加 vim 命令。

```shell
# 容器内更新包管理工具
apt-get update
# 安装 vim
apt-get -y install vim

# 完成后，commit新镜像
docker commit -m"add vim cmd" -a"username" 容器id atguigu/myubuntu:1.1

# 启动新的镜像
docker run -it imageId

```

### 本地镜像发布到阿里云

![image.png](https://img-blog.csdnimg.cn/img_convert/304fe0eefc1ca42cdad206c78bc4baf2.png#averageHue=#f3f3f3&clientId=u47714028-2a0a-4&from=paste&height=601&id=u116f3204&originHeight=601&originWidth=677&originalType=binary&ratio=1&rotation=0&showTitle=false&size=118430&status=done&style=none&taskId=uf7f5f212-59b0-44d1-b327-375b14c1962&title=&width=677)



