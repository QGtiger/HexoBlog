title: 小白搭建SS科学上网
author: Light Fish
tags:
  - 科学上网
categories:
  - 科学上网
date: 2019-01-30 21:49:00
---
# 小白搭建SS科学上网

>这几天也是无聊，去[vultr官网](https://www.vultr.com)买了个国外的服务器并且不部署了自己的hexo博客——[LF's Blog](https://qgtiger.github.io),但是总感觉这样不是很对得起这个国外服务器，所以，今天就来用这个国外服务器搭建SS(shadowsocks),也就是我们熟悉的节点。有可能你会问你前几天不是写了个[Chrome浏览器的科学上网](/2019/01/06/上网/),但是这个不一样，搭建了这个SS，我们就可以在移动客户端，如iPhone，android手机，PC等进行科学上网，我也是看到了能在移动客服端上搭建才起了"色心",So show you .

<!-- more -->

>我网上看的教程也是大部分是搬瓦工上买的VPS服务器

# 购买国外的服务器

>购买国外服务器可以从`搬瓦工`和`Vultr`上进行购买

>`Vultr`购买服务器可以看上一篇博客——[hexo 教程五--部署到centos服务器](/2019/01/30/hexo-教程五-部署到centos服务器/),里面有提及到vultr服务器的购买和连接。如果嫌弃我写的博客，我这里也推荐一篇博客： http://vultr.aicnm.com/%E6%9C%80%E6%96%B0Vultr%E6%B3%A8%E5%86%8C%E5%8F%8AVPS%E8%B4%AD%E4%B9%B0%E5%9B%BE%E6%96%87%E6%95%99%E7%A8%8B/

>至于搬瓦工的VPS购买教程你可以参考这篇博客： https://www.bandwagonhost.net/716.html

# 连接VPS

>这里我使用的是vultr上购买的CentOS 7服务器，emmm,我去试试CentOS 6的vultr VPS服务器(方正能随时退)，因为我后面要安装锐锋来提高带宽，也就是访问速度，CentOS7我试过好像不太行。

>在上一篇博客——[hexo 教程五--部署到centos服务器](/2019/01/30/hexo-教程五-部署到centos服务器/)，里面也是提及到如何连接VPS，这里写博客使用的是Xshell，资源你可以去上一篇博客看，或者上网查找。

# Vultr VPS搭建SS

>1.安装wget命令工具,下图也是能看的Vultr上购买的VPS已经安装了wget

```
yum install wget
```

![img](http://qnpic.top\shadowsock%5C3.jpg)

>依次运行下面三行命令，如下图所示按提示选择选项。建议：端口选择大于 1000 。

```
wget — no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

![img](http://qnpic.top\shadowsock%5C4.jpg)

>执行最后一条命令时，就会进入配置界面,你可以进行配置，如下图

![img](http://qnpic.top\shadowsock%5C5.jpg)

>上面这个配置安装可能需要一会时间，静静等待

>如果你看到了下面这个图，那么恭喜你，你已经完成了SS的搭建

![img](http://qnpic.top\shadowsock%5C1.jpg)

# SS客户端下载

>MAC版下载地址：https://github.com/gaoshilei/ShadowsocksX-NG/releases/download/1.4.3-R8-build3/ShadowsocksX-NG-R8.dmg

>windows版下载地址：https://github.com/iMeiji/shadowsocks_install/releases/download/0.13/ssr-win.4.7.0-fix.7z

>ios下载app： seedwingy

>安卓版下载地址：https://github.com/iMeiji/shadowsocks_install/releases/download/0.13/ssr-android-3.4.0.5.apk﻿

# 运行成功截图

>我这里在windows和iPhone上进行了验证

## windows

>在上面下载了Shadowsocks了，并运行

![img](http://qnpic.top\shadowsock%5C6.jpg)

>上图也是基本配置了SS，点击确认后就是开启了科学上网。下图就是解释`PAC模式`和`全局模式`，PAC就是智能模式，它访问国内网站时，并不会使用这个SS代理，而全局模式就是全部使用代理。这里的话当然是推荐PAC模式

![img](http://qnpic.top\shadowsock%5C7.jpg)

>下图也是选择哪个SS节点

![img](http://qnpic.top\shadowsock%5C8.jpg)

## iPhone

>现在iPhone上对于翻墙也是打击蛮严重的，不过我无意间看到了这个`Seedwingy`软件，看到的人赶快去下载吧，下载记得要五星好评才能使用~~

<img src="http://qnpic.top\shadowsock%5C10.PNG" width="250">

# 锐锋安装

>锐速serverspeeder是一款TCP网络加速软件，能在Linux系统和Windows系统的服务器中安装，安装后能启到提高网络连接稳定性、带宽利用率、低访问失败率等作用，从而提高服务器网络访问速度。锐速并非实际增大服务器带宽，只是提高网络的稳定性和利用率而已。一个明显变化就是在同一VPS安装科学上网工具观看YouTube，没安装锐速前观看YouTube 720P视频非常不流畅，经常会出现缓冲现象；而安装锐速后能流畅观看YouTube 720P视频。--from google

>1.把一键安装的脚本下载下来

```
wget -N --no-check-certificate https://raw.githubusercontent.com/wn789/serverspeeder/master/serverspeeder.sh
```

>2.脚本下载完成之后赋权执行

```
chmod +x serverspeeder.sh

bash serverspeeder.sh
```

>如果你出现了下图，就以为与当前的VPS的不支持，emmmm,当然也有解决方案，因为可能要花费的时间有点长，我就不多过介绍了，毕竟上面搭建的SS访问youtube网站等还是可以随便观看的720P。下面我会给出解决方案的连接

![img](http://qnpic.top\shadowsock%5C9.jpg)

<br><br><br>
搬瓦工购买教程：https://www.bandwagonhost.net/716.html

安装SS教程：https://medium.com/@jackme256/%E6%90%AC%E7%93%A6%E5%B7%A5-vps-%E6%90%AD%E5%BB%BA-shadowsocks-ss-%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91%E5%9B%BE%E6%96%87%E6%95%99%E7%A8%8B-ss%E5%A4%9A%E7%94%A8%E6%88%B7%E9%85%8D%E7%BD%AE%E4%BC%98%E5%8C%96-efc6dda704fe

安装锐速加速教程：http://www.gaoshilei.com/2017/11/06/SSR/

========================================

SS客户端下载地址：
MAC版下载地址：https://github.com/gaoshilei/ShadowsocksX-NG/releases/download/1.4.3-R8-build3/ShadowsocksX-NG-R8.dmg
windows版下载地址：https://github.com/iMeiji/shadowsocks_install/releases/download/0.13/ssr-win.4.7.0-fix.7z
iOS版下载地址：Rocketwingy
安卓版下载地址：https://github.com/iMeiji/shadowsocks_install/releases/download/0.13/ssr-android-3.4.0.5.apk﻿


<br><br><br>这里的博客也是到这里的就结束了，如果你不满意网速，你可以自行去安装锐锋，So,Just have fun...

