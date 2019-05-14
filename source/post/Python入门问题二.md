title: Python入门问题二
author: Lightfish
toc: true
tags:
  - Python
  - Python基础
categories:
  - Python
date: 2018-12-06 00:25:48
---
# Python入门问题二

### 1.简述Django的ORM
>ORM，全拼Object-Relation Mapping，意为对象-关系映射实现了数据模型与数据库的解耦，通过简单的配置就可以轻松跟换数据库，而不需要修改代码只需要面向对象编程，ORM操作本质上会根据对接的数据库引擎，翻译成相对应的sql语句，所有使用Django开发的项目无需关系程序底层使用的是MySql、Oracle、sqlite...，如果数据库迁移，只需要更换Django的数据库引擎就好了

<!-- more -->
### 2.一行代码展开二维列表
```
import numpy as np
a = [[np.random.randint(1,10),np.random.randint(1,10)] for _ in range(3)]
print(a)
x = [j for i in a for j in i]
print(x)
```
![img](https://lightfisher.github.io/img/Python_question/26.jpg)

* 将列表装换成numpy矩阵，通过numpy的flatten()方法
```
import numpy as np
a = [[np.random.randint(1,10),np.random.randint(1,10)] for _ in range(3)]
print(np.array(a).flatten().tolist())
```

![img](https://lightfisher.github.io/img/Python_question/27.jpg)

### 3.举例说明异常模块中try，except，else，finally的相关意义

* try...except...else 没有获取异常，执行else语句
* try...except...finally 不管有没有获取异常都会执行finally语句

### 4.举例说明zip函数的用法

* zip()函数在运算的时候，会以一个或多个序列(可迭代对象)作为参数，返回一个元组的列表。同时将这些序列中并排的元素配对
* zip()参数可以接受任何类型的序列，同时还可以有两个以上的参数；当传入参数不等时，zip能够自动以最短序列长度为准进行截取，并获得元组

![zip](https://lightfisher.github.io/img/Python_question/28.jpg)

### 5.列出常见的状态吗和意义
* `200` `OK` 请求正常处理完毕
* `204` `No Content` 请求成功处理，没有实体的主体返回
* `206` `Partial Content` GET范围请求已成功处理
* `301` `Moved Permanently` 永久重定向，资源已永久分配新URL
* `302` `Found` 临时重定向，资源已临时分配新URL
* `303` `See Other` 临时重定向，期望使用GET定向获取
* `304` `Not Modified` 发送的附带条件不足请求为满足
* `307` `Temporary Redirect` 临时重定向，POST不会变成GET
* `400` `Bad Request` 请求报文错误或参数不足
* `401` `Unauthorized` 需要通过HTTP认证，或认证失败
* `403` `Forbidden` 请求资源被拒绝
* `404` `Not Found` 无法找到请求资源(服务器无法拒绝)
* `500` `Internal Server Error` 服务器故障或Web应用故障
* `503` `Service Unavailable` 服务器超负载或停机维修