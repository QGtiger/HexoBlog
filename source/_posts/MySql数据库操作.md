title: MySql数据库操作
author: Lightfish
toc: true
tags:
  - Python
  - MySql
categories:
  - Python
date: 2018-12-07 18:04:11
---
# MySql数据库的基本操作

>MySql是关系型数据，关系型数据库是基于关系模型的数据库，而关系模型是通过二维表来保存的，所以它的存储方式就是行列组成的表，每一列是一个字段，每一行是一条记录。
<br>
>在Python2中连接MySql的库大多是MySQLdb，但是这个库在Python3中不在支持，所以这里我推荐是PyMySQL。相信PyMySQL的安装大家都懂，只需要`pip install pymysql`下载到本地，然后安装，这里就不详述了。=.=

<!--more-->

### 1.数据库的连接等简单操作

```
import pymysql

db = pymysql.connect(
    host='localhost', #本地就是localhost或者127.0.0.1
    user='root', #你的MySql登录用户名
    password='', #登录密码
    port=3306  #默认端口是3306
    )
```


>取数据库的游标

```
cursor = db.cursor()
cursor.execute('SELECT VERSION()') #查看数据库的版本号 注意execute就是执行mysql语句
```
>取数据库的版本

```
data = cursor.fetchone() #fetchone()就是获取第一条数据，也就是上面的那个版本号
print('Database version:' + data[0])
```
>建了一个名为spider 的数据库，并且默认编码是utf-8

```
cursor.execute('CREATE DATABASE spider DEFAULT CHARACTER SET utf8') #default character set utf8 注意没有'-'哦
```
>选中数据库

```
cursor.execute('USE spider') 
```
>建表

```
sql = 'CREATE TABLE IF NOT EXISTS students (id VARCHAR(255) NOT NULL,name VARCHAR(25)NOT NULL,age INT NOT NULL,PRIMARY KEY (id))'
cursor.execute(sql)
```
这里就是成功截图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxpx0ffqj30r10iq42f.jpg)

emmm，我用的是mysql的一个可视化工具`navicat`,你可以到这来[下载](https://pan.baidu.com/s/13BDqu8idklKPbYO1HPO5TQ) 密码: 5w3y  
下面就是真正的mysql基本操作了

### 2.插入数据

```
id = '2016210405068'
name = '言语'
age = 18
sql = 'INSERT INTO students(id,name,age) values(%s,%s,%s)' #sql语句
```
>插入数据的标准写法

```
try:
    cursor.execute(sql, (id, name, age))
    # 需要执行db对象的commit()方法才能实现数据的插入，对于数据的插入、更新、删除操作都需要调用
    print('Sucessful insert...')
    db.commit()
except Exception as e:
    print(e)
    # 加了一层异常处理，如果执行失败，则调用rollback()函数来执行数据的回滚，相当于什么都没有发生
    db.rollback()
```

>**注意** 在很多情况下我们要达到的效果是插入方法无需改动，只需要传入一个字典就行，比如构造这个字典：

```
data = {
    'id': '2016210405068',
    'name': 'lightfish',
    'age': 22
}
table = 'students' #数据库表
keys = ','.join(data.keys())  # str类型
values = ','.join(['%s'] * len(data))
sql = 'INSERT INTO {table}({key}) VALUES({values})'.format(table=table, key=keys, values=values)
try:
    if cursor.execute(sql, tuple(data.values)): #元组类型
        print('Sucessful insert')
        db.commit
except Exception as e:
    print(e)
    db.rollback()
```

### 更改数据
```
sql = 'UPDATE students SET age=%s WHERE name=%s'
try:
    cursor.execute(sql, (22, 'Bob'))
    db.commit()
except Exception as e:
    print(e)
    db.rollback()
```

>#### **注意** 下面这个很重要

>更新数据的时候，我们关心会不会出现重复的问题。所以我们这里可以再实现一种去重的方法，如果数据存在，则更新数据；否则插入数据

```
data = {
    'id': '2016210405068',
    'name': '辰东',
    'age': 33
}

table = 'students'
keys = ','.join(data.keys())
values = ','.join(['%s'] * len(data))

# on duplicate key update

sql = 'INSERT INTO {table}({keys}) VALUES ({values}) ON DUPLICATE KEY UPDATE'.format(table=table, keys=keys,
                                                                                     values=values)
update = ','.join([' {key}=%s'.format(key=key) for key in data.keys()])
sql += update

try:
    cursor.execute(sql, tuple(data.values()) * 2)
    print('Sucessfull insert...')
    db.commit()
except Exception as e:
    print(e)
    db.rollback()
```

>完整的SQL语句是
```
INSERT INTO students(id, name, age) VALUES (%s, %s, %s) ON DUPLICATE KEY UPDATE id = %s, name = %s, age = %s
```

### 删除数据

>删除数据就相对简单了直接用`DELETE`，但是依然要用`commit()`函数才能生效

```
table = 'students'
conditions = 'age>20'
sql = 'DELETE FROM {table} WHERE {condition}'.format(table=table, condition=conditions)
try:
    cursor.execute(sql)
    print('Delete sucessfull...')
    db.commit()
except Exception as e:
    print(e)
    db.rollback()
```

### 查询数据库

> **注意**`fetch`方法内部实现有一个偏移，开始的`fetchone()`就获取了一条数据，所以后面的`fetchall()`只获取了总数减一条，因此我推荐后面一种方法，用`while`方法加`fetchone()`,`fetchall()`会将结果以元组的形式全部返回，如果数据很大就占用的开销很大。

```
try:
    cursor.execute(sql)
    print('Counts: '+cursor.rowcount)
    one = cursor.fetchone()
    print('One: '+one)
    results = cursor.fetchall()
    print('Results: '+results)
    print('The type of results: '+type(results))
    for row in results:
        print(row)
except Exception as e:
    print(e)
```

>第二种方法

```
try:
    cursor.execute(sql)
    print('Counts: '+cursor.rowcount)
    one = cursor.fetchone() #one是一个元组
    while one:
        print('Row: ',one)
        one = cursor.fetchone()
except Exception as e:
    print(e)
``` 

<br><br><br>
以上就是Mysql的基本操作，So<br><br>Just have fun...