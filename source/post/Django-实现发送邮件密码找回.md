title: Django 实现发送邮件密码找回
author: Light Fish
tags:
  - Python
  - Django
categories:
  - Python
  - Django
date: 2019-01-29 14:16:00
---
# Django 发送邮件实现密码找回

>这次的博客使用的是Django内置的邮件功能实现邮件的验证。So，show you now。

## 前言

>在实现邮件发送功能之前，我们需要对邮件进行相关配置，以QQ邮箱为例，在QQ邮箱的设置中找到账户设置，在账户设置中找到`POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV`服务，然后开启`POP3/SMTP`服务。值得注意的是，开启服务的时候会返回一个客户的授权密码，改密码是用于登陆第三方邮件客户端的专用密码，切记保存密码，该密码在开发过程中需要使用。

<!-- more -->

## 使用步骤

>1.在Django的`setting.py`中添加QQ邮箱的相关配置，配置信息如下：

```
# 邮件配置信息
EMAIL_USE_SSL = True

# 邮件服务器， 如果是163邮箱就改成smtp.163.com
EMAIL_HOST = ‘smtp.qq.com’

# 邮件服务的端口
EMAIL_PORT = 465

# 发送邮件的账号，也就是前面你开启服务的邮箱号
EMAIL_HOST_USER = ‘1426286337@qq.com’

# SMTP服务密码
EMAIL_HOST_PASSWORD = ‘xxxxxx’
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
```

>2.上述配置是邮件发送方的邮件服务器信息，各个配置信息说明如下

* 1.EMAIL_USER_SSL：设置Django与邮件服务器的连接方式为SSL。

* 2.EMAIL_HOST：设置服务器的地址，该配置使用SMTP服务器

* 3.EMAIL_PORT：设置服务器端口的信息，若使用的是SMTP服务器，则端口应为`465`或`587`

* 4.EMAIL_HOST_USER： 发送邮件的账号，该账号必须开启了`POP3/SMTP`服务

* 5.EMAIL_HOST_PASSWORD：客户端授权密码

* 6.DEFAULT_FROM_EMAIL：设置默认发送邮件的账号

## 简单验证

>这里就简单的来验证一下

```
# urls.py代码
from django.urls import path
from . Import views

urlpatterns = [
    path(‘findpassword.html’, views.findPassword, name=‘findPassword’),
]

# 模板html文件中的form表单
        <h1>MyDjango Auth</h1>
            {% if tips %}
        <div>{{ tips }}</div>
            {% endif %}
        <form class="form" action="{% url 'findPassword' %}" method="post">
            {% csrf_token %}
            <div>用户名:<input type="text" name='username' value="{{ username }}"></div>
            <div>验证码:<input type="text" name='VerificationCode'></div>
            {% if new_password %}
                <div>新密码:<input type="password" name='password'></div>
                <div>重复密码:<input type="password" name='re_password'></div>
            {% endif %}
            <button type="submit" class="btn btn-primary btn-block">{{ button }}</button>
        </form>
        <div class="flex-left top-gap text-small">
            <div class="unit-2-3">
                <a href="{{ unit_2 }}">{{ unit_2_name }}</a>
            </div>
            <div class="unit-1-3 flex-right">
                <a href="{{ unit_1 }}">{{ unit_1_name }}</a>
            </div>
        </div>

# view.py的视图函数
from django.shortcuts import render,redirect
from django.http import HttpResponse
from django.contrib.auth.models import User
from django.contrib.auth import login,logout,authenticate
import random
from django.contrib.auth.hashers import make_password
import re

def findPassword(request):
    button = ' 获取验证码 '
    new_password = False
    title = ' 修改密码 '
    unit_2 = '/user/login.html'
    unit_2_name = ' 立即登录 '
    unit_1 = '/user/register.html'
    unit_1_name = ' 立即注册 '
    if request.method == 'POST':
        username = request.POST.get('username','')
        VerificationCode = request.POST.get('VerificationCode','')
        password = request.POST.get('password','')
        re_password = request.POST.get('re_password','')
        user = User.objects.filter(username=username)
        if not user:
            tips = ' 用户'+username+' 不存在'
        else:
            if not request.session.get('VerificationCode',''):
                button = ' 重置密码 '
                tips = ' 验证码发送 '
                new_password = True
                VerificationCode = str(random.randint(1000,9999))
                request.session['VerificationCode'] = VerificationCode
                user[0].email_user(' 找回密码 ', VerificationCode)
            elif VerificationCode == request.session.get('VerificationCode',''):
                if password == re_password:
                    dj_ps = make_password(password, None, 'pbkdf2_sha256')
                    user[0].password = dj_ps
                    user[0].save()
                    del request.session['VerificationCode']
                    request.session['user'] = username
                    return redirect('/')
                else:
                    tips = ' 前后密码不同 '
            else:
                tips = ' 验证码错误，请重新获取 '
                new_password = False
                del request.session['VerificationCode']
    return render(request,'find.html',locals())
```

>用户第一次访问的时候，触发`GET`请求，视图函数直接将模板返回，只有账号和验证码要求输入。当用户输入用户名，并且输入正确，单击“获取验证码”按钮，触发`POST`请求，视图函数findPassword 首先根据输入的用户名和模型中的User里的数据进行查找，若不存在生成提示信息。

![img](http://qnpic.top\django_send_message%5C1.jpg)

>如果存在，判断会话`session`的`VerificationCode`是否存在。若不存在则发送邮件，验证码是`random`函数随机生成的，并将验证码写入会话session的VericationCode，其作用是与用户输入的验证码进行匹配。

>邮件的发送使用内置的函数 `email_user` 实现的，该方法是模型 `User` 特有的方法之一，只适用于模型User，需要注意的是，用户的邮箱来自于模型User的字段email，邮箱信息为空的话是无法发送成功的。

>用户收到验证码以后，输入验证码然后单击，触发 `POST` 请求，视图函数就会获取用户输入的验证码和`session`中的 `VericationCode`进行对比，不符合系统就会提示，如下图

>如果相符，就会执行密码修改，然后使用 `make_password` 对密码加密处理并保存在模型User 中，最后记得要删除session中的 `VericationCode`，否则会画会一直存在，在下次获取验证码的时候就会出错。


## 拓展

>除了使用内置 `email_user` 实现邮件发送之外，Django还提供多种发送方式，我在Django的shell模式中进行讲解，代码如下：

```
python manage.py shell # 启动shell模式

from django.core.mail import send_mail
from django.conf import settings. # 获取setting.py的配置信息

from_email = settings.DEFAULT_FROM_EMAIL
# 发送邮件，接受邮件以列表表示，说明可设置多个接收对象
send_mail(‘MyDjango’, ‘This is a email from Django’, from_email, [‘1223@qq.com’])

# 使用 send_mass_mail 实现多封邮件同时发送
from django.core.mail import send_mass_mail
Message1 = (‘MyDjango’, ‘This is Django’, from_email, [‘123@qq.com’])
Message2 = (‘MyDjango’, ‘This is Django’, from_email, [‘234@qq.com’])
send_mass_mail((Message1, Message2), fail_silently=Flase)

# 使用EmailMultiAlternatives 实现邮件发送
from django.core.mail import EmailMultiAlternatives
content = ‘<p>这是一封<strong>重要的<strong>邮件 </p>’
msg = EmailMultiAlternatives(‘MyDjango’, content, from_email, [‘123@qq.com’])
# 设置正文为HTML格式
msg.content_subtype = ‘html’
# attach_alternative 对正文进行补充和添加
msg.attach_alternative(‘<strong>This is from Django</strong>’, ‘text/html’)
# 添加附件（可选）
msg.attach_file(‘E://attach files.csv’)
# 发送
msg.send()
```

>上述例子中，方法说明如下

* 1.使用 send_mail 每次发送都会建立一次连接，发送多封就会多次建立连接

* 2.使用send_mass_mail 是建立单个连接发送多个邮件，优于send_mail

* 3.使用 EmailMultiAlternatives 比前两个更为人性化，正文内容可以为HTML格式，也可添加附件，满足多方面的开发需求