title: Nodejs模板引擎ejs
author: Light Fish
tags:
  - Nodejs
categories:
  - Nodejs
date: 2019-03-15 17:49:00
---
# Nodejs模板引擎ejs

>最近也是在学习nodejs，也是想去写个小项目跟数据库有着操作，所以就想是不是跟Django的模板引用差不多。所以如这个标题一样ejs引擎就是这次我写项目享用的模板引擎。今天也是来简单记录下ejs的学习。

<!-- more -->

# 一个简单的例子

>捡来看一下处理字符串的小例子

```
var ejs = require('ejs')//ejs 后台模板引擎
var ejs=require("ejs");
var str="很高兴,哈哈哈今天是<%= y%>年<%= n%>月<%=m%>号";
var data={
	y:2019,
    n:3,
    m:15
}
var test=ejs.render(str, data);
console.log(test);//自动解析成很高兴,哈哈哈今天是7月4号
```

>上面例子很简单,使用<%=  %>里面填入需要解析的变量,使用ejs.rander(str,data);str是需要解析的参数,data是存储的解析数据!

# 模板

>下面来看一下较为复杂的一个例子

>创建模板文件test.ejs(后缀名无所谓)

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>ejs模板引擎</title>
</head>
<body>
    <h2>nice 今天天气挺不错 今天是<%= y %>年<%=m%>月<%=d%>日 看一下今天的电影排行条目</h2>
    <ul>
 <% for(var i=0;i<news.length;i++){ %>
 <li>电影名称:<%=news[i].title  %> 阅读人数: <%= news[i].reader %> 上映时间: <%= news[i].date %></li>
 <% }%>
 </ul>
    <h2>一下是阅读量大于1000的电影条目</h2>
    <ul>
 <% for(var i=0;i<news.length;i++){%>
 <% if(news[i].reader>1000) { %>
 <li>电影名称:<%=news[i].title  %> 阅读人数: <%= news[i].reader %> 上映时间: <%= news[i].date %></li>
 <%}%>
<%}%>
 </ul>

 <h2>上映时间是2012年的</h2>
 <ul>
     <%for(var i = 0;i<news.length;i++){
        if(news[i].date=='2012年'){
     %>
        <li>电影名称:<%=news[i].title%> 阅读人数: <%=news[i].reader%> 上映时间: <%=news[i].date%></li>
     <%
    }}%>
 </ul>
</body>
</html>
```

>在此情况下,不在一个文件内,需要先进行文件的读取后执行,模板解析操作,看一下node代码

```
// ejs模板引擎读取内容
var http=require('http');
var ejs=require('ejs');
var fs=require('fs');
const server=http.createServer(function(req,res){
    //读取文件
    fs.readFile("./test.ejs",function(err,data){
        // console.log(data.toString())
        var template=data.toString();
        var shuju={
            y:2019,
            m:3,
            d:15,
            news:[
                {'title':"霸王别姬","reader":"864","date":"2012年"},
                {'title':"大明王朝1566","reader":"15864","date":"2006年"},
                {'title':"黄金搭档2012","reader":"854","date":"2012年"},
                {'title':"大明王朝1566","reader":"15864","date":"2006年"},
                {'title':"黄金搭档2012","reader":"854","date":"2012年"},
                {'title':"大明王朝1566","reader":"15864","date":"2006年"},
                {'title':"黄金搭档2012","reader":"854","date":"2012年"}
            ]
        }
        var html=ejs.render(template,shuju);
        res.writeHead(200,{"content-type":"text/html"});
        res.end(html);
    })


})
server.listen(3000,"127.0.0.1");
```

>成功截图

![img](http://qnpic.top/ejs_test%5C1.jpg)

# 简单总结

>对于上面的案例简单总结一些,`<% %>`中间可以包含任意的js语句,但是在输出内容(纯数据的调用)的时候需要使用<%=  "这里是调取的数据" %>,这里的数据在案例中包含在数据这个变量中,总体调取的是new数组下面的对象,在调取数据的时候无需强调数据下面的news,直接使用news.对象属性就行,因为在ejs处理的时候已经声明了数据是调取的数据文件,而读取的ejs文件是一个模板文件.

<br>

>ejs模板引擎api地址:https://www.npmjs.com/package/ejs

<br><br><br>So,just have fun..