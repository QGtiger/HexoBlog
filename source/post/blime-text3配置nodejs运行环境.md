title: sublime text3配置nodejs运行环境
author: Lightfish
tags:
  - Nodejs
categories:
  - Nodejs
date: 2019-03-10 14:05:00
---
# sublime text3配置nodejs运行环境

>最近几日也是在学习nodejs，一般我都是在用vi来进行简单的编写。但是编写复杂点到的就会可能因为不当心的代码输入错误而报错，所以我就来尝试一下用Sublime Text3来进行编写。所以这篇博客也是记录下sublime下的nodejs运行环境的配置。

<!-- more -->

## 安装nodejs

>首先当然是安装nodejs咯，由于这个过于简单就略过了~~

## 下载nodejs插件并安装

>1.下载地址： https://github.com/tanepiper/SublimeText-Nodejs

![imh](http://qnpic.top\sublime_nodejs%5C4.jpg)

>2.下载下来后改名为nodejs

![ig](http://qnpic.top\sublime_nodejs%5C3.jpg)

>3.复制到sublime的插件目录下

![imh](http://qnpic.top\sublime_nodejs%5C1.jpg)

![img](http://qnpic.top\sublime_nodejs%5C2.jpg)

![img](http://qnpic.top\sublime_nodejs%5CGIF.gif)

>4.然后就是在修改其目录下的配置文件，用sublime打开Nodejs.sublime-build 和 Nodejs.sublime-settings 两个文件

* 1) 修改Nodejs.sublime-build如下图

![img](http://qnpic.top\sublime_nodejs%5C5.jpg)

* 2) 更改Nodejs.sublime-settings文件如下图(记得转义)：

![img](http://qnpic.top\sublime_nodejs%5C7.jpg)

>5.nodejs的配置到这里就基本结束了，你就可以选择nodejs的运行环境进行运行nodejs文件，这里我就来简单演示一个demo

![img](http://qnpic.top\sublime_nodejs%5C8.jpg)

![img](http://qnpic.top\sublime_nodejs%5C6.jpg)

<br><br><br>So,Just have fun...

