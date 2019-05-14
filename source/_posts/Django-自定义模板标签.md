title: Django 自定义模板标签
author: Light Fish
date: 2019-04-22 17:42:07
tags:
---
# 自定义模板标签

>在Django中模板中的for和if就是默认的模板标签，为了更为复杂的操作，Django还允许开发自定义的模板标签，共有三种自定义模板标签，如simple_tag、inclusion_tag和assignment_tag。

<!-- more -->

>下面我会用我再LightBlog的实例来简单讲解一下

## 准备

>首先你需要在你的项目setting.py文件中注册的app目录下，创建<font  style="color:red"><b>templatetags</b></font>文件夹，这个目录名称是Django默认的，必须使用这个名称。然后在该目录下创建`__init__.py`和你要写模板标签的py文件，比如我这里就是`article_tags.py`.

![img](http://qnpic.top/django_tags%5C1.jpg)

## 自定义模板标签显示文章总数

>编辑模板标签文件`article_tags.py`

```
from django import template

register = template.Library()
from article.models import ArticlePost

@register.simple_tag
def total_articles():
    return ArticlePost.objects.count()
```

>然后现在你要使用该模板标签的html文件中先引入

```
{% load article_tags %}
```

>然后就可以使用啦

```
{% extends "base.html" %}
{% load article_tags %}
{% block title %} 博客首页 {% endblock %}
{% block content %}

<h1>丰富头脑，善化行为</h1>
    <p>There are {% total_articles %}  articles which you can read.</p>
```

![img](http://qnpic.top/django_tags%5C2.jpg)

## 使用自定义模板显示用户的文章总数

>首先依旧先在`article_tags.py`文件中编写函数,这里是参数的使用

```
from django import template

register = template.Library()
from article.models import ArticlePost

@register.simple_tag
def author_total_articles(user):
    return user.article_post.count()
```

>然后就可以在想使用该模板标签的html文件中引入在使用

```
{% extends "base.html" %}
{% load article_tags %}
    <h3 style="text-align:center">共发表{% author_total_articles user %}篇文章</h3>
```

>先 `load article_tags` 引入后就可以使用，记得加参数，这里的user其实就是后端传过来的数据库对象

![img](http://qnpic.top/django_tags%5C3.jpg)

## 自定义模板显示最新文章

>这里的话就介绍`inclusion_tag`的使用,依旧先在`article_tags.py`文件中加入以下代码

```
from django import template

register = template.Library()
from article.models import ArticlePost

@register.inclusion_tag('article/list/latest_articles.html')
def latest_articles(n=5):
    latest_articles = ArticlePost.objects.order_by('-created')[:n]
    return {"latest_articles": latest_articles}
```

>`article/list/latest_articles.html` 的html代码如下

```
<ul>
    {% for article in latest_articles %}
    <li>
        <a href="{% url 'article:article_content' article.id %}">{{ forloop.counter }}.{{ article.title }}</a>
    </li>
    {% empty %}
    <p>nothing..</p>
    {% endfor %}
</ul>
```

>这里的话相当于使用`article/list/latest_articles.html`这个template，然后传入数据渲染html页面，然后这个渲染后的页面，最终被主html页面引用，引用方法如下

```
<h3 style="text-align:center">最新文章</h3>
            {% latest_articles 4 %}
```

>`latest_article 4`就完成了引用，返回的也是html页面，最终显示页面

![img](http://qnpic.top/django_tags%5C4.jpg)

## 使用自定义模板标签显示评论最多文章

>这里的话也是一样先在`article_tags.py`文件中定义标签，当然这里主要也是为了介绍django 数据聚合函数 `ANNOTATE`,这里的话，我推荐这篇[博客](https://www.cnblogs.com/wumingxiaoyao/p/7065471.html)先了解一下

```
from django import template
from django.db.models import Count 

register = template.Library()
from article.models import ArticlePost

@register.simple_tag
def most_commented_articles(n=5):
    return ArticlePost.objects.annotate(total_comments=Count('comments')).order_by('-total_comments')[:n]
```

>这里的`total_comments=Count('comments')`里的comments就是通过Foreign关联的，如models.py文件的关联代码如下

```
from django.db import models
class Comment(models.Model):
    article = models.ForeignKey(ArticlePost, on_delete=models.CASCADE, related_name="comments") 
```

>就是这里的related_name="comments"来进行关联，关联后赋值给total_comments给后面进行筛选和取值，返回的是数据库对象，然后在html模板文件中使用

```
{% load article_tag %}

{% most_commented_articles as articles %}
            <ul>
                {% for article in articles %}
                <li>
                    <a href="{% url 'article:article_content' article.id %}">{{ forloop.counter }}.{{ article.title }}</a>
                </li>
                {% endfor %}
            </ul>
```

>最后渲染界面如下

![img](http://qnpic.top/django_tags%5C5.jpg)

<br><br><br>这次的博客也是简单介绍了simpla_tag和inclusion_tag的用法，记得先`register = template.Library()`哦，So，<br><br>Just have fun...

