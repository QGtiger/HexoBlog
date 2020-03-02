title: Django 后端图片优化处理
author: Light Fish
tags:
  - Django
categories:
  - Django
date: 2019-05-25 22:30:00
---
# Django 后端图片优化处理

> 在我的LightBlog博客上的头像上传前端是借用cropper——一款优秀的jquery插件，在之前的博客也是介绍过，可以去了解一下[《cropper实现头像裁剪上传》](/2019/03/28/cropper实现头像裁剪上传/).回归正题，我前端通过cropper上传裁剪好的图片到后端接收和保存，但是后端接收道德往往是较大的图片，如果不经过一定的压缩处理就会造成页面刷新速度慢和流量浪费等情况，所以我们可以在上传图片的时候生成该图片的缩略图。一种方法可以是借助pillow这个图片处理库在接收后的视图函数中进行压缩和保存，这种简单易并且容易理解，所以这里就不介绍了。这里介绍的是第二种方法——django-imagekit第三方库

<!-- more -->

> 关于这个库的详细说明可以看[官方文档](https://django-imagekit.readthedocs.io/en/latest/),这里就简单介绍下基本的用法和应用场景

## 安装依赖库

> 1.要在Django中使用ImageField模块，需要安装pillow模块

```
pip install pillow
```

> 2.然后安装django-imagekit

```
pip install django-imagekit
```

> 3.完成上述步骤后就可以在django项目中的settings文件中进行配置

```
INSTALLED_APPS = [
	# ...
    'imagekit', # django-imagekit
]
```

> 4.现在准备工作全部完成，可以在项目中使用 django-imagekit 来处理图片了。

## 应用场景

> 一个基本的应用场景就是在我用户上床头像的时候就生成缩略图，所以就在modles中进行配置：

```
from django.db import models
from imagekit.models import ImageSpecField
from imagekit.processors import ResizeToFill


# Create your models here.
class UserInfo(models.Model):
    photo = models.ImageField(' 头像 ',upload_to='avator', default='default/default.jpg')

    # 注意：ImageSpecField不会生成数据库中的表
    # 处理后的图片
    photo_150x150 = ImageSpecField(
        source="photo",
        processors=[ResizeToFill(30, 30)], # 处理后的图像大小
        format='JPEG',  # 处理后的图片格式
        options={'quality': 95} # 处理后的图片质量
        )
```

> 如上就设置好了，是不是很简单^-^.这个时候你就可以在前端页面使用模板标签,下面这个是获取缩略图的路径,150x150 其实就是命名，我这里其实是30x30的size

```
<img id="avator" src="{{ request.user.userinfo.photo_150x150.url }}" class="layui-nav-img avator">
```

![img](http://qnpic.top/2019-05-25%5C1.jpg)

> 以上就是生成的地址链接，你也可以用下面的访问未压缩的图片

```
<img src="{{ userinfo.photo.url }}" class="layui-circle">
```

> 当你看链接一点也是有点蒙，所以找目录的时候就看到了下面

![img](http://qnpic.top/2019-05-25%5C2.jpg)

> `django-imagekit` 只是在media的目录下生成了 `CACHE`目录并指向该缩略图。该图片路径使用 `imagekit.cachefiles.namers.source_name_as_path` 来生成图片的路径。大体逻辑是，根据你设置的参数：processors,format,options进行pickle，把数据写到一个内存文件中，然后根据文件内容做一个md5，作为生成这个尺寸图片的名称。这么说比较抽象，还是举个例子：原图:/image/the5fire.png 生成图 /CACHE/images/the5fire/123dadwdniijonsd.jpg，这不是实际的生成结果，只是示例，最后的 `123xxxsd` 可以当做那个md5的值，当然前面的 `/CACHE/images/` 这样的路径也可以定制。以上就是为什么在该目录下生成，其实你是不用管的，下面就就来讲一下我再实践中遇到的问题和解决方法

## 问题描述

> 我LightBlog的头像命名是以用户名加'.jpg',这样就很简单的解决了头像的冲突，所以我在头像上传和保存那里有了一点卡顿

* 1.首先我原本是通过以下方法来进行保存 ， 但是这样保存并不会使用imageField的save方法，所以也就没有使用到django-imagekit。所以我换了方法

```
		uploadimg = request.FILES.get('uploadimg', '')
        userinfo = UserInfo.objects.get(user=request.user)
        fname = '{}/{}'.format(settings.MEDIA_ROOT, str(userinfo.photo))
        try:
            with open(fname, 'wb') as f:
                for c in uploadimg.chunks():
                    f.write(c)
            return HttpResponse(json.dumps({'status':200,'tips':'上传成功','photo':userinfo.photo.url}))
        except:
            return HttpResponse(json.dumps({'status':500,'tips':'上传失败'}))
```

* 2.使用了ImageField的save方法

```
		uploadimg = request.FILES.get('uploadimg', '')
        userinfo = UserInfo.objects.get(user=request.user)
        avatorInfo = str(userinfo.photo).split('/')
        if avatorInfo[0] == 'default':
            pass
        else:
            if avatorInfo[1] == username+'.jpg':
                pass
            else:
                fname = '{}/{}'.format(settings.MEDIA_ROOT,str(userinfo.photo))
                os.remove(fname)
        try:
            userinfo.photo.save(username + '.jpg', uploadimg)
            return HttpResponse(json.dumps({'status':200,'tips':'上传成功','photo':userinfo.photo.url}))
        except:
            return HttpResponse(json.dumps({'status':500,'tips':'上传失败'}))
```

> 但是上面的这种方法确实实现缩略图，但是当你第二次上了的时候并不会覆盖前一个图片而是创建了另一个,如果一直一样下去，肯定会浪费服务器储存空间，所以我又想了一计

![img](http://qnpic.top/2019-05-25%5C3.jpg)

* 第二次上传的时候，先删除前一个的头像图片在保存我上传的图片，(嘿嘿，美的很)。然而django-imagekit反手就是给我一个'巴掌',并不行,下面gif可以看到，第二次上传并没有生成缩略图。

![img](http://qnpic.top/2019-05-25%5CGIF.gif)

* 经过上面的两次错误，我也是大概了解到了django-imagekit这个模块的工作流程应该是到正常触发ImageField 的save函数(即数据库中的值发生变化)才会生成缩略图，因为第二次的时候其实数据库中的值并没有发生变化都是一样的。所以应该是数据库中的该值发生变化才会触发。所以经过我一定时间的考虑，我写出了下面代码

```
@login_required(login_url='/account/login/')
@csrf_exempt
def my_image(request):
    username = request.user.username
    if request.method == 'POST':
        uploadimg = request.FILES.get('uploadimg', '')
        userinfo = UserInfo.objects.get(user=request.user)
        avatorInfo = str(userinfo.photo).split('/')
        if avatorInfo[0] == 'default':
            pass
        else:
            if avatorInfo[1] == username+'.jpg':
                pass
            else:
                fname = '{}/{}'.format(settings.MEDIA_ROOT,str(userinfo.photo))
                os.remove(fname)
        try:
            userinfo.photo.save(username + '.jpg', uploadimg)
            return HttpResponse(json.dumps({'status':200,'tips':'上传成功','photo':userinfo.photo.url}))
        except:
            return HttpResponse(json.dumps({'status':500,'tips':'上传失败'}))
    else:
        return HttpResponse("该页面摸得GET")
```

> 该方法下，一个用户，会有两张图片，一张是第一次上传的，一张就是后面上传的，这样的话就一定层面上减少了服务器储存空间的浪费。(emmm,主要我现在可能对该方法底层的一些触发并不是很了解，sry~~)

## 效果图

![img](http://qnpic.top/2019-05-25%5CGIF2.gif)



## 拓展

> 有点憨了，没有好好浏览官方文档(也不能太怪我，咱英语差也不敢问,也不敢说).这次我也是去试了下，发现可以完全替代django自带models里的ImageField.使用代码也是很简单基本和我介绍的一样，只是自己憨,代码如下，只需要修改models文件就行了(当然记得`makemigrations`和`migrate`)

```
class UserInfo(models.Model):
    #...
    photo = ProcessedImageField(
        upload_to='avator',
        processors=[ResizeToFill(400, 400)],
        format='JPEG',
        options={'quality':98}, verbose_name='展示图片')
```

> 以上这样就实现了上传图片后的裁剪，这样的好处这直接，我用ImageField 保存的图片大小至少几百kb而如上配置完了不到50kb。gif效果图如下

![img](http://qnpic.top/django-imagekit%5CGIF.gif)

<br><br><br>这次也是简单介绍了`Django-imagekit`的用法，So<br><br>Just have fun...