title: Hexo+github搭建个人博客三
author: Lightfish
toc: true
tags:
  - Hexo搭建
categories:
  - Hexo
date: 2018-12-12 15:16:00
---
# Hexo+github搭建个人博客

>前两个博客已经大概讲述了，hexo博客的搭建和基本的设置。今天，我就来教你如何写hexo搭建的博客。

<!-- more -->

## 1.原生方式新建文章

>Hexo的项目结构是在网站根目录的source/_posts目录下存放你的博客文档，以.md文档格式存储，默认已存在一个hello-world.md文章,这个文章就是刚开始搭建博客展现出来的，相信你已经看过了。

>新建文章可以用`hexo new <title>`,也可以指定一个layout属性，指定文章作为其他形式存放在别的目录，例如page新页面、draft草稿等。详细参考[hexo|写作](https://hexo.io/zh-cn/docs/writing)


>然后可以直接去那个文件下,打开进行编辑，网上有很多这些markdown编辑器，如小书匠等等(支持在线)，我使用sublime Text的。基本截图如下：

![img](http://qnpic.top/hexo3%5C1.jpg)

![img](http://qnpic.top/hexo3%5C2.jpg)

![img](http://qnpic.top/hexo3%5C3.jpg)

>emmmm,至于如何写markdown格式的文章，emmmm，网上应该有相关的教程，我就不献丑，可以参看这几篇[博客](https://www.jianshu.com/p/191d1e21f7ed)


## 2.使用Hexo Admin插件

>[Hexo Admin](https://github.com/jaredly/hexo-admin) 是一个本地在线式文章管理器，可以用直观可视化的方式新建、编辑博客文章、page页面，添加标签、分类等，并且支持剪贴板粘贴图片（自动在source_images_目录中创建文件),是不是感觉很棒，接下来我就简单讲述下Hexo Admin插件的安装

1.在你的hexo站点目录下，输入以下代码进行安装

```
npm install --save hexo-admin
```

2.下面你就可以启动服务，进行检查是否安装成功,**注意**，浏览器输入网址是:localhost:4000/admin,后面记得加`/admin`,4000是你的端口号，记得改成你的启动端口号

```
hexo s -d
```

![img](http://qnpic.top/hexo3%5C4.jpg)

![img](http://qnpic.top/hexo3%5C5.jpg)


## 5.添加阅读统计

>给每条博客添加阅读统计，效果图如下

![img](http://qnpic.top/hexo3%5C6.jpg)

>1.先进入[leancloud官网](https://leancloud.cn/),自行进行注册和登录，然后创建应用

![img](http://qnpic.top/hexo3%5C7.jpg)

>2.进入设置界面

![img](http://qnpic.top/hexo3%5C8.jpg)

>3.应用key,后面要设置的

![img](http://qnpic.top/hexo3%5C9.jpg)

>4.将App ID、App Key 配置到`next/_config.yml`中`leancloud_visitors`

```
leancloud_visitors:
  enable: true 设置为true 默认为false
  app_id:  #你的App ID，注意冒号后面空格
  app_key:  #你的App Key，注意冒号后面空格
  Dependencies:  https://github.com/theme-next/hexo-leancloud-counter-security #设置依赖
  security: true #如果您不关心lc计数器中的安全性并且只想直接使用它（没有hexo-leancloud-counter-security插件），请将`security`设置为`false`。
  betterPerformance: true #更好的性能
```

>5.在leancloud存储的位置创建Class,必须命名为`Counter`

![img](http://qnpic.top/hexo3%5C10.jpg)

* 注：这里主要是创建时需要注意的命名`Counter`,还有在`next/config.yml`中的`leancloud_visitors`配置。

## 添加评论

>我们来进行一下文章添加评论系统吧。来设置一些常用的评论系统，我的博客选择的是[Valine](https://leancloud.cn),也是上面的那个网站。重新创建一个应用。前三步骤是一样的，创建的appid和appkey要进入`next/_config.yml`配置

```
valine:
  enable: true # 设置为true，默认为false
  appid:  # 将应用key的App ID设置在这里
  appkey: # 将应用key的App Key设置在这里
  notify: true# 邮箱通知 , https://github.com/xCss/Valine/wiki，默认为false
  verify: true# 验证码 默认为false
  placeholder: Just go go ^_^ # 初始化评论显示，根据自己修改，这里默认，
  avatar: wavatar # 头像风格，默认为mm，可进入网址：https://valine.js.org/visitor.html查看头像设置，这里有许多头像风格，进行设置
  guest_info: nick,mail,link # 自定义评论标题
  pageSize: 10 # 分页大小，10页就自动分页
  visitor: true # 是否允许游客评论 ，进入官网查看设置：https://valine.js.org/visitor.html
```

>效果图

![img](http://qnpic.top/hexo3%5C11.jpg)

这样就完成了valine评论的配置了，接下来就可以进行评论了，我们还可以在后台查看评论信息。

>在valine后台，存储位置中的数据里面创建Class，名称必须为命名为`Comment`

![img](http://qnpic.top/hexo3%5C12.jpg)

![img](http://qnpic.top/hexo3%5C13.jpg)

>至此评论就配置完毕了,gif效果图如下

![img](http://qnpic.top/hexo3%5CGIF.gif)

<br><br><br>
以上就是本篇博客的全部内容了，祝你写博客愉快，So
<br><br>Just have fun...