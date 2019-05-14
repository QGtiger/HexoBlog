title: GitBook 安装教程
author: Light Fish
tags:
  - Git
  - GitBook
categories:
  - Git
date: 2019-01-28 16:28:00
---
# GitBook 安装教程

>GitBook 是一个基于 Node.js 的命令行工具，支持 Markdown 和 AsciiDoc 两种语法格式，可以输出 HTML、PDF、eBook 等格式的电子书，这里就简单介绍下GitBook的安装和基本使用。

<!-- more -->

## 安装node.js

>1.去node.js官网下载安装包，根据自己电脑下载对应的版本，[下载地址](http://nodejs.cn/download/)

![node](http://qnpic.top\gitbook%5C3.jpg)

>2.一路默认下去，这里就截图说明安装node.js时候，node.js和npm都被安装了，并加入了环境变量(node.js和npm的关系类似python和pip的关系，npm就是一个包管理工具)

![img](http://qnpic.top\gitbook%5C4.PNG)

>3.安装完成就可以通过命令看版本号`node -v`和`npm -v`，验证了安装成功

![img](http://qnpic.top\gitbook%5C5.jpg)

## 安装GitBook

>4.执行命令 `npm install -g gitbook-cli` .下载好后出现了版本号，如下图

![img](http://qnpic.top\gitbook%5C1.jpg)

## 安装GitBook-editor编辑软件

>5.网上下载gitbook-editor软件,[下载地址](https://legacy.gitbook.com/editor) (https://legacy.gitbook.com/editor), 基本安装就行了

![img](http://qnpic.top\gitbook%5C2.jpg)

>6.无需登录，可以直接跳过

![img](http://qnpic.top\gitbook%5C6.jpg)

>7.**注意：** 该界面我们可以更改默认文件保存路径，点击菜单`GitBook Editor`菜单下的`Chang Library Path`,创建一个自己的文件存储路径

![img](http://qnpic.top\gitbook%5C7.jpg)

>8.点击新书名称，默认用英文

![imig](http://qnpic.top\gitbook%5C8.jpg)

>9.新建后打开首页

![img](http://qnpic.top\gitbook%5C9.jpg)

>10.修改每一个章节的名称

![img](http://qnpic.top\gitbook%5C10.jpg)

>11.左侧菜单可以添加目录或者子目录，右键添加

![img](http://qnpic.top\gitbook%5C11.jpg)

>12.如果你喜欢或熟悉`markdown`格式来编辑，你可以点击<font color="red"><b>右下角</b></font>不起眼打问号，这里有Markdown编辑开启预览模式，也有`setting`修改配置，开启后会分屏，在编辑的时候实时预览

![img](http://qnpic.top\gitbook%5C12.jpg)

>13.每次编辑完了后，记得保存哦~~

>14.保存完了后，你可以在文件保存的所在目录下(shift+右键进入cmd界面)，输入命令`gitbook serve`(注意，是`serve`不是server)，在线网页预览，一般预览地址是 `localhost:4000`

![img](http://qnpic.top\gitbook%5C13.jpg)

![img](http://qnpic.top\gitbook%5C14.jpg)

>15.我们看下文件夹下面有什么，运行`gitbook serve`之前是没有静态网页的，运行成功后就会自动生成。如果你想做一个web电子书、文档、教程、而且自身前端水平不太好的情况下，就可以拿生成的html来嵌入web框架当做模板。

![img](http://qnpic.top\gitbook%5C15.jpg)

<br><br><br>So,这次的GitBook简单安装和使用教程就到这了
<br><br>Just have fun...















