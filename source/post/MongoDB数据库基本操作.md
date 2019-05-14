title: MongoDB数据库基本操作
author: Lightfish
toc: true
tags:
  - Python
  - MongoDB
categories:
  - Python
date: 2018-12-07 17:11:04
---
# MongoDB的操作

#### 1.连接MongoDB
>Python 想要连接MongoDB需要安装pymongo，emm，安装方法可以在cmd下，pip安装(`pip install pymongo`)
>安装完了后你就可以用一下代码连接MongoDB

<!--more-->

```
import pymongo
client = pymongo.MongoClient(host='localhost',port='27017') #默认端口27017
db = client['mydb'] #指定mydb这个数据库，如果没有会自动创建
collection = db['students']  #指定students这个集合，如果没有会自动创建一个
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqlkdgrj30tr0drtcv.jpg)

#### 2.MongoDB数据库，数据的插入

>插入数据，注意是已字典的形式。比如插入以下数据

```
student={
  'id':20180101,
  'name':'Jack',
  'age':18,
  'gender':'male'
}
用 collection.insert(student)
```
如下图
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqlo9zej314809tq5e.jpg)

>最新官方推荐`insert_one()`插入一条数据，返回的不同，是InsertOneResult对象
>在MongoDB中都有一个_id属性来唯一标识，如果没有就会自动产生一个ObjectId类型的_id属性，返回_id值
>当然也可以一次性插入多条数据  最新官方推荐 `insert_many()`来插入多条数据,或者依旧用`insert`,`collection.insert([student1,student2])`,注意是列表形式。

#### 3.MongoDB数据库的查询

>我们可以用`find_one()`或者`find()`方法来进行查询，find()返回一个生成器对象
```
result = collection.find_one({'name':'Mike'})
```
>返回的是`字典类型`  查询不存在时，则会返回`none` 记住哦： 是字典类型
>比如你想查询age等于20的若干数据，你会用什么呢
```
results = collection.find({'age':20})
for result in results:
  print(result)
```

>查询大于20呢
```
results = collection.find({'age':{'$gt': 20}})
for result in results:
  print(result)
```
>这里查询的条件键值已经不是单纯的数字，而是一个字典，比较符号`$gt` 大于
>下面是这些比较符号的总结

|符号   |  含 义    |   实 例|
|------|------|------|
|$gt  |    大于    |  {'age':{'$gt':20}}|
|$lt  |    小于    |  {'age':{'$lt':20}}|
|$gte  |    >=     |  {'age':{'$gte':20}}|
|$lte   |   <=  |    {'age':{'$lte':20}}|
|$ne    |   !=   |   {'age':{'$ne':20}}|
|$in   |   在范围内 |  {'age':{'$in':[20,23]}}|
|$nin   |  不在范围内|  {'age':{'$nin':[20,23]}}|

>还支持正则表达式，如查询以'M'开头名字的数据

```
results = collection.find({'name':{'$regex':'^M.*'}})
```

>还有简单的功能符号

```
$exists   属性是否存在    {'name':{'$exists':True}}   name属性存在
$where    高级条件查询    {'$where':'obj.fans_counts == obj.follows_counts'}   自身粉丝数等于关注数
```

#### 4.计数

* 要统计查询结果有多少条数据，可以用`count()`方法，比如统计有多少数据
```
count = collection.find().count()
```

* 或者统计符合某种条件的数据数,例如统计名字以a开头的
```
count = collection.find({'name':{'$regex':'^a.*'}}).count()
```

#### 5.排序

>排序，直接调用`sort()`方法，并在其中传入排序的字段以及升降标志
>`pymongo.ASCENDING` 升序  `pymongo.DESCENDING`  降序
```
results  = collection.find().sort('name',pymongo.ASCENDING)
print(result['name'] for result in results)
```

#### 6.偏移

>在某些情况，我们只需要提取其中的几个元素，我就可以调用`skip()`方法，示例如下:
```
results = collection.find().sort('name',pymongo.DESCENDING).skip(2)
```
>这样我们就可以跳过前两个
>另外，我们还可以用`limit()`方法来指定要去的个数,只取两个数据
```
results = collection.find({'age':{'$gt':20}}).sort('name',pymongo.ASCENDING).skip(2).limit(2)
```

#### 7.数据的更新

>对于数据的更新，我们可以用`upodate()`方法
```
condition={'name':'Mike'}
student = collection.find_one(condition)
student['age'] = 25
result = collection.update(condition,student) #update()方法是将原条件和修改后的数据传入
print(result)  #返回的是字典类型 
```
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxqlv74vj30v309r765.jpg)

>官方推荐 `update_one()`但是如果使用`update_one()`方法，第二个参数就不能传入字典，应该用`$`类型操作符作为字典的键名,示例如下：
```
result = collection.update_one(condition,{'$set':student})
```
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxta6g26j30v809nmyz.jpg)

#### 8.数据的删除

>删除的话，可以直接用`remove()`方法，符合条件的数据全部删除
>这里依然推荐两个官方推荐方法  `delete_one()`和 `elete_many()`
```
results = collection.delete_many({'age':{'$gt':20}})
print(results)  #返回的字典形式
print(results.deleted_count)  #删除的个数
```
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxta9ry4j30o006j0te.jpg)
此时也只有一条数据