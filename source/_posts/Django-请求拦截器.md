title: Django 请求拦截器
author: Light Fish
tags:
  - Django
categories:
  - Django
date: 2019-05-27 17:45:00
---
# Django 请求拦截器

> 前两天再弄前后端分离的token验证部分，也是很轻松的实现了，这里也是要好好感谢下自己，谢谢。回归正题，实现了header里的token验证，所以我想在django后端的每个请求的视图函数都验证一下，本来我是想写个装饰器，这样也是尽可能的实现了代码的重用性，但是这样如果视图函数多的话也是有点无聊的。就想起我室友学java spring开发的时候用的一个请求拦截器，它能拦截每个请求并做统一的验证，这个拦截器才是我真正想要的。所以就去问了一下度娘。度娘反手就是一个网址给我瞅了两眼，就再结合一下vue前段项目就有了这片博客。

<!-- more -->

## Django 请求拦截器的实现

> 1.现在你的django 项目的注册application 下新建一个py文件，名字随意，这里就演示为loginmiddleware.py

![img](http://qnpic.top\/home/lightfish/Desktop/blog/blog_img/2019-05-27/1.png)

> 2.loginmiddleware.py 文件的 代码如下,401状态码是 token过期，402是没有token验证

```python
from django.shortcuts import HttpResponse
from .token_module import out_token, get_token
import json


try:
    from django.utils.deprecation import MiddlewareMixin  # Django 1.10.x
except ImportError:
    MiddlewareMixin = object


class SimpleMiddleware(MiddlewareMixin):
    def process_request(self, request):
        if request.path != '/backend/login' and request.path != '/backend/register':
            try:
                token = request.META.get('HTTP_AUTHORIZATION')
                token_list = token.split('&')
            except:
                return HttpResponse(json.dumps({'tips': '您未登录', 'status':402}))
            try:
                if out_token(token_list[1], token_list[0]):
                    pass
                else:
                    return HttpResponse(json.dumps({'tips': '您未登录,登录信息过期', 'status': 401}))
            except Exception as e:
                print(e)
```

> 这里的`out_token` 和 `get_token`就是token加解密的过程，这更我前两天的博客相关联——[前后端分离之token验证](/2019/05/24/前后端分离/#前后端分离开发登录验证——header验证),可以去看看。出去这部分其他的逻辑就很简单了，不多介绍了

> 3.写完了loginmiddleware.py 这个中间件，你想在django 项目中启用它就得在settings配置文件中配置一下

```
MIDDLEWARE = [
    #...
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'backend.loginmiddleware.SimpleMiddleware' # here 请求拦截器
]
```

* 就是上面这样简单,下面就来简单介绍在Django-Vue前后端项目中如何实现

## Django-Vue 前后端分离实现token验证

> 至于Vue前端的登录注册我就不在介绍了，就写个button发送请求来测试是否在登录状态

* 1.button 前端代码，这里的handleTest 代码也就很简单，向后端访问数据，并打印

```
<Button type="primary" @click="handleTest" key="test">测试index</Button>

// handleTest 代码
    handleTest () {
      request({
        method:'get',
        url: '/backend/index'
      }).then(res=>{
        let response = res.data;
        console.log(response)
        this.$Modal.info({
          title: '/backend/index 路由的数据',
          content: response.tips
        })
      })
    }
```

* 2.Django后端这个返回的数据也很简单,就是这样简单

```
def indexView(request):
    if request.method == 'GET':
        return HttpResponse(json.dumps({'tips':'这是一个简单的页面信息'}))
```

* 3.感觉这样很简单，就修改下axios 封装的配置文件中的响应拦截器，401状态码是 token过期，402是没有token验证

```
service.interceptors.response.use(
  response => {
    endLoading()
    if(response.data.status == 401){
      Message.error('token值无效，请重新登录')
      // 清除token
      localStorage.removeItem('token')
    }
    if(response.data.status == 402){
      Message.error('请先重新登录')
      router.push('/login')
    }
    return response
  },
  //...
)
```

> 这里我在说一下前后端的工作流程，token是登录才被前端存放在localStorge
>
> * 1.前端想后端访问，带上存放在localStorge中的token放在header中，如果没有就不放，但是后端的请求拦截起就会拦截这个请求并验证是否有token和token是否过期，如果验证成功就去视图函数处理，否则就直接返回状态数据。而前端也有请求响应拦截器，前端接受这个返回数据，向看看status(401状态码是 token过期，402是没有token验证),并前端相对应进行界面展示

* 最后效果图如下：

![img](http://qnpic.top\/home/lightfish/Desktop/blog/blog_img/2019-05-27/GIF.gif)

> 因为我token设置的过去时间是是20秒,我设置在settings里。

```
# token 过期时间
TOKEN_EXPIRE_TIME = 20

# 视图函数
from django.conf import settings
settings.TOKEN_EXPIRE_TIME # 获取settings配置里的token过期时间
```



<br><br>这次的博客也是减少了Django的请求拦截器和前端axios拦截器的配置,So,

<br>Just have fun...