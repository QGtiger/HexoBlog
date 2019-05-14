title: linux 服务器的一些命令行使用
author: Light Fish
tags:
  - linux
categories:
  - linux
date: 2019-02-26 13:52:00
---
# linux 服务器

>本篇博客用于记载一些linux的命令行。

<!-- more -->

## 查看端口

>查看端口是否被占用

```
netstat -lnp|grep 8000 #后面接端口，如果被使用会显示
```

>查看进程的详细信息

```
ps 8000 # 后面接端口
```

>结束进程

```
kill -9 8000 # 后面是-9 加端口号
```