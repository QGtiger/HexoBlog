title: Django 上线部署准备
author: Light Fish
tags:
  - Django
categories:
  - Django
date: 2019-05-07 17:24:00
---
# Django 静态文件处理

>一般Django项目上线部署的时候都是要设置settings文件中的Debug改成false和`ALLOWED_HOSTS = ['*']`,这里的DEBUG设置成False后，Django就不会自动搜索静态文件。这里的话，你首先得了解STATIC_ROOT是什么，你可以看这篇文章——[STATIC_ROOT、STATICFILES_DIRS和MEDIA_ROOT的区别](https://stackoverflow.com/questions/24022558/differences-between-staticfiles-dir-static-root-and-media-root),或者看[settings官方文档](https://docs.djangoproject.com/en/2.2/ref/settings/)

<!-- more -->

>* 1.`MEDIA_ROOT` 是存放上传的文件，如我的LightBlog就是存放上传的头像

>* 2.`STATIC_ROOT` 用来存储使用`python manage.py collectstatic`命令收集起来的静态文件

>* 3.`STATICFILES_DIRS` 是Django将搜索除static安装的每个应用程序的文件夹之外的其他静态文件的文件夹列表。

>在上线部署的时候可以使用Django来处理静态文件，也可以使用nginx处理静态文件，这里给出Django处理静态文件的办法。

## settings配置模板文件路径

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
                    os.path.join(BASE_DIR,'templates'),
                 ],
        'APP_DIRS': False,  # 自定义模板位置，不在允许按照默认方式寻找模板文件
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

## settings 配置STATIC_ROOT等

```
STATIC_URL = '/static/'
# 项目根目录下的static文件夹
STATIC_ROOT = os.path.join(BASE_DIR,'static')

STATICFILES_DIRS = [
    os.path.join(BASE_DIR,'media'),
    #os.path.join(BASE_DIR,'static') #这个debug配置成false后就不能会上面的STATIC_ROOT相冲
]

MEDIA_ROOT = os.path.join(BASE_DIR,'media')
```

## 在urls 添加

```
from django.views import static
from django.conf import settings
from django.conf.urls import url

urlpatterns = [
    url(r'static/(?P<path>.*)$', static.serve, {'document_root': settings.STATIC_ROOT}, name='static'),
    url(r'^media/(?P<path>.*)$', static.serve, {'document_root': settings.MEDIA_ROOT}, name='media')
]
```

# 异常处理机制

>网站的异常是一个普遍存在的机制，常见的异常以404或500为主，异常的出现主要是网站自身的数据缺陷或者人为不合理的访问所导致。这里就简单介绍下自定义404界面

>由于网站的404异常时作用于整个网站，因此在项目下的同名文件夹下的urls.py中进行设置404信息，代码如下

```
# 项目LightBlog下的urls.py
from article import views
handler404 = views.page_not_found
handler500 = views.page_not_found

# 或者
"""
handler404 = "article.views.page_not_found"

即：handler404 = “应用名称.views.函数名称” 
"""
```

>可以看到网站的404和500异常信息都由 article 下的views.py下的page_not_found视图函数进行处理。article是注册的app应用。page_not_found视图函数代码如下：

```
# 404,505页面
def page_not_found(request, exception):
    return render(request, '404/error404.html', status=404)

```

>上面面的参数exception是我在上ubuntu服务器上运行的时候所报错误加上去的，本地没有也是可以运行的，不加服务器上报以下错误,注意一下就行

![img](http://qnpic.top/django_404%5C1.jpg)

>以上的视图函数也是上面简单，就是返回一个页面，这里我就不多加阐述了，可以自己编写或者网上找个好看的，下面是我的404错误页面,喜欢的话，点点[这里](http://lightfish.cf/index.php?share/file&user=1&sid=hbJSRXVf)看看

![img](http://qnpic.top/django_404%5C404.gif)

<br><br><br>以上就是django项目上线部署的准备，So, Just have fun...