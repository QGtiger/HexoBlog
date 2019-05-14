title: KOD 开源免费的私人网盘创建
author: Lightfish
tags:
  - 干货
categories:
  - 干货
date: 2019-03-07 20:10:00
top: true
---
# KOD 开源免费的私人网盘

>如今百度云网盘限速严重，而Dropbox、Google Drive 必须科学上网(emm,我记得我写过一篇科学上网，现在好像不行了)，似乎除了 Office365 (`OneDrive`)和 `MEGA` 外也没太多选择了，所以很多人开始用电脑/NAS/VPS作为服务器自建私人网盘。所以今天我就来安利一款——KODExplorer。

<!-- more -->

>KODExplorer 可道云 (原名芒果云) 是一款基于 PHP 开发的开源 WEB 网页版轻量级私有云网盘工具。准确点来说，它是一款功能丰富的在线文件管理系统！或者叫“云桌面”会更加合适。随时随地在线连接，访问自己的网盘内容，而且除了「文件管理」功能以外，KODExplorer 还提供了一个“云桌面”，让你在浏览器上也能体验到与 Windows 相似的系统，并且该桌面还集成网页版微信、ProcessOn 在线制图工具、石墨文档、百度脑图等在线应用。好处有点多，我这里就不多说了，如果你想知道具体的好处，可以看这篇博客——[可道云](https://www.iplaysoft.com/kodexplorer.html),你也可以来这个官方演示demo查看——[demo演示](http://demo.kodcloud.com/index.php),密码/账户都是demo。现在就来简单记录下如何搭建

## 搭建

>1.我记得之前我安利过一款，`000webhost 免费虚拟云主机`,你可以查看这篇博客——[000webhost 免费虚拟云主机](/2019/02/09/000webhost-免费虚拟云主机/),空间算是免费里面算好的了，存储一下简单的文档和文件还是绰绰有余。

>2.至于如何注册账户和申请到云空间，我这里就不多说了，我介绍这个就已经阐述过，你可以看这篇博客——[000webhost 免费虚拟云主机](/2019/02/09/000webhost-免费虚拟云主机/)

>3.你可以直接去这里进行下载——[官方下载页面](https://kodcloud.com/download/). 或者点击这里下载——[百度云盘](https://pan.baidu.com/s/14_NFLAfVOctOsjOTLoaHMQ) 提取码: eepk 或者这个[蓝奏云下载](https://www.lanzous.com/i3cnt0h) 或者这个[可道云下载](http://lightfish.cf/index.php?share/file&user=1&sid=u7P4fdFI)  （随便调一个就行）

![img](http://qnpic.top\kod%5C1.jpg)

>4.这里的话我就默认都已经注册到了自己的云主机，我就直接演示(但是由于我写这篇博客的时候，这个网站好像我请求太多了，叫我等会时间再注册一个云主机，所以我就没去搭建一个云主机，在我的原来这个上演示，具体步骤知道就行)。言归正传，看了我上篇的那个简单介绍这个虚拟云主机的博客，相信都已经会上传文件，其实只需要讲下载的文件放到网站更目录下就行了，我这里就简单介绍一下压缩文件的上传

![img](http://qnpic.top\kod%5C2.jpg)

>5.右键单击压缩文件，进行解压

![img](http://qnpic.top\kod%5C3.jpg)

>6.如果出现下图就说明可以正常解压，如果不是就刷新、

![img](http://qnpic.top\kod%5C4.jpg)

>7.解压后就是就是加压出来个压缩文件，这个时候你其实也是可以进行访问的只需要在你的网址后面加上这个文件夹的名字，但是问了好看，你就需要将这个文件夹里的文件移动到根目录下。选中这个文件夹下的所有文件点击下图的移动按钮(注意我这里只是演示全选，你的应该是解压出来的文件下的所有文件)

![img](http://qnpic.top\kod%5C5.jpg)

>8.选择移动到根目录

![img](http://qnpic.top\kod%5C6.jpg)

>9.这个时候就可以正常访问你的私人云盘了，第一次访问应该会叫你设置密码

![img](http://qnpic.top\kod%5C7.jpg)

>10.这次的博客也是基本到这里了，So

<br><br><br>Just have fun.
