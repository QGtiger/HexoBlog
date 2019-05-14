title: Django 使用MySql的几点注意事项
author: Light Fish
tags:
  - MySql
  - Django
categories:
  - Python
  - Django
date: 2019-01-29 17:05:00
---
# Django连接MySql的几点注意事项

>在重装电脑后的系统中，这几天内也是陷入了安装软件的小小痛苦中，[上篇博客](/2019/01/29/MySql-安装和简单配置/)也是简单说了下Windows下如何安装MySql，现在就来讲解一下Django运行使用到MySql的一些注意事项。

<!-- more -->

# 前言

>我也是自重装电脑后，运行我的一个Django项目后，出现了这一些错误，如提示`Did you install mysqlclient?`或`django.db.utils.OperationalError: (2059, <NULL>)`错误等等，今天就来讲解下这两个问题

## 安装mysqlclient

>安装`mysqlclient`，可以用`pip install mysqlclient`来进行安装，也可以本地安装，你可以来这里进行[下载](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient)

>cp35代表python3.5的版本，win32代表32位的系统，所以需要选择正确，否则安装过程会报错平台不匹配

![img](http://qnpic.top\django_mysql%5C1.jpg)

>因为这里是本地安装，你可以用`pip install`安装，后面跟你下载的的文件名

![img](http://qnpic.top\django_mysql%5C2.jpg)

## MySql 8.X的一些问题

>`MySql`8.X对用户密码的加密方式为`caching_sha2_password`, django暂时还不支持这种新增的加密方式。只需要将用户加密方式改为老的加密方式即可。

>1.登录mysql，连接用户为root

```
mysql -u root -p
```

>2.执行命令查看加密方式

```
use mysql;
select user,plugin from user where user='root';
```

![img](http://qnpic.top\django_mysql%5C3.jpg)

>3.执行命令修改加密方式

```
alter user 'root'@'localhost' identified with mysql_native_password by 'yourpassword' # yourpassword就要修改成你的mysql登录密码
```

![img](http://qnpic.top\django_mysql%5C4.jpg)

>4.属性权限使配置生效

```
flush privileges
```

>5.现在就可以启动Django服务了

<br><br><br>So，Just have fun...