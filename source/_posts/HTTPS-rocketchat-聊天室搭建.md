title: HTTPS rocketchat 聊天室搭建
author: Light Fish
date: 2019-04-11 14:12:51
tags:
---
# 开源免费的rocketchat搭建

>rocketchat 是一款开源免费的网站式聊天室，你可以搭建在你的VPS上。今天也是简单写一下做个记录。其中我会介绍如何配置SSL证书。

<!-- more -->

# 前言

>在搭建之前当然你需要一台VPS，这里的话，我就不多阐述了，我搭建的是vultr上的`ubuntu 18.10LTS` 版本。使用nginx反向代理和免费的SSL证书的配置。

>当然你最好也是需要一个域名，因为SSL的证书配置需要一个域名，你可以去freenom上注册一个免费的域名。

# 安装Rocketchat

>开始前我们需要对刚购买的VPS进行更新系统

```
sudo apt update && sudo apt upgrade
```

>然后就是进行安装rocketchat，安装`Rocketchat`最快的方法就是使用`snap`。`snap`是`Linux`系统上的一种包管理工具。在`Ubuntu 16.0LTS`以上版本系统都内置了`snap`

>`snap`安装`Rocketchat`

```
sudo snap install rocketchat-server
```

>安装后,Rocketchat会自动启动，你也是可以检查下是否启动成功

```
sudo service snap.rocketchat-server.rocketchat-server status
```

![img](http://qnpic.top/rocketchat%5C1.jpg)

# 安装nginx并配置

>安装nginx的方法我之前也是在Centos7上介绍过，这里的话是使用一下命令行

```
sudo apt install -y nginx
```

>启动nginx

```
sudo systemctl start nginx
# 设置开机自启
sudo systemctl enable nginx
```

## 设置反向代理

>默认的欢迎页面的文件位置 `/etc/nginx/sites-enabled/defalut`,但是其中真正的文件位置在 `/etc/nginx/sites-available/`,只不过设置了软连接

```
sudo ls -l /etc/nginx/sites-enabled
total 0
lrwxrwxrwx 1 root root 34 Aug 16 18:20 default -> /etc/nginx/sites-available/default
```

>删除欢迎页面

```
sudo rm /etc/nginx/sites-enabled/default
```

## 创建反向代理

>这里的话，需要创建配置文件，所以需要`vim`,如果没有安装的话，可以使用以下命令行安装`vim`

```
sudo apt-get install -y vim
```

>然后创建配置文件`rocketchat.conf`

```
sudo vim /etc/nginx/sites-available/rocketchat.conf
```

>基本内容如下：

```
server{
	listen 80;

	server_name example.com: #这里的example.com填写的是你的域名
   
   location / {
   		proxy_pass http://localhost:3000/; #指向rocketchat运行的300端口，从而完成反向代理
   }
}
```

>**注意：**上面的`example.com`是你的域名并且要配置你的域名设置一个A记录指向你的VPS的ip

>然后通过下述方法创建新的软连接来启用新的配置

```
sudo ln -s /etc/nginx/sites-available/rocketchat.conf /etc/nginx/sites-enabled/
```

>测试配置是否有误

```
sudo nginx -t
```

![img](http://qnpic.top/rocketchat%5C2.jpg)

>重新加载nginx配置

```
sudo ngins -s reload
```

# 配置SSL证书

>申请SSL证书的方法有很多，这里的话使用免费的[Let's Encrypt](https://letsencrypt.org/)的证书。有人写了名为`Cerbot`的工具，轻松配置证书

```
sudo apt-get install -y software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install -y python-certbot-nginx
sudo certbot --nginx
```

>Certbot 会询问有关该网站的信息。在执行 `sudo apt-get install -y python-certbot-nginx` 的时候会询问位置信息，我们选择 亚洲（也就是 `6. Asia`），时区选择 `69. Shanghai` 即可。(该方法也是我网上偶然得到，所以有点出入，我运行的时候没有该情况)

>在执行 `sudo certbot --nginx` 的时候会询问你的邮箱，填写和你注册域名时相同的邮箱，其他询问同意即可。

>PS:运行上述代码的时候哦，遇到的情况当然也是远不如此，当时emmm，问题蛮简单的。唯一需要注意的是`nginx反向代理`填写的域名要记得解析一条A记录指向你的VPS，否则会挑错

## 开启证书自动续约

>我们的证书有效期是3个月，不过cerbot很聪明有一个自动续约的功能，执行下述代码：

```
sudo certbot renew --dry-run
```

# 开始使用Rocketchat

>配置到这里，该聊天室也会成功配置在你的VPS上。第一次登陆也是需要设置管理员信息。下面也是我成功搭建成功的截图

![img](http://qnpic.top/rocketchat%5C3.jpg)

<br><br><br>So<br><br>Just have fun...