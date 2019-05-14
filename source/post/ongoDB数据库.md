title: MongoDB数据库
author: Lightfish
toc: true
tags:
  - Python
  - MongoDB
categories:
  - Python
date: 2018-12-06 20:58:18
---
# 非关系型数据库MongoDB的安装
>MongoDB是由C++语言编写的非关系型数据库，是一个基于分布式文件存储的开源数据库系统，其内容存储形式类似JSON对象，它的字段值可以包含其他文档、数组及文档数组，非常灵活。

<!--more-->

### 1.数据库的下载和安装

>直接去[mongodb官网](https://www.mongodb.com/)进行下载

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqked0pj31gb0qiag0.jpg)

>下载完成以后，双击它开始安装，指定安装路径，例如我指定的安装路径是E:\mongodb,如图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqkl443j30xb0inju5.jpg)

>安装完成之后，进入MongoDB的安装目录，在bin同目录下新建文件夹data，如上图
>然后进入data文件夹，新建文件夹db来存储数据目录如图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqko0h0j30xa0ixtb5.jpg)

>然后进入到之前bin目录下(是bin目录下哦，不是同bin目录)，打开shift+右键打开命令行，如图(emm，我用的是cmder，蛮好的，有空出个博客):

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqkt5xfj30xs0iy418.jpg)

>输入以下代码：

```
mongod --dbpath E://mongodb/data/db  #后面这个目录就是你前面创建的data/db目录
```

>运行之后，会出现一些输出信息，看到下图就是你启动了mongodb服务

![mongodb](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqkwoifj30rh0gtqbg.jpg)

>然后点击bin目录下的`mongo.exe`，就进入了mongodb界面，输入show dbs；看看里面的数据库，基本就成功了

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxql48vsj30xf0imwjo.jpg)

![img](/img/mongodb/7.jpg)

### 2.可视化工具Robo 3T的下载和安装

>Robo 3T是针对MongoDB的的可视化工具，由于篇幅问题，可以看这篇[博客](https://blog.csdn.net/qq_36070288/article/details/73822101)

<br><br><br>
Just have fun...