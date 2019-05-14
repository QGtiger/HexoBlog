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

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxni4lt1j30g30agabc.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxnibk10j311j0eggp4.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxnig7skj31h70btmyb.jpg)

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

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxniix4nj31ha0jmtbk.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxnin23zj31g30oognu.jpg)

<br><br><br>
以上就是本篇博客的全部内容了，祝你写博客愉快，So
<br><br>Just have fun...