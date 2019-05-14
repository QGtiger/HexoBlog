title: hexo 教程五--部署到centos服务器
author: Light Fish
tags:
  - hexo搭建
  - ''
categories:
  - hexo
date: 2019-01-30 10:36:00
---
# 如何将hexo上传到自己的centos服务器上

>最近也会经过自己室友的启发下，无聊也来把自己的hexo博客部署到`centos`服务器上，顺便也玩一下centos服务器。So，let's get it.

<!-- more -->

# 基本思路

>在自己的本地主机上部署hexo博客，然后`git push`到自己的`github`上，如果，你不是很懂`git`的操作，你可以看我这一篇[Git博客](/2018/12/13/t-的一些基本操作和讲解/)。

>上传到自己的git仓库，上传的文件也是hexo项目里`public`的文件，如下图：

![img](http://qnpic.top\hexo_centos%5C1.jpg)

![img](http://qnpic.top\hexo_centos%5C2.jpg)

>所以，可以在`CentOS`上搭建`hexo`博客，然后用Git操作`git clone`，克隆下仓库(就是上面上传到github仓库的文件)。然后每次更新只需要用`git pull`来实现更新。

>上面实现了更新hexo博客，然后我们用的是`nginx`,来实现网页的展示

# 部署步骤

>想要实现部署，首先要有一台的自己的`CentOS`服务器(如果没有，可以先在虚拟机上进行试验)。然后在自己的服务器上安装`nodejs`,`hexo`,`nginx`等。

## CentOS服务器购买

>这里的话，我是从[vultr官网](https://www.vultr.com)上进行购买(你也可以从阿里云那进行购买,主要vultr的买的服务器可以随时推掉。。)。咳，言归正传，你先到[vultr](https://www.vultr.com)上注册、登录。然后进行充值，可以微信支付。

![img](http://qnpic.top\hexo_centos%5C3.jpg)

>充值完成了，也就可以进行购买

>选取购买服务器的区域，这里我推荐是美国，我刚开始我也是以为日本最近就访问快一点，但是不能`SSH`连接，百度一下好像是日本的基本都被封了...

![mg](http://qnpic.top\hexo_centos%5C4.jpg)

>选取CentOS服务器,并且选择配置，基本的就行了

![img](http://qnpic.top\hexo_centos%5C5.jpg)

>然后，基本配置一下就可以购买了

![img](http://qnpic.top\hexo_centos%5C6.jpg)

![img](http://qnpic.top\hexo_centos%5C7.jpg)

>购买完成之后，进入这个界面就可以看到了，后面的`running`状态就得等个几分钟等它配置一下

![img](http://qnpic.top\hexo_centos%5C8.jpg)

>点击服务器，进入详细界面，下面的就是IP地址 和 登录密码

![img](http://qnpic.top\hexo_centos%5C9.jpg)

## 连接CentOS服务器

>购买完成以后，就可以连接CentOS服务器进行配置，基本的连接方式有`git bush`界面下，输入下面代码进行登录，然后输入密码：

```
ssh root@104.156.250.144   # 后面的IP地址就是你购买服务器的IP地址，自己修改一下
```

![img](http://qnpic.top\hexo_centos%5C10.jpg)

>你也可以通过Xshell进行登录，[下载链接](https://pan.baidu.com/s/1Kd3lURMuVqyLNyCfpQ0rdw)  提取码: tybe 。emmm，使用步骤，我就不多介绍了，网上应该有教程（对了，我这个是绿色破解版，你下载解压后，先运行`绿化.bat`）

## `CentOS`服务器部署hexo

>在前面的连接完成的界面内进行步骤部署(这里参考自室友[博客](http://47.102.99.250/2019/01/21/%E3%80%90%E9%9A%8F%E7%AC%94%E3%80%91%E5%8F%88%E5%9B%9E%E5%88%B0%E6%9C%80%E5%88%9D%E7%9A%84%E8%B5%B7%E7%82%B9-Hexo/))

>1.升级yum

```
yum update -y
#对所有的提问都回答“yes”；
```

>2.安装`Git`

```
sudo yum install git-core

git --version  # 查看git版本验证是否安装成功
```

>3.安装nodejs

```
yum install -y nodejs
```

>4.安装hexo

```
npm install hexo-cli -g
```

>5.安装nginx

```
yum install nginx -y
# 启动nginx
systemctl start nginx.service
```

>6.拓展nginx命令行

* 1.重启nginx

```
nginx -s reload
```

* 2.停止nginx

```
nginx -s stop
```

>7.配置`nginx.conf`,`nginx.conf`在`/etc/nginx`目录下

![img](http://qnpic.top\hexo_centos%5C11.jpg)

>8.对了那个hexo的博客好像不能随便创建在某个目录下,根据原来的`nginx.conf`的原来配置静态页面在`/usr/share/nginx/html`目录下。所以，我们可以在`/usr/share/nginx`下部署hexo博客，记得在`nginx.conf`里的静态文件目录，指向到`public`目录。

![img](http://qnpic.top\hexo_centos%5C13.jpg)

>但是为了长远的更新博客，前面也提到过，我们只需要`git pull`，拉取github仓库。而github仓库既是public文件，所以，我们只需要指向这个文件就行了，如下面，我`git clone`的是我的io仓库，所以我只需要修改`nginx.conf`如下

![img](http://qnpic.top\hexo_centos%5C12.jpg)

>对了，我记得vultr上购买的服务器没有默认开启firewall,所以，要执行以下命令行以运行HTTP和HTTPS通信

```
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```


>现在就基本配置完成了，只需要执行`nginx -s reload`，重启nginx，你去访问自己的网站及应该能访问了。

![img](http://qnpic.top\hexo_centos%5C14.jpg)

<br><br><br>这次的博客也是基本完成了,(你也先访问以下我的[服务器](http://104.156.250.144/),如果不能访问可以是我退了)，算是熟悉了一点centos服务器和nginx的使用So<br><br>Just have fun...




