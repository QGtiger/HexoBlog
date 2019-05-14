title: Hexo+github搭建个人博客二
author: Lightfish
toc: true
tags:
  - hexo搭建
categories:
  - Hexo
date: 2018-12-05 23:22:18
---
# Hexo+github搭建个人博客

#### 头像的设置和 [yilia](https://github.com/litten/hexo-theme-yilia) 主题的安装和简单配置

<!-- more -->

* 今天就来写写如何让你的博客更像一个博客

##### 1.安装主题

第一步当然是安装主题啦，相信在官方文档中也有详细的记录，我就简单总结下：
>在你的博客下输入以下代码进行安装

```
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

>然后修改站点文件下的`_config.yml`文件(用文本程序打开就行了，notepad++、sublime等)，对其中的`theme:`属性修改成我们的`yilia`,如下图：

![theme](http://lightfisher.github.io/img/hexo2/1.jpg)

上图的Hexo就是我创建的博客目录，介个`_config.yml`就是这个博客的配置文件，emmmm，才想起来我应该先讲这个配置文件的，该你的博客名等等就在这里，我马上讲。然后你现在`hexo g`一下，再启动你的hexo服务看看`hexo s`。

>**注意:**对了，你第一次运行的时候应该会出现一个错误，就是那个显示全部文章的目录会显示不出来，但它会显示解决方案，我就不献丑了，成功截图如下：

![yilia](http://lightfisher.github.io/img/hexo2/2.jpg)

![img](http://lightfisher.github.io/img/hexo2/17.jpg)

相信你已经完成了我们的主题成就，如果没有，出门后拐。<br>成功截图如下，emmm，如果你发现，你怎么没有头像，没有那个好看的动漫等等，emm，莫慌，这是我添加了其他的第三方脚本才这样，后面会讲。

##### 2.站点配置文件的最最最基本讲解
* 网站

| 参数 | 含义|
|------|------|
|title |网站的标题|
|subtitle|网站的副标题|
|description|网站的描述|
|author|你的名字|
|language|网站使用的语言(中文：zh-CN)|
|timezone|时区，默认电脑时区|
|theme|主题|

>emmm，你先就改改这几个就可以了，那有些我下次有机会在分享，或者你现在一定要专研，你可以看看这个 [博客](https://blog.csdn.net/gyq1998/article/details/78294689)，修改大致如下：

![setting](http://lightfisher.github.io/img/hexo2/3.jpg)

现在你应该看上去又舒服了点，有了自己的网站名，和简单的自我描述，但是我没有头像很难受，下面我就讲一哈头像的设置

##### 3.头像的设置和网站图标设置

>可能是我愚钝，我使用的方法是直接在源文件中修改，暴力解决，找到themes->yilia->layout->_partial文件夹下，修改left-col.ejs文件

![layout](http://lightfisher.github.io/img/hexo2/4.jpg)

>然后修改，其中的属性。头像地址有了，当然是去目标文件夹下放图片啦，放在public->img文件夹下，修改头像路径(是你的头像名称啊~~)，截图如下：

![img](http://lightfisher.github.io/img/hexo2/6.jpg)

![avatar](http://lightfisher.github.io/img/hexo2/5.jpg)

>再然后就是网站图标的设置了，如果你没有ico图标文件，你可以去[比特虫](http://www.bitbug.net/)做个你喜欢的图标，然后放在Hexo\public\img文件夹下，和上面一样。至于，这个网站图标的设置当然还是暴力解决啦，找到themes\yilia\layout\_partial\head.ejs，修改如下代码(你的图标名字啊~~)：

![img](http://lightfisher.github.io/img/hexo2/8.jpg)

![img](http://lightfisher.github.io/img/hexo2/7.jpg)

>你先`hexo g`然后启动hexo服务`hexo s`看看效果(如果端口冲突,用`hexo s -p +你的端口号`),我成功截图如下

![img](http://pjas65wzi.bkt.clouddn.com/hexo122.jpg)

移动端也可以

![img](http://lightfisher.github.io/img/hexo2/9.jpg)

##### 4.主题的配置

>这个是我最想吐槽的，我上网查询的时候都是说`_config.yml`文件，虽说没有错，但是，没说哪个，要知道，一个是站点的，也就是你创建那个博客文件目录下。但其实，是要修改themes\yilia文件夹下的`_config.yml`文件，=。=我还是无聊看主题的布局文件的时候看到的=。=

![img](http://lightfisher.github.io/img/hexo2/10.jpg)

>然后就是对其中文件的修改和理解，这里我就不阐述了，因为里面都有中文的解释，但是我还是无聊说一下吧，看见这个subnav了不，这就是为什么你点击那些什么github，简书图标没用的原因，你可以改成你想要的网址,不想要的可以注释掉，想要的当然可以取消注释=。=

![img](http://lightfisher.github.io/img/hexo2/11.jpg)

>想加友链，你就修改如下图,名字也可以改，你也可以加加我的博客链接看看，应该可以=.=

![img](http://lightfisher.github.io/img/hexo2/12.jpg)

>想改自己的更多描述，你可以修改aboutme

![img](http://lightfisher.github.io/img/hexo2/13.jpg)

>这个就有点难了，就是添加 标签和分类，实现我是实现了，但是菜单那个就没有实现，看看下次我能不能更新吧，如果你想现在就简单实现，你可以看看这个[博客](https://blog.csdn.net/ganzhilin520/article/details/79047249),我的问题是单击左边菜单中的分类，里面不显示，而我单机那个分类小图片反而是大致显示的(标签类似)

![img](http://lightfisher.github.io/img/hexo2/14.jpg)
<br>
![img](http://lightfisher.github.io/img/hexo2/15.jpg)
单击这个
<br>
![img](http://lightfisher.github.io/img/hexo2/16.jpg)
<br><br><br>
下次讲如何写属于自己的博客=.=
<br>
Just have fun...