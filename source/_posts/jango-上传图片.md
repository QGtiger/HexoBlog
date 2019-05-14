title: Django 上传图片
author: Lightfish
tags:
  - django
  - python
categories:
  - python
date: 2018-12-24 13:14:00
---
# Django后台获取前端post上传的文件方法

>今天无聊就来写写django后台上传文件的方法

<!-- more -->


## 准备工作

>`django-admin startproject uploadimg` 新建django项目

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwwc0kmuj30b601gjre.jpg)

>`cd uploadimg` 进入自己创建的项目，然后`django-admin startapp index` 创建app，名字index

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwwc2iavj30cq01aq2y.jpg)

>在项目根目录下创建一个静态文件夹 `pubstatic`,等会上传的文件就保存到这里

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwwc5qifj309p07ndgh.jpg)

>修改`settings.py`文件，因为这次，只涉及上传图片到django后台，就只设置这几项

* 1.第一步当然是注册app咯~

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwwc8g9ij30cq080wez.jpg)

* 2.设置模板路径

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwwcaqeij30ms0bsgmz.jpg)

* 3.设置静态文件路径，`STATICFILES_DIRS`，这个就是为了查找静态文件，上面那个`STATIC_URL = '/static/'`这个本来就存在，它会自动到上面注册好的app下进行查看`static`静态文件夹

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxg8jzdj30l303zt92.jpg)

>到这，对于这次的博客配置基本结束

## URL改写

>在`uploadimg`文件夹下的`urls.py`进行配置，`path('',include('index.urls'))`,URL为空，将URL分发给index的`urls.py`处理

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxgcqccj30h306imxo.jpg)

>所以，我们要在`index`下创建`urls.py`文件，因为本来是不存在的，然后进行简单的代码

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxglh27j30gy06yt95.jpg)

>第一个`upload.html`就是简单的展示界面，展示一个form表单，`views.py`文件中`index`函数

```
from django.shortcuts import render
def index(request):
    return render(request,'form.html') 
```

>form.html html文件代码基本如下，注意在`form`表单的定义中一定要加上`enctype＝ “multipart/form-data"`属性，否则后台可能会取不到文件

```
{% load static %}  #这个为了能够调用静态文件
<form method = "post" action ="uploadimg" enctype="multipart/form-data">  
	{% csrf_token %}  # 防止CSRF网站攻击 
	<div class=selectric">
		<input id="selectinput" name='images' type="file" class="file_cselectpic"/>
</div>
<p class="选择图片_text">只支持jpg、png和gif,且大小不超过2M</p>
<input type="submit" id="photo_confirm"/>
</form>
```

>上面那个form表单`action`就是跳转到uploadimg，`index`下的`urls.py`就接受这个url,调用`ChangImg`函数

```
from django.shortcuts import render
from django.http import HttpResponse
from django.core.files.storage import default_storage
from django.core.files.base import ContentFile
import os
from django.conf import settings
def ChangeImg(request):
    try:
        image = request.FILES.get('images')
        if image.size > 10000 and image.size < 20480000:
            save_path = settings.STATICFILES_DIRS[0]+'/upload/img/'+image.name
            if os.path.exists(save_path):
                os.remove(save_path)
            path = default_storage.save(save_path,ContentFile(image.read()))
            print(path)
            return HttpResponse('Successful...')
        else:
            return HttpResponse('Error...')
    except Exception as e:
        print(e)
        return HttpResponse('Error...')
```

>当我们获取到图片img后，可以通过

* image＝request.FILES.get('images')去获取到该图片

* image.name 获取到图片的名字

* image.size获取到图片的大小

* image.read()可以获取图片内容

>通过`default_storage.save(save_path,ContentFile(image.read()))`把图片缓存到指定文件，save_path就是前面构造的存储路径，如果不存在路径会自动创建

>`from django.conf import settings` import的`settings`可以访问settings.py文件中的属性，`STATICFILES_DIRS[0]`，就是前面设置的静态文件路径 

## 成功截图

>点击提交后，跳转到`uploadimg`url,接受后，调用`ChangeImg`函数，`request.FILES.get('images')`获取图片，这个`'images'`就是前面选图片的`input`标签的name属性值。

>获取图片后 ，调用`default_storage.save(save_path,ContentFile(image.read()))`保存图片

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxgnynkj31as0nfq42.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxgskidj31b10potf0.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxh384xj315u0k9wfn.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxh8ko1j31580hd74v.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwxhbmu2j30xd0indif.jpg)


<br><br><br>这次的博客就到这里了，So<br><br>Just have fun...