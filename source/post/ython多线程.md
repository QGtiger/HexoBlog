title: Python多线程
author: Lightfish
toc: true
tags:
  - 多线程
  - Python
categories:
  - Python
date: 2018-12-22 23:08:00
---
# Python多线程

>今天无聊就来写写Python多线程，至于多线程和多进程是什么，emmm，自行百度。

<!-- more -->

## 单线程

>再讲多线程之前，我先来讲讲单线程。在久远 的DOS时代，操作系统处理任务都是单线程的，所以，我们先来模拟一下。

```
from time import ctime,sleep,time

def music():

    for i in range(2):

        print("I was listening to music. %s" %ctime())

        sleep(1)

def movie():

    for i in range(2):

        print("I was at the movies! %s" %ctime())

        sleep(5)

if __name__ == '__main__':

    start = time()

    music()

    movie()

    end = time()

    print("all over %ss" % (end-start))
```


>我们先是听了音乐，通过for循环了两次，每次`time.sleep()`了一秒，然后看了会电影，每场电影5秒。我通过`print("all over %ss" % (end-start))`来获知我们花了多长时间，截图如下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywsf0q7kij30k20f1mz7.jpg)

## 多线程

>时代在进步，我们的CPU也是越来越快，CPU就开始抱怨，"P点大的事，就要占用我一点时间，其实我可以同时干好多活的"。于是操作系统进入了多任务时代。我们听着音乐，吃着火锅就不再是dream了。

>Python提供了两个模块来实现多线程，分别是`thread`和`threading`，thread有一点缺点，在threading上得到了弥补，所以我们就直接学习threading就好了，引用上面的 例子

```
import threading

from time import ctime,sleep,time


def music(func):

    for i in range(2):

        print ("I was listening to %s. %s" %(func,ctime()))

        sleep(1)

def movie(func):

    for i in range(2):

        print ("I was at the %s! %s" %(func,ctime()))

        sleep(5)

threads = []

t1 = threading.Thread(target=music,args=('爱情买卖',))

threads.append(t1)

t2 = threading.Thread(target=movie,args=('阿凡达',))

threads.append(t2)

if __name__ == '__main__':

    start = time()

    for t in threads:

        t.setDaemon(True)

        t.start()

    end = time()

    print ("all over %s" % (end-start))
```

>1.`import threading` 首先导入`threading`模块

>2.`threads = [] t1 = threading.Thread(target=music,args=('爱情买卖',)) threads.append(t1)` 创建了threads数组，用来保存线程。用`threading.Thread()`方法，创建线程，在这个方法里调用music方法 `target=`music，注意,这里的music后面不能加括号，加括号了就是函数的实现。`args`方法是对music进行传参,**注意**，传出的参数是以`元组`类型

>3.for循环遍历数组，并用`setDaemon`方法将线程声明为守护线程，必须在`start()`方法调用前设置，如果不设置为守护线程程序会被无限挂起。

>4.在下图中应该可以看到，所用时间几乎为零。这是因为子线程启动后，父线程也继续执行下去，当父线程执行完最后一条语句时，没有等待子线程，直接退出，同时子线程也一同结束。ps:运行出来应该都是0.0，可能我电脑卡了会=。=

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywsfhch1oj30l90ehgod.jpg)

>从执行结果来看，子线程`（muisc 、movie ）`和主线程`（print ("all over %s" % (end-start))）`都是同一时间启动，但由于主线程执行完结束，所以导致子线程也终止。

## 调试

>上面那方法显然不是我们想要的效果。想要解决这个问题很简单，只需要调用`join()`方法，用于等待线程终止。`join()`的作用是，在子线程完成运行之前，这个子线程的父线程将一直被阻塞。

```
import threading

from time import ctime,sleep,time


def music(func):

    for i in range(2):

        print ("I was listening to %s. %s" %(func,ctime()))

        sleep(1)

def movie(func):

    for i in range(2):

        print ("I was at the %s! %s" %(func,ctime()))

        sleep(5)

threads = []

t1 = threading.Thread(target=music,args=('爱情买卖',))

threads.append(t1)

t2 = threading.Thread(target=movie,args=('阿凡达',))

threads.append(t2)

if __name__ == '__main__':

    start = time()

    for t in threads:

        t.setDaemon(True)

        t.start()

    t.join()

    end = time()

    print ("all over %s" % (end-start))
```

成功截图如下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywsfhja6gj30ph0ejacz.jpg)

>把听歌sleep方法调到4秒，问现在运行完要几秒?

>答案是10秒左右，因为两个进程同时运行啊。取最多的线程消耗时间就好了。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywsfiaajzj30p10ewgok.jpg)

## 代码整合改进

好好看，好好学

```
from time import sleep, ctime 

import threading

def super_player(file,time):

    for i in range(2):

        print ('Start playing： %s! %s' %(file,ctime()))

        sleep(time)

#播放的文件与播放时长

list = {'爱情买卖.mp3':3,'阿凡达.mp4':5,'我和你.mp3':4}

threads = []

files = range(len(list))

#创建线程

for file,time in list.items():

    t = threading.Thread(target=super_player,args=(file,time))

    threads.append(t)

if __name__ == '__main__': 

    #启动线程

    for i in files:

        threads[i].start() 

    for i in files:

        threads[i].join()

    #主线程

    print ('end:%s' %ctime())
```


以上就是本次博客的全部内容了，基本参考博客 So

Just have fun...