

[TOC]



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

### 6：Docker 镜像 commit 操作案例

```
docker commit -m=“提交的描述信息” -a=“作者” 容器ID 要创建的目标镜像名:[标签名]
```

以 Ubuntu 举例，在 Ubuntu 增加 vim 命令。

```shell
#[terrychen@terrychen ~]$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED        STATUS              PORTS      NAMES
795c161779ba   redis:6.2      "docker-entrypoint.s…"   26 hours ago   Up 26 hours         6379/tcp   redis-1
b73dacfdde8a   ubuntu:20.04   "bash"                    31 hours ago   Up About a minute              ubuntu-1
#[terrychen@terrychen ~]$ sudo docker exec -it b73dacfdde8a /bin/bash
#root@b73dacfdde8a:/# vim a.txt
bash: vim: command not found
#root@b73dacfdde8a:/# apt-get update
#root@b73dacfdde8a:/# apt-get -y install vim
#[terrychen@terrychen ~]$ sudo docker commit -m 'add vim' -a'vimAddUbuntu' b73dacfdde8a vimaddubuntu
#[terrychen@terrychen ~]$ sudo docker images
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
vimaddubuntu   latest    b40d6198c5f5   33 seconds ago   192MB
ubuntu         20.04     2abc4dfd8318   10 days ago      72.8MB
redis          6.2       7734eda5fdc1   4 weeks ago      106MB
#[terrychen@terrychen ~]$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED        STATUS          PORTS      NAMES
795c161779ba   redis:6.2      "docker-entrypoint.s…"   27 hours ago   Up 27 hours     6379/tcp   redis-1
b73dacfdde8a   ubuntu:20.04   "bash"                    32 hours ago   Up 41 minutes              ubuntu-1
```

```
docker commit 命令的主要的功能是：容器要在当前镜像的基础上，可能要加一些东西，可能要配置一些东西，此时这个容器就是我以后想要用的模板，我用docker commit  将我的这个容器直接变成一个模板
```

```
这个地方也是docker镜像的分层,在原镜像的基础上，进行增强之后的镜像叠在原镜像的上面
```



### 7：本地镜像发布到阿里云

![image.png](https://img-blog.csdnimg.cn/img_convert/304fe0eefc1ca42cdad206c78bc4baf2.png#averageHue=#f3f3f3&clientId=u47714028-2a0a-4&from=paste&height=601&id=u116f3204&originHeight=601&originWidth=677&originalType=binary&ratio=1&rotation=0&showTitle=false&size=118430&status=done&style=none&taskId=uf7f5f212-59b0-44d1-b327-375b14c1962&title=&width=677)

阿里云开发者平台配置

https://promotion.aliyun.com/ntms/act/kubernetes.html

登录后，点击镜像搜索

创建仓库镜像

![image.png](https://img-blog.csdnimg.cn/img_convert/92ed9b2c20ee056669d4d5b4baa8f1a4.png#averageHue=#dbd8d3&clientId=u47714028-2a0a-4&from=paste&height=440&id=u1c20a63d&originHeight=440&originWidth=891&originalType=binary&ratio=1&rotation=0&showTitle=false&size=130357&status=done&style=none&taskId=u4ad58600-dcf4-4b91-a75a-33100f8a3e8&title=&width=891)

选择控制台->云服务器ECS->容器镜像服务

选择个人实例

![image.png](https://img-blog.csdnimg.cn/img_convert/3dd0a1ab215bd6c52e524a08c70aa478.png#averageHue=#faf7f4&clientId=u47714028-2a0a-4&from=paste&height=377&id=uce44ad54&originHeight=377&originWidth=879&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105654&status=done&style=none&taskId=u2526902b-03e4-4c3b-978e-0d3e27ed606&title=&width=879)

命令空间

![image.png](https://img-blog.csdnimg.cn/img_convert/abc4b6797bcb1c900b1c94bd7bb80f1a.png#averageHue=#fdfbfa&clientId=u47714028-2a0a-4&from=paste&height=429&id=u1788aa6d&originHeight=429&originWidth=857&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81424&status=done&style=none&taskId=ucc2191fb-ca8e-4843-b469-664706073d0&title=&width=857)

创建命令空间

仓库名称

![image.png](https://img-blog.csdnimg.cn/img_convert/11b7c841cd939b082b86035362fbbc25.png#averageHue=#fdfbfa&clientId=u47714028-2a0a-4&from=paste&height=430&id=ue1c4bfd5&originHeight=430&originWidth=868&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76664&status=done&style=none&taskId=u414b6278-c04f-4970-b909-57c5d718c1f&title=&width=868)

![image.png](https://img-blog.csdnimg.cn/img_convert/9e75de62c674a6bf02adabe14cc79f90.png#averageHue=#dbd8d8&clientId=u47714028-2a0a-4&from=paste&height=383&id=ud2add7f6&originHeight=383&originWidth=863&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83118&status=done&style=none&taskId=u8ee40939-aa81-4ebe-8867-e2d83e7b6b1&title=&width=863)

![image.png](https://img-blog.csdnimg.cn/img_convert/21ca218632330020ac1949bd2488fe1e.png#averageHue=#c4c3c3&clientId=u47714028-2a0a-4&from=paste&height=411&id=u1cf7ec5b&originHeight=411&originWidth=858&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76071&status=done&style=none&taskId=ucd91e69a-4d8a-4b1e-9579-360eb3f7bbc&title=&width=858)

进入管理界面获得脚本

![image.png](https://img-blog.csdnimg.cn/img_convert/b7206f6065552732be0f9f6d27237782.png#averageHue=#fcfbfa&clientId=u47714028-2a0a-4&from=paste&height=266&id=u826d66e0&originHeight=266&originWidth=882&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58914&status=done&style=none&taskId=u30a76477-b4f0-48c5-8ba4-0c6f3c4b8c0&title=&width=882)

将镜像推送到阿里云

![image.png](https://img-blog.csdnimg.cn/img_convert/c507404ed305bc6c56f0426e83f177ca.png#averageHue=#dcb998&clientId=u47714028-2a0a-4&from=paste&height=600&id=u8a09039a&originHeight=600&originWidth=858&originalType=binary&ratio=1&rotation=0&showTitle=false&size=169848&status=done&style=none&taskId=u0efbd58b-7979-4587-85a6-228ce67d850&title=&width=858)

将阿里云上的镜像下载到本地
docker pull registry.cn-hangzhou.aliyuncs.com/atguiguwh/myubuntu:1.1

![image.png](https://img-blog.csdnimg.cn/img_convert/0e474376bd0408bb38f1f4eabb978883.png#averageHue=#fdfbfb&clientId=u47714028-2a0a-4&from=paste&height=283&id=uee3c0f38&originHeight=283&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=152514&status=done&style=none&taskId=u6c318159-834e-429f-9458-ce16c4c7b23&title=&width=727)





### 8：本地镜像发布到私有库

 Dockerhub、阿里云这样的公共镜像仓库可能不太方便，涉及机密的公司不可能提供镜像给公网，所以需要创建一个本地私人仓库供给团队使用，基于公司内部项目构建镜像。

Docker Registry是官方提供的工具，可以用于构建私有镜像仓库

将本地镜像推送到私有库流程：

**1.下载镜像Docker Registry**

```
docker pull registry
```


**2.运行私有库Registry，相当于本地有个私有Docker hub**

```
docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry
```


默认情况，仓库被创建在容器的/var/lib/registry目录下，建议自行用容器卷映射，方便于宿主机联调



**3.案例演示创建一个新镜像，ubuntu安装ifconfig命令**

 3.1 从Hub上下载ubuntu镜像到本地并成功运行

 3.2 原始的Ubuntu镜像是不带着ifconfig命令的

 3.3 外网连通的情况下，安装ifconfig命令并测试通过

```
apt-get update
apt-get install net-tools
```

 3.4 安装完成后，commit我们自己的新镜像

```
docker commit -m="ifconfig cmd add" -a="z" be4779f8w112 zubuntu:1.2
```


 3.5 启动我们的新镜像并和原来的对比

**4.curl验证私服库上有什么镜像**

```
curl -XGET http://192.168.136.148:5000/v2/_catalog
```

```
{"repositories":[]}
```

可以看到，目前私服库没有任何镜像上传过

**5.将新镜像zzyyubuntu:1.2修改符合私服规范的Tag**

```
按照公式： docker   tag   镜像:Tag   Host:Port/Repository:Tag
```

自己host主机IP地址，填写你们自己的，不要粘贴错误
**使用命令 docker tag 将zubuntu:1.2 这个镜像修改为192.168.111.162:5000/zubuntu:1.2**

```
docker tag  zubuntu:1.2  192.168.136.148:5000/zzyyubuntu:1.2
```


**6.修改配置文件使之支持http**

别无脑照着复制，registry-mirrors 配置的是国内阿里提供的镜像加速地址，不用加速的话访问官网的会很慢。
2个配置中间有个逗号 ','别漏了，这个配置是json格式的。
2个配置中间有个逗号 ','别漏了，这个配置是json格式的。
2个配置中间有个逗号 ','别漏了，这个配置是json格式的。

```
vim命令新增如下红色内容：vim /etc/docker/daemon.json
{
“registry-mirrors”: [“https://docker.mirrors.ustc.edu.cn/”],
“insecure-registries”: [“192.168.136.148:5000”]
}…
```

修改完后，建议重启docker

**7.push推送到私服库**

```
docker push 192.168.136.148:5000/zubuntu:1.2
```

**8.curl验证私服库上有什么镜像**

```
curl -XGET http://192.168.136.148:5000/v2/_catalog
```


**9.pull到本地并运行**

```
docker pull 192.168.111.162:5000/zzyyubuntu:1.2

docker run -it 镜像ID /bin/bash
```



## Docker 容器数据卷

> **坑：容器卷记得加入：–privileged=true**
>
> Docker挂载主机目录访问如果出现cannot open directory .: Permission denied
> 解决办法：在挂载目录后多加一个–privileged=true参数即可
>
> 如果是CentOS7安全模块会比之前系统版本加强，不安全的会先禁止，所以目录挂载的情况被默认为不安全的行为，
> 在SELinux里面挂载目录被禁止掉了额，如果要开启，我们一般使用–privileged=true命令，扩大容器的权限解决挂载目录没有权限的问题，也即
> **使用该参数，container内的root拥有真正的root权限，否则，container内的root只是外部的一个普通用户权限。**



### 1：概念：

容器数据卷是一种特殊的目录或文件，它们存在于宿主机上，但可以被 Docker 容器挂载和访问。与容器的其他部分不同，数据卷不受联合文件系统的管理，这使得

它们可以提供持久化存储和共享数据的特性。

简单来说，容器数据卷就像是宿主机上的一个文件夹，但是可以被容器访问和使用。它的功能主要有两个方面：

1. **持久化存储**：容器数据卷的设计目的就是为了数据的持久化。即使容器被删除或重新启动，数据卷上的数据仍然会保留在宿主机上。这样可以确保重要数据不会因为容器的生命周期而丢失。
2. **数据共享**：容器数据卷也可以用于在多个容器之间共享数据。多个容器可以挂载同一个数据卷，从而实现它们之间的数据共享，这在一些场景下非常有用，比如多个容器需要访问相同的配置文件或者日志目录。

举个例子来说明，假设你有一个 Web 应用，它运行在 Docker 容器中。你希望在容器内外都能够访问到应用生成的日志文件，同时希望这些日志文件不会因为容器的重启而丢失。这时，你可以使用容器数据卷来实现这个需求。你可以创建一个数据卷，并将它挂载到容器内的日志目录。这样，应用生成的日志文件就会保存在数据卷上，即使容器被删除或重新启动，日志文件仍然会被保留在宿主机上，从而确保了日志数据的持久化存储。同时，如果你有多个运行同样应用的容器，它们也可以挂载同一个数据卷，从而实现日志数据的共享，方便你对整个系统的监控和管理。

总之，容器数据卷提供了一种方便且持久的方式来管理容器内的数据，同时也为容器之间的数据共享提供了便利。

```
总结：
容器数据卷就是：宿主机上的一个文件夹/目录，可以被容器访问与挂载。数据卷不受unionFS的管理。

容器数据卷的功能是什么：
1：可以实现数据的共享，多个docker容器可以挂载与访问同一个容器数据卷。
2：可以实现容器数据的持久化，容器中的数据保存在容器数据卷中，而容器数据卷又保存在宿主机上，当容器删除时，可以通过宿主机的容器数据卷恢复数据。
```



```
 docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录      镜像名
```

<img src="D:/%E4%BD%A0%E5%A5%BDJava/image-20240508180345536.png" alt="image-20240508180345536" style="zoom:50%;" />

### 2：特点：

1. **数据共享和重用**：数据卷可以在多个容器之间共享和重用数据。这意味着多个容器可以访问同一个数据卷，使得它们之间能够共享数据。
2. **实时生效**：对数据卷中的更改会立即生效，而不需要重新构建镜像或重新启动容器。这意味着你可以在运行的容器中修改数据，而这些修改会立即反映在其他容器中，非常方便。
3. **不包含在镜像更新中**：数据卷中的更改不会影响镜像的更新。这意味着当你更新镜像时，数据卷中的数据不会被修改或覆盖，保证了数据的完整性和持久性。
4. **持续生命周期**：数据卷的生命周期会一直持续到没有容器使用它为止。即使容器被删除，数据卷中的数据仍然会存在，直到你主动删除或停止使用这个数据卷。



举个例子来说明，假设你正在使用 Docker 运行一个基于数据库的应用，比如一个 WordPress 网站。你希望数据库中的数据能够被持久化保存，以防止容器被删除时数据丢失。这时，你可以创建一个数据卷，并将其挂载到数据库容器中的存储目录。这样，无论数据库容器如何变化，数据都会保存在数据卷中，确保了数据的持久性和安全性。同时，如果你需要扩展应用，可以轻松地创建新的容器并挂载同一个数据卷，实现数据的共享和重用，非常方便。

### 3：案例：

宿主vs容器之间映射添加容器卷
直接命令添加：(将docker容器内的数据保存进宿主机的磁盘中)

```
docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录      镜像名
```

 例如：

```
sudo docker run -it --privileged=true -v /temp/host_data:/temp/docker_data ubuntu:20.04
```

宿主机下假如没这个目录，会自己帮你创建

查看数据卷是否挂载成功：

```
docker inspect 容器ID
```

```
"Mounts": [
            {
                "Type": "bind",
                "Source": "/temp/host_data",
                "Destination": "/temp/docker_data",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
```

容器和宿主机之间数据共享：

**1 docker修改，主机同步获得**
**2 主机修改，docker同步获得**
**3 docker容器stop，主机修改，docker容器重启后依旧可以同步**

```shell
[terrychen@terrychen /]$ cd /temp/host_data
[terrychen@terrychen host_data]$ ls
a.txt
[terrychen@terrychen host_data]$ sudo touch b.txt
[terrychen@terrychen host_data]$ ls
a.txt  b.txt
[terrychen@terrychen host_data]$ sudo docker exec -it 8297bde0e38c /bin/bash
root@8297bde0e38c:/# cd /temp/docker_data
root@8297bde0e38c:/temp/docker_data# ls
a.txt  b.txt

[terrychen@terrychen /]$ sudo docker run -d --privileged=true -v /temp/host_data:/temp/redis_data redis:6.2
edd0dfb4b81bbb21a4eff5d93f151a94aca795b21f80d7fe898318fe9f4e8db7
[terrychen@terrychen /]$ sudo docker ps
]CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS      NAMES
edd0dfb4b81b   redis:6.2      "docker-entrypoint.s…"   22 seconds ago   Up 21 seconds   6379/tcp   amazing_heisenberg
8297bde0e38c   ubuntu:20.04   "/bin/bash"               2 hours ago      Up 2 hours                 charming_diffie
[terrychen@terrychen /]$ sudo docker exec -it edd0dfb4b81b /bin/bash
root@edd0dfb4b81b:/data# cd /temp/redis_data
root@edd0dfb4b81b:/temp/redis_data# ls
a.txt  b.txt
```







### 4：读写规则映射添加说明

**读写(默认) rw = read + write ：**

```
docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw      镜像名
```


**只读 ：容器实例内部被限制，只能读取不能写 ro = read only ：**

**此时如果宿主机写入内容，可以同步给容器内，容器可以读取到。**

 

```
docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:ro      镜像名
```



### 5：卷的继承和共享

容器1完成和宿主机的映射

```
docker run -it  --privileged=true -v /mydocker/u:/tmp --name u1 ubuntu
```


容器2继承容器1的卷规则

```
docker run -it  --privileged=true --volumes-from 父类  --name u2 ubuntu
```

```
例如： docker run -it  --privileged=true --volumes-from u1  --name u2 ubuntu
```



继承的爹没了跟儿子没关系

```
总结：
继承后，两个容器是并列的，指向同一个数据卷，一个改动数据卷，另一个也相应的改动
```

## Docker常规安装简介

<img src="https://img-blog.csdnimg.cn/d072239a864d42de8616fe3a08b7228f.png" alt="在这里插入图片描述" style="zoom:50%;" />

以后做团队项目，只用去下载仓库或者官网打包好的镜像，直接运行镜像即可使用。步骤也就直接统一了

### 1：总体步骤：

**搜索镜像**
**拉取镜像**
**查看镜像**
**启动镜像 ：服务端口映射**
**停止容器**
**移除容器**

常见模拟案例：

### 2：安装tomcat

**1.docker hub上面查找tomcat镜像**

```
docker search tomcat
```


**2.从docker hub上拉取tomcat镜像到本地**

```
docker pull tomcat
```


**3.docker images查看是否有拉取到的tomcat**

**4.使用tomcat镜像创建容器实例(也叫运行镜像)**

```
docker run -it -p 8080:8080 tomcat
-p 小写，主机端口:docker容器端口
-P 大写，随机分配端口
i:交互
t:终端
d:后台
5.访问猫首页
```

出现问题：出现404，访问不到

解决：

1.可能没有映射端口或者没有关闭防火墙

 2.先把webapps文件删了，再把webapps.dist目录换成webapps。因为真正有东西的在webapps.dist里面，所以把它进行改名

先把webapps文件删了：  rm -f webapps
把webapps.dist目录换成webapps 

```
 mv webapps.dist webapps
```

这样子运行最新版都要修改，太麻烦了。可以使用免修版：

免修改版说明：

```
docker pull billygoo/tomcat8-jdk8
```

```
docker run -d -p 8080:8080 --name mytomcat8 billygoo/tomcat8-jdk8
```



### 3：安装mysql

**1.docker hub上面查找mysql镜像**

**2.从docker hub上(阿里云加速器)拉取mysql镜像到本地标签为5.7**

**3.使用mysql5.7镜像创建容器(也叫运行镜像)**

#### 简单版：

使用mysql镜像

```
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

```
docker ps
```

```
docker exec -it 容器ID /bin/bash
```

```
mysql -uroot -p
尝试建库建表插入数据：
```

```
show databases;
create database db01;
create table t1(id int,name varchar(20));
insert into t1 values(1,'z3');
select * from t1;
```

去使用本地的navicat看看能不能连接运行在dokcer上的mysql容器实例服务，发现是可以的，且刚刚新建的表和数据都存在。在navicat中插入数据发现也是互通的。

**坑：但是插入中文数据时，出现了报错：**

**说明docker上默认字符集编码隐患 ，于是乎在docker里面的mysql容器实例查看，输入命令内容如下：**

```
 SHOW VARIABLES LIKE 'character%'
```


发现全是拉丁，这种得在docker的mysql实例修改！

且一不小心删除容器后，那里面的mysql数据如何办？

看下面的实战版↓：

#### 实战版（解决乱码和备份问题)：

1**.新建mysql容器实例**

```
docker run -d -p 3306:3306 --privileged=true 
-v /zzyyuse/mysql/log:/var/log/mysql 
-v /zzyyuse/mysql/data:/var/lib/mysql 
-v /zzyyuse/mysql/conf:/etc/mysql/conf.d 
-e MYSQL_ROOT_PASSWORD=123456  
--name mysql 
mysql:5.7
```

2. **解决中文乱码问题：新建my.cnf 通过容器卷同步给mysql容器实例**



```
cd /zzyyuse/mysql/conf/
vim my.cnf
cat my.cnf
```

```
[client]
default_character_set=utf8
[mysqld]
collation_server = utf8_general_ci
character_set_server = utf8
```


**3.重新启动mysql容器实例再重新进入并查看字符编码**

**4.再新建库新建表再插入中文测试**

**5.结论：**

之前的DB 无效

修改字符集操作+重启mysql容器实例

之后的DB 有效，需要新建

结论：docker安装完MySQL并run出容器后，建议请先修改完字符集编码后再新建mysql库-表-插数据

**6.假如将当前容器实例删除，再重新来一次，之前建的db01实例还有，里面的数据也没有丢失**

```
docker run -d -p 3306:3306 --privileged=true 
-v /zzyyuse/mysql/log:/var/log/mysql 
-v /zzyyuse/mysql/data:/var/lib/mysql 
-v /zzyyuse/mysql/conf:/etc/mysql/conf.d 
-e MYSQL_ROOT_PASSWORD=123456  
--name mysql 
mysql:5.7
```


以后，务必要注意解决乱码和备份问题！

### 4：安装redis

**1.从docker hub上(阿里云加速器)拉取redis镜像到本地标签为6.0.8**

2.命令提醒：容器卷记得加入–privileged=true

Docker挂载主机目录Docker访问出现cannot open directory .: Permission denied
解决办法：在挂载目录后多加一个--privileged=true参数即可

**3.在CentOS宿主机下新建目录/app/redis**

```
mkdir -p /app/redis
```


**4.将一个redis.conf文件模板拷贝进/app/redis目录下**

 4.1拷贝配置文件：
 将准备好的redis.conf文件放进/app/redis目录下

这个看之前的Linux安装redis的详细教程：

**5./app/redis目录下修改redis.conf文件**
![在这里插入图片描述](https://img-blog.csdnimg.cn/66b761e4bfc34e59bf2690ff713b9e9e.png)

 3.4开启redis数据持久化 appendonly yes 可选

**6.使用redis6.0.8镜像创建容器(也叫运行镜像)**

**7.测试redis-cli连接上来**

8.请证明docker启动使用了我们自己指定的配置文件

修改前：我们用的配置文件，数据库默认是16个
修改后：宿主机的修改会同步给docker容器里面的配置。 记得重启服务
9.测试redis-cli连接上来第2次





