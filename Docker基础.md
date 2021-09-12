# Docker

![image-20200606203315597](E:/docker/docker_learning-master/Docker.assets/image-20200606203315597.png)

## 参考资料

官方文档：https://docs.docker.com/docker-for-windows/ 

【官方文档超级详细】

仓库地址：https://hub.docker.com/

【发布到仓库，git pull push】

## 前期基础

linux基本命令，类似cd，mkdir等



## Docker概述

### Docker为什么会出现

一款产品，开发和上线两套环境，应用环境配置费时费力，而且容易出问题

尤其对于机器学习和深度学习的库更是如此，很可能存在版本问题、底层依赖冲突问题

所以发布项目时，不只是一套代码过去，而是代码+环境整体打包过去

所谓开发即运维，保证系统稳定性，提高部署效率

使用Docker后的流程：

开发：建立模型--环境--打包带上环境，即镜像--放到Docker仓库

部署：下载Docker中的镜像，直接运行即可

Docker的思想来自于集装箱，集装箱，对环境进行隔离

Docker通过隔离机制，可以将服务器利用到极致

### 容器vs虚拟机

在容器技术出来之前，用的是虚拟机技术

#### 虚拟机原理示意图

![image-20200606205436434](E:/docker/docker_learning-master/Docker.assets/image-20200606205436434.png)

缺点：

1. 资源占用多
2. 冗余步骤多
3. 启动很慢

#### 容器化技术示意图

不是模拟的完整的操作系统

![image-20200606205739655](E:/docker/docker_learning-master/Docker.assets/image-20200606205739655.png)

#### 二者对比

比较虚拟机和Docker的不同

|          | 传统虚拟机               | Docker        |
| -------- | ------------------------ | ------------- |
| 虚拟内容 | 硬件+完整的操作系统+软件 | APP+LIB       |
| 大小     | 笨重，通常几个G          | 轻便几个M或KB |
| 启动速度 | 慢，分钟级               | 快，秒级      |
|          |                          |               |

## Docker安装

### Docker的基本组成

![image-20200606212250845](E:/docker/docker_learning-master/Docker.assets/image-20200606212250845.png)

明确几个概念：

1. 镜像(image)：docker镜像好比一个模板，可以通过这个模板来创建容器(container)，一个镜像可以创建多个容器，类似Python中的Class

2. 容器(container)：类似Python中通过Class创建的实例，Object；容器可以理解为一个简易的系统

3. 仓库(repository)：存放镜像的地方，

   分为共有仓库和私有仓库

   - Docker Hub：国外的

   - 阿里云：配置镜像加速

### 环境准备

我们要有一台服务器，并且可以操作它

1. Linux命令基础，虚拟机
2. CentOS 7
3. 使用Mobaxteam链接远程服务器或Xshell

### 安装xshell

下载CentOS7 https://www.jianshu.com/p/a63f47e096e8

下载VMware 360软件管家下载

VMware配置虚拟机 https://blog.csdn.net/babyxue/article/details/80970526

xshell链接服务器 https://blog.csdn.net/zzy1078689276/article/details/77280814

### Centos安装

https://docs.docker.com/engine/install/centos/

### 卸载旧的版本

```
# 卸载旧的版本
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```

![image-20200608092628498](E:/docker/docker_learning-master/Docker.assets/image-20200608092628498.png)

### 安装基本环境



```
# 安装基本的安装包
$ sudo yum install -y yum-utils
```

!

![image-20200608093114774](E:/docker/docker_learning-master/Docker.assets/image-20200608093114774.png)

### 设置镜像的仓库

注意！！下载默认用国外的，太慢不要用！

用国内镜像，百度搜索，docker的阿里云镜像地址

```
# 不要用官网默认这个！
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo # 默认是国外的

# 换成下面的

$ sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 阿里云镜像
```

![image-20200616145430166](E:/docker/docker_learning-master/Docker.assets/image-20200616145430166.png)

直接复制粘贴就OK了

更像软件包索引

```
yum makecache fast
```

![image-20200616150014082](E:/docker/docker_learning-master/Docker.assets/image-20200616150014082.png)

没有问题的话就是可以用的

### 安装docker引擎

```python
yum install docker-ce docker-ce-cli containerd.io # docker-ce 社区版 ee 企业版
```

注意这里会有几个个y/n的判断

![image-20200616150818860](E:/docker/docker_learning-master/Docker.assets/image-20200616150818860.png)

![image-20200616150922549](E:/docker/docker_learning-master/Docker.assets/image-20200616150922549.png)

要看到Complet再收手！



### 启动Docker

```
systemctl start docker # 代表启动成功
```

```
docker version
```

```
Client: Docker Engine - Community
 Version:           19.03.11
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        42e35e61f3
 Built:             Mon Jun  1 09:13:48 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.11
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       42e35e61f3
  Built:            Mon Jun  1 09:12:26 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```

```
docker run hello-world
```

![image-20200616151641013](E:/docker/docker_learning-master/Docker.assets/image-20200616151641013.png)

中间一堆是签名信息

run的运行流程图

![image-20200616161441669](E:/docker/docker_learning-master/Docker.assets/image-20200616161441669.png)

查看下载的镜像

```
docker images
```

![image-20200616151913277](E:/docker/docker_learning-master/Docker.assets/image-20200616151913277.png)

### 卸载Docker

```
# 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

# 删除资源
rm -rf /var/lib/docker # docker 的默认工作路径

```

### 阿里云镜像加速

```
sudo mkdir -p /etc/docker # 创建一个目录
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://uyfgafsw.mirror.aliyuncs.com"]
}
EOF # 编写配置文件

sudo systemctl daemon-reload # 重启服务
sudo systemctl restart docker # 重启docker
```

![image-20200616160315298](E:/docker/docker_learning-master/Docker.assets/image-20200616160315298.png)

 ![img](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/07/09/kuangstudy5851fdc5-b4f3-4bae-8011-254d61c9e7c1.png) 

## 底层原理

Docker是真么工作的？

Docker是一个Client-Server结构的系统，Docker的守护进程在主机上。通过Socket从客户端访问！

DockerServer接受到Docker-Client的指令，

![image-20200616162107363](E:/docker/docker_learning-master/Docker.assets/image-20200616162107363.png)

Docker为什么比VM快？

1. Docker有着比虚拟机更少的抽象层
2. docker主要用的是宿主机的内核，vm需要Guest OS

![image-20200616162302653](E:/docker/docker_learning-master/Docker.assets/image-20200616162302653.png)