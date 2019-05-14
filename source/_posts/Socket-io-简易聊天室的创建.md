title: Socket.io 简易聊天室的创建
author: Light Fish
date: 2019-03-04 16:18:21
tags:
---
# Socket.io 开发简易聊天室

>这几天可能因为开学有点迷茫，学习的方向有点糊涂了，就无聊学习了一下Socket.io。希望能够慢慢找回状态。哎~~~

<!-- more -->

## 前言

>Socket.io 是一个用来实现实时双向通信的框架，其本质是基于 WebSocket 技术。所以我先来简单的聊聊WebSocket技术，这里就简单的举一个例子：

* 用户小A，打开了某个网站的充值界面，该界面上有一个付款的二维码。

* 当小A 用某宝的 APP 扫码付款之后，网页要自动跳转到付款成功的界面。最简单的方法就是网页每隔一段时间就请求一次服务器——“怎么样？那货付款没有？”，“怎么样？还没付吗？”，“怎么样？这次总该付了吧”。

* 当接收到服务端返回确认付款的标识后，页面再进行跳转。

>这种定时对服务端发送 HTTP 请求的技术也被称为“轮询”。轮询的缺点也显而易见，短时间内的多次访问无疑会对服务器造成很大压力。

>所以后来的人们发明一个方法——“长轮询”技术，这与短轮询的区别在于，每次客服端请求服务器后，服务器并不会立即返回“用户还未付款”的结果，而是一直将HTTP保持在挂起的状态，只有当用户付款后才会返回给客服端 “用户已付款”的结果。当然如果用户在规定的时间内没有付款还是会断开连接。相比短轮询，长轮询有一定的优势，但是这还不够好。直到HTML5里的WebSocket技术的诞生。

>`WebSocket`，即 Web 浏览器与 Web 服务器之间的全双工通信标准。最初 WebSocket 只是作为 HTML5 标准的一部分，而后来却逐渐变成了独立的协议标准。一旦浏览器与服务器建立起 WebSocket 协议的通信连接，之后所有的通信都依靠该协议进行。不论是服务器还是浏览器，任何一方都能向对方发送报文。通信过程中可相互发送 JSON、HTML或图片等任意格式的数据。

## 聊天室的简单实现

>这里入门的话，我推荐的是[Socket.io的中文文档](https://www.w3cschool.cn/socket/socket-ulbj2eii.html),这里的话，你一步步走下就基本会了。我这里也是上传到我的github上去了。——[chat 聊天室](https://github.com/QGtiger/node_project/tree/master/chat)

>所以我这里就简单展示几张图

![img](http://qnpic.top\Socket_chat%5C1.jpg)

![img](http://qnpic.top\Socket_chat%5C2.jpg)

![img](http://qnpic.top\Socket_chat%5C3.jpg)

<br><br><br>So, Just have fun...

