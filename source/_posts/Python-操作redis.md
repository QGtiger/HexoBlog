title: Python 操作redis
author: Light Fish
date: 2019-04-14 13:06:52
tags:
---
# Python 操作redis

>本篇博客详细介绍python操作redis，python的连接方式： [点击](/2019/04/14/Python-redis/)  下面介绍详细使用

<!-- more -->

## string 操作

> set(key, value, ex=None, px=None, nx=False, xx=False)

```
r.set('name','lightfish')
'''
参数
	set(key, value, ex=None, px=None, nx=False, xx=False)
   ex, 过期时间 秒
   px，过期时间 毫秒
   nx， 设置为True，则只有key不存在时才执行，同setnx(key, value)
   xx，设置为True，则只有key存在时执行
'''
```

>setex(key, time, value)  psetex(key, time_ms, value)

```
setex(key, time, value) # 设置过期时间 秒 

psetex(key, time_ms, value) # 设置过期时间 毫秒 ms
```

>记得要先设置时间，看源码就知道为什么了，这其实就是相当于自执行redis语句，只是封装了一下

![img](http://qnpic.top/python_redis2%5C1.jpg)

>mset({key1: value1, key2,value2}) 批量设置值

```
r.mset({'name1':'lightfish', 'name2':'qgtiger'})
```

>get(key) 获取值 maget(*key)批量获取

```
r.get('name')

r.mget('name1','name2')
或者
li = ['name1','name2']
print(r.mget(li))
```

>getset(key, value) 设置新值，输入原值

```
print(r.getset('name1','lightfisher')) #输出 lightfish
print(r.get('name1')) #输出lightfisher
```

>getrange(key, start, end) 

```
r.set('name','lightfish')
r.getrange('name',0,4) #输出light
```

>setrange(key, offset value) #从指定位置开始向后替换

```
r.set('name', 'lightfish')
r.setrange('name', 1, 'z')
print(r.get('name'))  #输出 lzghtfish
r.setrange('name', 5, 'house')
print(r.get('name')) # 输出 lzghthouse
```

>setbit(key, offset, value)

```
对二进制表示位进行操作
'''
key: redis的键
offset: 位的索引，(将值对应的ASCII码转变成二进制后在进行索引)
value: 值只能是1，0 因为是修改二进制
'''

str = '345'
r.set('name', str)
for i in str:
	print(i, ord(i), bin(ord(i))) #输出值、ASCII码，对应转换的二进制
 
'''
输出：
3 51 0b110011
4 52 0b110100
5 53 0b110101
'''

r.setbit('name', 6, 0) #把第六位(0开始)转变为0，也就是上述的3对应转变为0b110001
print(r.get('name')) #输出 145
```

>getbit(key, offset)

```
获取key对应valueASCII码的二进制中的某位
r.set('name',3) #对应二进制0b110011
print(r.getbit('name',5)) #输出 0
```

>strlen(key) 输出对应value的长度

```
r.set('name', 'lightfish')
print(r.strlen('name')) #输出9
```

>incr(self, key, amount=1)

```
#自增mount对应的值，当mount不存在时，则创建mount＝amount，否则，则自增,amount为自增数(整数)
print(r.incr("mount",amount=2))#输出:2
print(r.incr("mount"))#输出:3
print(r.incr("mount",amount=3))#输出:6
print(r.incr("mount",amount=6))#输出:12
print(r.get("mount")) #输出:12
```

>incrbyfloat(self, key, amount=1.0)

```
#类似 incr() 自增,amount为自增数(浮点数)
```

>decr(self, key, amount=1)

```
#自减name对应的值,当name不存在时,则创建name＝amount，否则，则自减，amount为自增数(整数)
```

>append(key, value)

```
r.set("name","light")
print(r.get("name"))    #输出:light
r.append("name","fish")
print(r.get("name"))    #输出:lightfish
```

## hash 操作

>redis中的Hash 在内存中类似于一个name对应一个dic来存储 

> hset(name, key, value)

```
#name对应的hash中设置一个键值对（不存在，则创建，否则，修改）
r.hset("dic_name","a1","aa")
```

>hget(name,key)

```
r.hset("dic_name","a1","aa")
#在name对应的hash中根据key获取value
print(r.hget("dic_name","a1"))#输出:aa
```

>hgetall(name)

```
#获取name对应hash的所有键值
print(r.hgetall("dic_name")) #输出 {b'a1': b'aa'}
```

>hmset(name, mapping)

```
#在name对应的hash中批量设置键值对,mapping:字典
dic={"a1":"aa","b1":"bb"}
r.hmset("dic_name",dic)
print(r.hget("dic_name","b1"))#输出:bb
```

>hmget(name, keys, *args)

```
# 在name对应的hash中获取多个key的值
li=["a1","b1"]
print(r.hmget("dic_name",li))
print(r.hmget("dic_name","a1","b1"))
```

>hlen(name)、hkeys(name)、hvals(name)

```
dic={"a1":"aa","b1":"bb"}
r.hmset("dic_name",dic)

#hlen(name) 获取hash中键值对的个数
print(r.hlen("dic_name"))

#hkeys(name) 获取hash中所有的key的值
print(r.hkeys("dic_name"))

#hvals(name) 获取hash中所有的value的值
print(r.hvals("dic_name"))

'''
Output:
2
[b'a1', b'b1']
[b'aa', b'bb']
'''
```

>hexists(name, key)

```
#检查name对应的hash是否存在当前传入的key
print(r.hexists("dic_name","a1"))#输出:True
```

>hdel(name,*keys)

```
#删除指定name对应的key所在的键值对
r.hdel("dic_name","a1")
```

>hincrby(name, key, amount=1)

```
#自增hash中key对应的值，不存在则创建key=amount(amount为整数)
print(r.hincrby("demo","a",amount=2))
```

>hincrbyfloat(name, key, amount=1.0)

```
#自增hash中key对应的值，不存在则创建key=amount(amount为浮点数)
```

## list 操作

>redis中的List在在内存中按照一个name对应一个List来存储 

>lpush(name,values)

```
# 在name对应的list中添加元素，每个新的元素都添加到列表的最左边
r.lpush("list_name",2)
r.lpush("list_name",3,4,5)#保存在列表中的顺序为5，4，3，2
```

>rpush(name,values)

```
#同lpush，但每个新的元素都添加到列表的最右边
```

>lpushx(name,value)

```
#在name对应的list中添加元素，只有name已经存在时，值添加到列表的最左边
```

>rpushx(name, value)

```
#在name对应的list中添加元素，只有name已经存在时，值添加到列表的最右边
```

>llen(name)

```
# name对应的list元素的个数
print(r.llen("list_name"))
```

>linsert(name, where, refvalue, value))

```
# 在name对应的列表的某一个值前或后插入一个新值
r.linsert("list_name","BEFORE","2","SS")#在列表内找到第一个元素2，在它前面插入SS

'''参数：
     name: redis的name
     where: BEFORE（前）或AFTER（后）
     refvalue: 列表内的值
     value: 要插入的数据'''
```

>r.lset(name, index, value)

```
#对list中的某一个索引位置重新赋值
r.lset("list_name",0,"bbb")
```

>r.lrem(name, value, num)

```
#删除name对应的list中的指定值
r.lrem("list_name","SS",num=0)

''' 参数：
    name:  redis的name
    value: 要删除的值
    num:   num=0 删除列表中所有的指定值；
           num=2 从前到后，删除2个；
           num=-2 从后向前，删除2个'''
```

>lpop(name)

```
#移除列表的左侧第一个元素，返回值则是第一个元素
print(r.lpop("list_name"))
```

>lindex(name, index)

```
#根据索引获取列表内元素
print(r.lindex("list_name",1))
```

>lrange(name, start, end)

```
#分片获取元素
print(r.lrange("list_name",0,-1))
```

>ltrim(name, start, end)

```
#移除列表内没有在该索引之内的值
r.ltrim("list_name",0,2)
```

<br><br><br>Just have fun