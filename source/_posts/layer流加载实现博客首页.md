title: layer流加载实现博客首页
author: Light Fish
date: 2019-04-09 22:08:44
tags:
---
# layer 流加载实现博客首页

>最近也是清明节，节后也是借着写我的博客。这次我也是写博客的时候，发现博客首页用流加载可能浏览效果更加好，所以我今天我就简单的描述一下。

<!-- more -->


# 后端代码实现

>1.Django后端的实现也是先去路由进行配置，然后指向相应的视图函数，由于篇幅有限我这里就不多阐述，就展示下视图函数，如何发送数据

```
from django.shortcuts import render,get_object_or_404,HttpResponse
from django.core.paginator import EmptyPage,Paginator,PageNotAnInteger #分页功能所需函数的导入
from .models import ArticlePost #这个就是博客文章的数据库models文件
import json


def article_page(request):
    article_titles = ArticlePost.objects.all()
    paginator = Paginator(article_titles, 8)
    page = request.GET.get('page')
    try:
        current_page = paginator.page(page)
        articles = current_page.object_list
    except PageNotAnInteger:
        current_page = paginator.page(1)
        articles = current_page.object_list
    except EmptyPage:
        current_page = paginator.page(paginator.num_pages)
        articles = current_page.object_list
    print(len(articles))
    articles_json = []
    for i in range(len(articles)):
        articles_json.append({'id':articles[i].id,'author':articles[i].author.username,'title':articles[i].title,'updated':articles[i].updated.strftime("%Y-%m-%d %H:%M:%S"),'body':articles[i].body[:70],'users_like':articles[i].users_like.count()})
    return HttpResponse(json.dumps({'static':200,'data':articles_json,'page_num':paginator.num_pages}))
```

>2.如上方法就想数据库取数据然后再传输数据，传输的数据然后前端再接收流加载出来

# layer框架

>3.首先当然是引入该框架所需的js文件和css文件,引入如下文件

```
<link rel="stylesheet" type="text/css" href="layer/css/layui.css">
<script type="text/javascript" src="layer/jquery-3.1.1.min.js"</script>
<script type="text/javascript" src="layer/layer.js"></script>
<script type="text/javascript" src="layer/layui.js"></script>
```

>4.这里是官方的文档以及一些我自己写的简单html文件，方便理解——[下载](https://www.lanzous.com/i3q09wf)

# 前端代码实现

>5.上述也是简单引入了所需文件，下面也是说明下用法,

```
<ul id="flow_demo" ></ul>

<script type="text/javascript">
	layui.use(['layer','flow'],function(){
		var layer = layui.layer;
		var flow = layui.flow;

		flow.load({
			elem: "#flow_demo",
			is_Auto:true,
			end: "emm,nothing next",
			is_Lazyimg: true,
			done: function(page,next){
				var lis = '';
				$.get('/article/article_page/?page='+page, function(e){
				    parse_res = JSON.parse(e)
                    	res = parse_res.data
                    	page_num = parse_res.page_num
                    	console.log(res)
				    for(var i = 0; i < res.length; i++){

                        var item = '<div class="list-group">'+
                            '<a href="/article/article_content/'+res[i].id+'" class="list-group-item active">'+
                                '<h3 class="list-group-item-heading">'+res[i].title+'</h3>'+
                            '</a>'+
                            '<p class="list-group-item-text">作者:'+res[i].author+'</p>'+
                            '<p class="list-group-item-text">概要:'+res[i].body+'</p>'+
                            '<p class="meta"><span><i class="layui-icon layui-icon-praise"></i> '+res[i].users_like+'</span>'+
                            '<span class="update_time">'+res[i].updated+'</span>'+
                            '</p>'+
                        '</div>';

                        lis+=item;

				    }
				    next(lis,page<page_num)
				})


			}
		})
	})
</script>
```

>6.关于详细的文档我这里推荐[官方文档](https://www.layui.com/doc/modules/flow.html),简单来说也是done后添加get请求获取数据，搭建html然后next函数添加到网页后续

>7.实现动态图如下

![img](http://qnpic.top/layer_flow%5CGIF.gif)

<br><br><br>这次的layer的流加载也是简单的介绍一下，So<br><br>Just have fun...