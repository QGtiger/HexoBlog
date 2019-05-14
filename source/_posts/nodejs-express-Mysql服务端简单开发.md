title: nodejs + express + Mysql服务端简单开发
author: Light Fish
tags:
  - Nodejs
categories:
  - Nodejs
date: 2019-03-17 17:31:00
---
# nodejs + express + Mysql服务端简单开发

> 这个也是我学习nodejs几天来的第一个项目，所以也是简单记录一下。用的是Express应用生成器工具`express-generator`快速生成一个应用的骨架。模板引擎用的是`ejs`,关于该模板引擎的介绍可以看这篇博客——[Nodejs模板引擎ejs](/2019/03/15/Nodejs模板引擎ejs/).

<!-- more -->

## 准备工作

> 首先是需要安装 NodeJS 环境，这里就不再做介绍

> 1. 安装 Express

```javascript
npm install express -g //全局安装
npm install express-generator -g //全局安装
```

> 2.初始化项,运行完下面这个命令行后就会生成一个名为student的文件夹，里面的目录结构如下图

```javascript
express -e student //-e 这里用ejs引擎模板
```

![img](http://qnpic.top/express_mysql%5C1.jpg)

> 3.执行如下命令行

```javascript
cd student //进入项目文件夹
npm install //安装项目依赖
```

> 4.最终目录

![img](http://qnpic.top/express_mysql%5C2.jpg)

> 5.安装mysql模块

```javascript
npm install mysql --save
```

> 6.整个项目用的是ejs模块，所以要安装

```javascript
npm install ejs
```

## 基本介绍

```
/bin : 用来启动应用服务器
/public: 存放静态资源目录
/routes：路由用于确定应用程序如何响应对特定端点的客户机请求，包含一个 URI（或路径）和一个特定的 HTTP 请求方法（GET、POST 等）。每个路由可以具有一个或多个处理程序函数，这些函数在路由匹配时执行。
/views: 模板文件所在目录
/app.js 这个是服务器启动的入口
```

## 具体基本步骤

> 由于篇幅的有限这里就简单只介绍其中的一个功能，主要也是大致能够懂得该项目的基本实现过程。所以我这个介绍的是添加信息和首页的实现。

#### 首页的实现

> 1.数据库连接，在项目目录下创建了config目录，并在里面创建了db.js,里面是是nodejs连接mysql的配置

```javascript
const mysql = require('mysql');

const DB = {
	host: 'localhost',
	port: 3306,
	user: 'root',
	password: '111111',
	database: 'student'
}

const DBConnection = mysql.createConnection(DB);

DBConnection.connect(function(err){
	if(err){
		console.log('error connected: '+err.stack);
		return;
	}
	console.log('connected successful as id '+DBConnection.threadId);
});

module.exports.DBConnection = DBConnection;
```

> 2.根据app.js的路由配置

```javascript
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

app.use('/', indexRouter);
app.use('/user', usersRouter);
```

> 3. '/' 分配给了'./routes/index' ,下面是index.js里的代码

```javascript
var express = require('express');
var router = express.Router();
var db = require('./../config/db.js'); //这里我实在项目下创建了config目录，并在里面创建了db.js,里面是是nodejs连接mysql的配置

var DBConnection = db.DBConnection;
var sql = 'SELECT * FROM STUDENT';

/* GET home page. */
router.get('/', function(req, res, next) {
  DBConnection.query(sql,function(err,rows){
  	if(err){
  		res.render('index', { title: '学生成绩录入系统', data:[] });
  	}else{
  		res.render('index', { title: '学生成绩录入系统', data:rows });
  	}
  });
});

module.exports = router;

```

> 4.res.render('index'),这里的话和Django的模板使用是一样的，index是模板文件，后面的传递的数据。下面是'index.ejs'的html代码

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %>   <a href="/user/add">添加学生信息</a>  <a href="/user/search">查询学生信息</a></p>
    <table class="std">
    	<tr>
    		<th>编号</th>
    		<th>姓名</th>
    		<th>语文</th>
    		<th>英语</th>
    		<th>数学</th>
    		<th>操作</th>
    	</tr>
    	<% for(var i = 0; i<data.length;i++){ %>
    		<tr>
    			<td><%=data[i].id%></td>
    			<td><%=data[i].name%></td>
    			<td><%=data[i].chinese%></td>
    			<td><%=data[i].math%></td>
    			<td><%=data[i].english%></td>
    			<td>
    				<a href="/user/del/<%=data[i].id%>">删除</a>
    				<a href="/user/toupdate/<%=data[i].id%>">修改</a>
    			</td>
    		</tr>
    	<%}%>
    </table>
  </body>
</html>

```

> 5.这样的话就完成了首页的实现，截图如下

![img](http://qnpic.top/express_mysql%5C3.jpg)



#### 添加信息的实现

> 1.添加信息的实现也是在app.js中将 '/user'  分配给了users.js路由配置文件，由于里面的代码太多，这里有只展示实现信息添加的一部分

```javascript
var express = require('express');
var router = express.Router();
var db = require('./../config/db');

var DBConnection = db.DBConnection;

router.get('/add',function(req,res,next){
	res.render('add',{title:"学生信息添加"});
});

router.post('/add',function(req,res,next){
	var name = req.body.name;
	var chinese = req.body.chinese;
	var math = req.body.math;
	var english = req.body.english;
	var sql = "INSERT INTO STUDENT(name,chinese,math,english) values('"+name+"','"+chinese+"','"+math+"','"+english+"')";
	DBConnection.query(sql,function(err,rows){
		if(err){
			res.send('添加学生信息失败...');
		}else{
			res.redirect('/'); //重定向
		}
	});

});
```

> 2.'add.ejs'  模板代码

```html
<!DOCTYPE html>
<html>
<head>
	<title><%=title%></title>
</head>
<body>
	<h2><%=title%></h2>
	<form action="/user/add" method='post'>
		<label for="name">姓名</label>
		<input type="text" name="name" id="name">
		<br>
		<label for="chinese">语文</label>
		<input type="text" name="chinese" id="chinese">
		<br>
		<label for="math">数学</label>
		<input type="tex" name="math" id="math">
		<br>
		<label for="english">英语</label>
		<input type="text" name="english" id="english">
		<input type="submit" value="提交">
	</form>
</body>
</html>
```

> 3.实现如下

![img](http://qnpic.top/express_mysql%5CGIF.gif)

<br><br><br>

这次的博客也是介绍了mysql和express-generator项目生成器的简单使用So,<br><br>Just havefun