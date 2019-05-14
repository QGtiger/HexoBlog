title: 免费GearHost云主机搭建个人网站
author: Light Fish
tags:
  - GearHost
  - 干货
categories:
  - GearHost
  - 干货
date: 2019-01-31 12:38:00
---
# GearHost 免费主机

>GearHost是国外一家主机商，提供免费和付费的主机套餐，支持PHP、.Net和Node.js程序。同时它还提供免费的Mysql、SQL Server 和Mongo db数据库，可以运行PHP和asp.net程序，而且还没有烦人的广告。----from [Here](http://wosn.net/625.html)

<!-- more -->

# 申请

>GearHost免费套餐主机容量是100MB，Mysql数据库5Mb，月流量是1GB。免费套餐对于建站数量没有限制，但是限制内存是256MB，CPU占用为5%，不支持ssl。

>申请的话，你可以直接去[百度](https://www.baidu.com)搜索GearHost，或者点击链接直接访问： https://www.gearhost.com/

![img](http://qnpic.top\free_cloud1%5C3.jpg)

>和其他的服务器运营商一样，都得进行注册登录，我这里就不多过详述，没什么值得注意的。

![img](http://qnpic.top\free_cloud1%5C1.jpg)

>点击添加，并且输入你想要的二级域名，选择免费的主机(你可以选择收费的)。然后最下面就有一个button，点击创建

![img](http://qnpic.top\free_cloud1%5C4.jpg)

![img](http://qnpic.top\free_cloud1%5C5.jpg)

>点击创建，它就会开始`building`，可能能等个几分钟。

![img](http://qnpic.top\free_cloud1%5C2.jpg)

>上面的`status`状态变成`running`就表示已经创建成功，点击你前面输入的二级域名名称，也就是你创建的云主机名称.点击进去就是创建的云主机的详情

![img](http://qnpic.top\free_cloud1%5C6.jpg)

![img](http://qnpic.top\free_cloud1%5C7.jpg)

# GearHost 空间FTP管理

>点击GearHost的管理面板中的`Publish`选项查看空间的FTP信息。FTP信息都是系统生成，不能自定义。

![img](http://qnpic.top\free_cloud1%5C8.jpg)

>上面就是之后需要云主机连接的信息。在此之前你需要下载`Filezilla`,你可以百度自行下载,或者点击这个链接——https://pan.baidu.com/s/1cBtjT8gDtNwrRJjY6skF8g 提取码: k2m6 下载成功后点击 文件->站点管理 ，如下图：

![img](http://qnpic.top\free_cloud1%5C9.jpg)

>进入站点管理，进行下图配置

![img](http://qnpic.top\free_cloud1%5C10.jpg)

>点击连接，等待连接

![img](http://qnpic.top\free_cloud1%5C11.jpg)

# 静态文件展示

>我们可以通过这个云主机来进行静态文件展示，如WordPress静态文件的展示，Hexo博客展示，或者GitBook静态文件的展示。这其中的可以参考之前的博客——[CentOS服务器部署hexo](/2019/01/30/hexo-教程五-部署到centos服务器/),基本理念一样。这里我就简单描述一下和展示

>先把`/site/wwwroot`下的文件删除，这个就是默认的，我们可以放入我们自己的html文件，用来展示

![img](http://qnpic.top\free_cloud1%5C12.jpg)

>现在自己的电脑上创建index.html文件用来测试，然后保存拖拽进去

![img](http://qnpic.top\free_cloud1%5C13.jpg)

![img](http://qnpic.top\free_cloud1%5C14.jpg)

>到这里就配置完成了，我们就可以直接去访问，就是之前的 那个网址，可以直接访问

![img](http://qnpic.top\free_cloud1%5C15.jpg)

<br><br><br>这里的博客就基本到这里了，我们可以通过这个云主机来进行静态文件展示，如WordPress静态文件的展示，Hexo博客展示，或者GitBook静态文件的展示。这其中的可以参考之前的博客——[CentOS服务器部署hexo](/2019/01/30/hexo-教程五-部署到centos服务器/)，So<br><br>Just have fun...