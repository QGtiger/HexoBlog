title: MySql 安装和简单配置
author: Light Fish
tags:
  - MySql
categories:
  - MySql
  - ''
date: 2019-01-29 15:00:00
---
# MySql安装和配置

>今日也是因为自己的原因，导致电脑的硬盘坏了(我也是纳闷了，就是从一个不到一米的桌子上摔下来就莫得了QAQ)，从而，只能换一个。原来的电脑里的数据也是就此没了，懒得去开盘，可怜我的400G的资源。哎~~~，今天也是想写一篇关于Django的发送邮件和密码验证，安装好pycharm和其他的运行环境，emmmm，在MySql上卡顿了一下就来简单写一篇关于MySql的安装和配置，来完成新硬盘的第一篇博客。

<!-- more -->

## Windows上安装

>其实Windows上安装MySql还简单点，奈何以前安装了个wamp就自动装好了就不是很在意。这里就可以再介绍下，最新版本可以直接去[MySql官网](https://dev.mysql.com/downloads/mysql/)下载,(这里下载的是zip，解压简单配置下就行了)

![img](http://qnpic.top\MySql_down%5C1.jpg)

>点击下载，它就提示你可以登录或者注册，你也可以点击跳过。

![img](http://qnpic.top\MySql_down%5C2.jpg)

>下载完了后，我们就可以将zip压缩包解压到你想要的的目录下，我这里是`E:\mysql-8.0.14-winx64\mysql-8.0.14-winx64`,**注意一下**，这里是**重难点**，去年没考，今年必考，我这里推荐解压的目录要是纯英文的，最好不要有汉字，我第一次安装就是有汉字，网上也是半天搜查不到，所以我就试了一下，So，I got it.

![img](http://qnpic.top\MySql_down%5C4.jpg)

* 下图就是我进行后边的操作而错误的原因，不要像我一样哦~

![img](http://qnpic.top\MySql_down%5C3.jpg)

>接下来，我们就可以来配置MySql的配置文件，在上图的解压文件目录`E:\mysql-8.0.14-winx64\mysql-8.0.14-winx64`下，创建`my.ini`文件(在上二图也是可以看到)，编辑`my.ini`，添加以下信息：

```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=E:\\mysql-8.0.14-winx64\\mysql-8.0.14-winx64
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

>接下来启动MySql数据库，打开cmd命令行工具(不要问我为什么不和你们一样，我用的是`cmder`，emmm，你们可以去百度一下)，切换到MySql的bin目录下：

![img](http://qnpic.top\MySql_down%5C5.jpg)

>初始化数据库：

```
mysqld --initialize --console
```

![img](http://qnpic.top\MySql_down%5C6.jpg)

>执行完成后哦，上图的`drpzXoa=e8>e`就是root用户的初始化密码，后续登录有用。然后输入一下安装命令：

```
mysqld install
```

![img](http://qnpic.top\MySql_down%5C8.jpg)

>这里就简单拓展一下，可能是电脑里之前安装过MySql，就会出现下图的错误,你可以输入`mysqld -remove MySQL`,从而卸载之前的MySql。

![img](http://qnpic.top\MySql_down%5C7.jpg)

![img](http://qnpic.top\MySql_down%5C9.jpg)

>以上就基本安装完成了，输入一下命令就可以启动MySql服务：

```
net start mysql
```

![i](http://qnpic.top\MySql_down%5C10.jpg)

## 登录MySql

>当 MySQL 服务已经运行时, 我们可以通过 MySQL 自带的客户端工具登录到 MySQL 数据库中, 首先打开命令提示符, 输入以下格式的命名:

```
mysql -h 主机名 -u 用户名 -p
```

>参数说明：

* 1. -h : 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略;

* 2. -u : 登录的用户名;

* 3. -p : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

>如果我们要登录本体的MySql数据库，只需要输入以下命令：

```
mysql -u root -p
```

>回车确认后，就会提示输入密码`Enter password:`,**注意一下**，如果你是第一次启动，密码就是上面的初始化密码`drpzXoa=e8>e`,输入后就可以进入Mysql服务界面。

![img](http://qnpic.top\MySql_down%5C11.jpg)

>是不是到这里就以为好了，no no no，至少这个现在版本不是(眼尖的应该就看到了，我输入`show databases`命令确实跳错，emmm，第一次还输错了，见笑了~~)，咳，这里依旧是重点，相信有点英语基础的也是能看得懂的，就是说要你重置下密码，毕竟初始化的密码实在是太“真实”了，So, You should input code: 

```
alter  user 'root'@'localhost' identified by '111111'; # 你也可以输入自己想要的密码
```

![img](http://qnpic.top\MySql_down%5C12.jpg)

<br><br><br>到这里就结束了本次的博客，So，Just For Fun...