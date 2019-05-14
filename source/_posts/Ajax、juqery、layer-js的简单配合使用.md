title: Ajax、juqery、layer.js的简单配合使用
author: Light Fish
tags:
  - 前端
categories:
  - 前端
date: 2019-03-24 23:31:00
---
# layer.js+Ajax+django完善登录注册

>这几日也是在搭建自己的一个django博客，原本的登录注册时上传数据，如果用户信息不对就重新刷新页面，这样的话原来的input标签里输入的数据也是会消失。所以我也会近几日看到了一个好的web弹层组件，感觉配合Ajax会很好，所以今天就简单写一下记录下，`layerjs`和`Ajax`的使用

<!-- more -->

# 注册功能的完善

>这次就用我以前做的一个django的登录注册项目来简单说一下。

* 1.urls.py 路由文件

```
from django.urls import path
from . import views

app_name = 'account' # 一定要写这行，否则html中会报 is not a  registered namespace 错误

urlpatterns = [
    path(r'login/', views.account_login,name='account_login'),
    path(r'logout/',views.account_logout,name='account_logout'),
    path(r'register/',views.account_register,name='account_register'),
]
```

* 2.views.py 视图函数文件（主要是注册的视图函数）

```python
from django.shortcuts import render,redirect
from django.http import HttpResponse
from django.views.decorators.csrf import csrf_exempt
from django.contrib.auth.models import User
from django.contrib.auth import authenticate,login,logout

@csrf_exempt
def account_register(request):
    username = request.session.get('username')
    title = ' LFBlog REGISTER '
    unit_2 = '/account/login/'
    unit_2_name = ' 立即登录 '
    unit_1 = '/account/setpassword'
    unit_1_name = ' 重置密码 '
    new_password = True
    password_tips = ' 重复密码 '
    register = True
    if request.method == 'POST':
        username = request.POST.get('username','')
        email = request.POST.get('email_input','')
        password = request.POST.get('password','')
        re_password = request.POST.get('new_password','')
        if User.objects.filter(username=username):
            return HttpResponse("1")
        elif not re.match(r'^[0-9a-zA-Z_]{0,19}@[0-9a-zA-Z]{1,13}\.[com,cn,net]{1,3}$',email):
            return HttpResponse("2")
        elif password != re_password:
            return HttpResponse("3")
        elif password == '' or len(password) < 6:
            return HttpResponse("4")
        else:
            user = User.objects.create_user(username=username, password=password, email=email)
            user.save()
            UserInfo.objects.create(user=user)
            login(request, user)
            request.session['username'] = username  # 验证是否登录成功，存储到session
            # return redirect('/blog/')
            return HttpResponse("5")
            # tips = ' 注册成功 '
    return render(request,'account/account_login.html',locals())

```

> **@csrf_exempt** ，是另一种操作POST时的加密机制，表单中还使用过 `csrf_token`,这里我们是需要引用的，否则会报403错误。或者去setting里面去掉csrf验证的中间件

* 3.HTML的模板代码如下

```
<div class="flex-center">
    <div class="container">
    <div class="flex-center">
    <div class="unit-1-2 unit-1-on-mobile">
        <h1>{{title}}</h1>
            <!--{% if tips %}-->
        <!--<div>{{ tips }}</div>-->
            <!--{% endif %}-->
        <form class="form" action="" method="post">
            {% csrf_token %}
            <div>用户名:<input type="text" id="username" ></div>
            {% if register %}
                <div>邮 箱:<input type="email" id="email" ></div>
            {% endif %}
            <div>密 码:<input type="password" id="password" ></div>
            {% if new_password %}
                <div>{{ password_tips }}<input type="password" id="new_password" ></div>
            {% endif %}
            <button type="button" class="btn btn-primary btn-block" onclick="post_submit()">确定</button>
        </form>
        <div class="flex-left top-gap text-small">
            <div class="unit-2-3">
                <a href="{{ unit_2 }}">{{ unit_2_name }}</a>
            </div>
            <div class="unit-1-3 flex-right">
                <a href="{{ unit_1 }}">{{ unit_1_name }}</a>
            </div>
        </div>
    </div>
    </div>
    </div>
</div>
```

>HTML代码基本如上，`if register`和`if new_password`是因为登录注册公用同一个模板。

* 4.javascript 代码如下

```javascript
<script type="text/javascript" src='{% static "js/jquery.js" %}'></script>
<script type="text/javascript" src="{% static 'js/layer.js'%}"></script>
<script type="text/javascript">
function post_submit(){
    if($('#email').length>0){ //判断是否有email的input标签，来知道是否是登录还是注册
        var username = $('#username').val();
        var email = $('#email').val();
        var password = $('#password').val();
        var new_password = $('#new_password').val();
        $.ajax({
            url: "{% url 'account:account_register' %}",
            type: "POST",
            data: {'username':username, 'email_input':email, 'password':password, 'new_password':new_password},
            success: function(e){
                if(e=="1"){
                    layer.msg(" 用户名已存在 ");
                }else if(e=="2"){
                    layer.msg(" 邮箱格式不正确 ")
                }else if(e=="3"){
                    layer.msg(" 前后密码不同 ")
                }else if(e=="4"){
                    layer.msg(" 密码不少于六位 ")
                }else{
                    location.href = "{% url 'blog:blog_list' %}";
                }
            },
        });
    }else{
        var username = $('#username').val();
        var password = $('#password').val();
        $.ajax({
            url: "{% url 'account:account_login' %}",
            type: "POST",
            data: {"username":username,"password":password},
            success: function(e){
                if(e=="2"){
                    layer.msg(" 账号错误，请重新输入 ");
                }else if(e=="3"){
                    layer.msg(" 用户名不存在，请注册 ")
                }else{
                    location.href = "{% url 'blog:blog_list' %}";
                }
            },
        });
    }
}
</script>
```



> 成功的截图

![img](http://qnpic.top/layer_django_ajax%5C1.jpg)

![img](http://qnpic.top/layer_django_ajax%5C2.jpg)

![img](http://qnpic.top/layer_django_ajax%5C3.jpg)

![img](http://qnpic.top/layer_django_ajax%5C4.jpg)

> 上面也是基本实现了注册登录的完善，当然layer.js也是有更好的样式，这里我就展示下我博客的栏目剪辑的部分

![img](http://qnpic.top/layer_django_ajax%5C5.jpg)

![img](http://qnpic.top/layer_django_ajax%5C6.jpg)

# 总结

> 这次的博客也是简单的展示了Ajax、jquery和layer.js的配合使用，以后可能也是会用到，简单记录一下，So

<br><br><br>Just have fun...