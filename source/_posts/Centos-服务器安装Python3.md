title: linux 服务器安装Python3
author: Light Fish
tags:
  - Python
  - linux
  - ''
categories:
  - Python
  - linux
date: 2019-02-26 13:10:00
---
# Centos 服务器安装Python3

>最近也是想把我当Django项目部署到我的的服务器上，也是在安装Python3和其依赖项上面有点疑惑，今天就来简单的总结下，记录一下。

<!-- more -->

# 安装准备

>在安装Python3之前，我们要分别安装Linux的 `wget` 工具、GCC编译器环境以及Python3使用的依赖组件、相关的安装指令如下：

* 1.安装Linux 的`wget`工具,用于网上下载文件

```
yum -y install wget
```

* 2.GCC编译器环境，安装Python3时所需的编译环境

```
yum -y insall gcc
```

* 3.Python3 使用的依赖组件

```
yum -y groupinstall "Development tools"

yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

# 安装Python3

>完成上述安装之后，我们就可以开始安装Python3。我们使用wget指令在Python官网下载Python3.6的压缩包，在CentOS 7 系统输入下载指令

```
wget "https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz"
```

>下载完成之后，可以在当前路径下查看下载的内容。

>下一步就是对压缩包进行解压，在当前压缩包目录下输入解压指令

```
tar -zxvf Python-3.6.3.tgz
```

>解压完成后就会在当前路径下出现Python-3.6.3的文件夹，该文件就是我们需要的开发环境，里面包含着Python3.6版本所需要的组件。最后我们将Python3.6 编译到CentOS 7 系统中，编译指令如下

```
# 进入该文件夹
cd Python-3.6.3
# 依次输入以下指令
sudo ./configure
make
make install
```

>编译完成我们就可以在CentOS系统中输入指令python3，即可进入python交互模式。

<br><br><br>So,Just have fun...