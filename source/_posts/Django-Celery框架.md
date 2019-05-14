title: Django Celery框架
author: Light Fish
tags:
  - Django
categories:
  - Django
date: 2019-05-05 19:17:00
---
# 分布式任务与定时任务

>网站的并发编程主要处理网站的业务流程，根据网站请求到响应的过程分析，Django处理用户请求主要在视图中执行，视图主要是一个函数，而且是`单线程`的，没错是单线程的。所以在遇到繁琐的数据读写和高密度的计算，往往会造成响应时间过长，在网页上出现卡死的情况，不利于用户体验，为了解决这样的情况，我们可以在视图中加入分布式任务，让它处理一些耗时的业务流程，从而缩短用户响应时间。

<!-- more -->

>Django 的分布式主要是由`Celery框架`实现，这是Python开发的分布式任务队列。它支持使用任务队列的方式在分布的机器、进程和线程上执行任务调度。Celery侧重于实时操作、用于生产系统每天处理数以百万的任务。Celery本身不提供消息存储服务，它使用第三方消息服务来传递。目前支持RabbitMQ、Redis和MongoDB等。这次就是用Redis，至于Redis的安装和使用可以自行百度一下。

## Celery配置所需环境

>安装所需要的功能模块：celery、redis、django-celery-results、django-celery-beat和eventlet

* 1.celery: 安装Celery框架，实现分布式任务调度

* 2.redis： 使Python和redis实现连接

* 3.django-celery-results： 基于Celery基础上封装的方式，主要适用于Django

* 4.django-celery-beat: 基于Celery基础上封装的定时任务功能

* 5.eventlet: Python的协程并发库，这是实现并发模式之一

>至于安装就是`pip install` 就行

## Django 配置

>这里演示的Django项目是`django_celery`,所以需要在该项目下的django_celery文件夹创建`celery.py`和在项目应用index下创建`tasks.py`,目录结构如下：

![img](http://qnpic.top/django_celery%5C1.jpg)

>然后在`celery.py`和`tasks.py`文件中分别编写功能代码，celery.py文件代码基本是固定的，而tasks.py的代码可以根据需求自行编写。两者代码如下：

```
# celery.py 文件
from __future__ import absolute_import, unicode_literals
import os
from celery import Celery

# 获取settings 的配置信息
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'django_celery.settings')

# 定义Celery对象，并将项目配置信息加载到对象中去
# Celery的参数一般以项目名命名
app = Celery('django_celery')

# 配置文件需要写在setting.py中，并且配置项需要使用`CELERY_`作为前缀
app.config_from_object('django.conf:settings', namespace='CELERY')

# 能够自动加载所有在django中注册的app，也就是setting.py中的INSTALLED_APPS
app.autodiscover_tasks()

# 测试代码
@app.task(bind=True)
def debug_task(self):
    print('Request: {0!r}'.format(self.request))

```

```
# tasks.py 文件

from __future__ import absolute_import, unicode_literals
from celery import shared_task
import time


@shared_task
def test():
    # 测试是否异步，设置time.sleep(6)
    start = time.time()
    time.sleep(6)
    end = time.time()
    return '测试6秒,time={}'.format(end-start)


# 该任务用于定时任务执行
@shared_task
def timing():
    now = time.strftime('%H:%M:%S')
    with open('output.txt', 'a+') as f:
        f.write('Now is {}\n'.format(now))


```

>从代码中可以得知，改文件是将Celery框架进行实例化并生成app对象，现在将app对象与django_celery项目进行连接，使项目可以执行分布式任务，再`celery.py`文件同级目录的`__init__.py`中编写相关代码，在django_celery初始化的时候，Django会自动加载app对象，__init__.py文件代码如下：

```
from __future__ import absolute_import, unicode_literals
from .celery import app as celery_app
__all__ = ['celery_app']
```

>在上述的`tasks.py`文件中可以看到分布式任务test只是time.sleep了6秒。下面是settings.py中进行相关的配置：

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 添加分布式任务功能
    'django_celery_results',
    # 添加定时任务功能
    'django_celery_beat',
    # index app注册
    'index',
]
```

```
# 数据库配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'celery_test',
        'USER': 'root',
        'PASSWORD': '111111',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}


# 设置存储celery任务队列的Redis数据库
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
# 设置存储Celery 任务结果的数据库
CELERY_RESULT_BACKEND = 'django-db' #这个就是django-db，django的命名规范

# 设置定时任务的相关配置
CELERY_ENABLE_UTC = False
CELERY_BEAT_SCHEDULER = 'django_celery_beat.schedulers:DatabaseScheduler'
```

>配置完成后记得进行数据库迁移，因为分布式任务和定时任务在运行过程中需要依赖数据库才能完成任务执行，使用`python manage.py migrate`执行。相关数据库表如下：

![img](http://qnpic.top/django_celery%5C2.jpg)

>下面就是Django路由的分发和视图函数的编写了，主要也是视图函数的引用了，代码如下：

```
# urls.py
from django.urls import path
from . import views


urlpatterns = [
    path('', views.index, name='index')
]

# views.py
from django.shortcuts import render,HttpResponse
from .tasks import test
import time

# Create your views here.


def index(request):
    # time.sleep(6)
    n = test.delay()
    return HttpResponse(n)

```

>至此我们就已经完成了Django 分布式功能的代码开发了，接下来就是如何使用分布式任务队列和定时任务。**注意**：你要启动django项目和Celery，两个哦~，都在该项目下

```
# 指令中django_celery就是项目名
celery -A django_celery worker -l info -P eventlet
```

![img](http://qnpic.top/django_celery%5CGIF1.gif)

>成功启动后就会自动加载django_celery中定义的分布任务，并显示相关的数据库信息，**注意**：记得启动redis服务，上面那个gif图下面忽然显示的就是分布式任务信息。

>当浏览器访问 http://localhost:8000 页面就会马上刷新，并不会等待六秒，会给celery的分布式任务后台队列执行,返回的就是该任务的的task_id，在相应的数据库表中会有记录


![img](http://qnpic.top/django_celery%5C3.jpg)

>以上就是分布式任务的好处了，该任务会在存储在消息队列中执行，相当于异步，在日常的开发中比如，数据库的操作可以用Celery的分布式任务，减少执行时间。

## 定时任务

>定时任务主要是通过Admin后台开发。在`Periodic tasks`中创建定时任务，设置每三秒执行一次，如下：

![img](http://qnpic.top/django_celery%5C4.jpg)

>**究极注意**：记得在该项目下运行：

```
celery -A django_celery beat -l info -S django
```

![img](http://qnpic.top/django_celery%5C5.jpg)

>运行运行以上代码后就能真的运行了，以上也能看出已在运行了,打开output.txt也是能看到三秒运行一次

![img](http://qnpic.top/django_celery%5C6.jpg)

<br><br><br>这次也是简单介绍了Celery框架的使用和配置，如分布式任务和定时任务，So, Just have fun...