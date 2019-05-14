title: Python-redis
author: Light Fish
tags:
  - Python
categories:
  - Python
date: 2019-04-14 11:03:00
---
# redis

>redis 是一个key-value存储系统。和Memcached类似，它支持存储的value类型相对更多，包括string(字符串)、list(链表)、set(集合)、zset(有序集合)和hash(哈希)。这些数据都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是`原子性`的。在此基础上，redis支持各种不同方式的排序。与memcached一样，为了保证效率，数据都是缓存在内存中。区别是redis周期性的把更新数据写入磁盘或者把修改操作写入追加的记录文件中，并且在此基础上实现master-slave(主从)同步。

<!-- more -->

>redis是一个高性能的key-value数据库。redis的出现，很大程度补偿了memcached这类key-value存储的不足，在部分场合可以对关系型数据库起到很好的补充作用。它提供了Python、Ruby、Erlang、PHP等，使用很方便。数据可以冲主服务器向任意数据的从服务器上同步，从服务器也是可以关联其他从服务器。这使得redis可执行单层树复制。由于完全实现发布/订阅机制，使得从数据库在任意同步树时，可订阅一个频道并接受主服务器完整的消息发布记录。今天就简单讲一下python操作redis和发布/订阅机制。

# Python操作redis

### 1.连接方式

>redis-py提供两个类Redis和StrictRedis用于实现redis的命令。(记得pip install redis 安装redis库)

```
import redis

r=redis.StrictRedis(host='127.0.0.1', port=6379, db=0)
r.set('name', 'LightBlog')
print(r.get('name'))
```

### 2.连接池

>redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。默认，每个Redis实例都会维护自己的连接池。可以直接建立一个连接池，然后作为参数，实现多个Redis实例共享一个连接池。

```
import redis

pool = redis.ConnectionPool(host='127.0.0.1', port=6379)
r = redis.Redis(connection_pool=pool)
r.set('name', 'Lightfish')
print(r.get('name'))
```

### 3.详细操作

>redis详细操作

### 4.管道

>redis-py默认在执行每次请求都会创建(连接池申请连接)和断开(归还连接池)一次连接操作，如果想要在一次请求中指定多个命令，则可以使用pipline，一次pipline原子性操作

```
import redis

pool = redis.ConnectionPool(host='127.0.0.1', port=6379)
r = redis.Redis(connection_pool=pool)

pipe = r.pipeline(transaction=True)

r.set('name', 'name1')
r.set('name2', 'name2')

pipe.execute()
print(r.get('name'))
```

### 5.发布/订阅

>首页创建一个RedisHelper.py文件，里面定义一个类，定义频道monitor，并定义发布和订阅的方法

```
import redis

class RedisHelper(object):
    def __init__(self):
        self.conn = redis.Redis(host='127.0.0.1',port=6379)
        self.channel = 'monitor' # 创建频道
    
    def publish(self, msg):
        self.conn.publish(self.channel, msg)
        return True

    def subscribe(self): #订阅
        pub = self.conn.pubsub()
        pub.subscribe(self.channel)
        pub.parse_response()
        return pub
```

>然后创建发布端，写一个publish-client.py文件，代码如下

```
from RedisHelper import RedisHelper

obj = RedisHelper()

while True:
    msg = input('input what u want to say("q" to exit):')
    if msg == 'q':
        break
    obj.publish(msg) #发布
```

>创建订阅端，写一个pubsub-client.py文件

```
from RedisHelper import RedisHelper

obj = RedisHelper()
sub = obj.subscribe()

# 以下方法是输入接收数据的类型，订阅号和数据如 [b'messgae', b'monitor', b'hello world']
# while True:
#     msg = sub.parse_response()
#     print(msg)

# 以下方法是创建监听然后，如果type是message类型就输入数据 data
for item in sub.listen():
    if item['type'] == 'message':
        print(item['data'])
```

![img](http://qnpic.top/python_redis1%5C1.jpg)

![img](http://qnpic.top/python_redis1%5C2.jpg)

<br><br><br>这次的博客也是简单介绍下了python操作redis的一些操作。还有redis的发布/订阅功能的实现，So<br><br>Just have fun...