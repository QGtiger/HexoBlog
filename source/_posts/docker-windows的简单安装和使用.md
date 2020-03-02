title: docker windows的简单安装和使用
author: Light Fish
date: 2019-06-12 22:38:33
tags:
---
# docker 的简单安装和使用

> 一直想去正式玩玩docker来着，今天也是简单如愿了，就简单记录一下

<!-- more -->

## Win7 安装

> 1.安装很简单，下载链接： https://get.daocloud.io/toolbox

![img](http://qnpic.top/2019-06-12%5C3.jpg)

> 2.下载完成后就是按照默认安装了，对了，安装的时候会提示要安装一序列的驱动，记得确认安装。安装完成后桌面上就有这三个图标

![img](http://qnpic.top/2019-06-12%5C4.jpg)

> 3.根据网上的教程就是双击Docker Quicksrart ，然后就会去下载最新版本。但是可能因为国内网络的问题很难安装完成，这里我就提供我从网上找的办法： 在你下面安装完成后的目录下其实就有这个 `boot2docker.io` 镜像

![img](http://qnpic.top/2019-06-12%5C5.jpg)

> 4.复制到 C:\Users\Administrator\.docker\machine\cache  然后在网络断开的情况下重新双击Docker Quicksrart 进行初始化安装。

![img](http://qnpic.top/2019-06-12%5C6.jpg)

> 5.这里我安装的时候出现了错误：` Failed to instantiate CLSID_VirtualBox w/ IVirtualBox, but CLSID_VirtualBox w/ IUnknown works.` 如果跟我一样可以看看这篇[博客](https://blog.csdn.net/xz360717118/article/details/67638548/) ,如果没有的话就进入了初始化安装

![img](http://qnpic.top/2019-06-12%5C1.jpg)

![img](http://qnpic.top/2019-06-12%5C2.jpg)

### XShell 连接

> Docker Quicksrart Terminal 使用起来不方便，可以用xshell  ssh连接登录Docker环境，IP地址上面也有，这次稍微需要注意的就是**默认登录用户**

```
用户名： docker
密码： tcuser
```

## 一些基本简单用法

> `docker pull ubuntu`  安装ubuntu镜像 `docker images`  查看镜像

![img](http://qnpic.top/2019-06-12%5C7.jpg)

- 运行镜像

> `docker run -it -d --name ubuntu_test -p 8088:80 ubuntu:latest`
>
> name 自定义容器名， -p制定容器端口映射，前者虚拟机，后者容器端口，成功返回id 

- 查看docker 运行镜像

> `docker ps`

- 根据id查看同期信息

> `docker inspect xx`

![img](http://qnpic.top/2019-06-12%5C8.jpg)

- 进入容器

> `docker exec -it ubuntu_test /bin/bash` 这样就进入 了ubuntu容器了

![img](http://qnpic.top/2019-06-12%5C9.jpg)

- 停止容器

> `docker stop d4`

![img](http://qnpic.top/2019-06-12%5C10.jpg)

- 制作镜像

> `docker commit d4 ubuntu_test1:1.0` 这里的d4就死前面运行的容器id

![img](http://qnpic.top/2019-06-12%5C11.jpg)

<br><br><br>So, Just have fun...