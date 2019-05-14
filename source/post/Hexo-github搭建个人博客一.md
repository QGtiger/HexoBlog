title: Hexo+github搭建个人博客一
author: Lightfish
toc: true
tags:
  - hexo搭建
categories:
  - Hexo
date: 2018-12-05 21:33:52
---
# Hexo+github搭建个人博客
  
>一直想搭建个人博客来着，本来想用Python，Django在自己的服务器上自己搭建，忽然想起来，以前好像见过同学用过Hexo框架搭建个人博客，所以我就先用Hexo搭建看看。因为网上的教程或多或少有时候都有点参差不齐，自己也是看得脑壳疼，所以下面我简单把我这两天搭建出现的问题和方法，写成博客，就当自己练手吧。

<!-- more -->

##### 1.环境的搭建 node.js  git的安装
这些依赖环境的安装，相信网上别的CSDN博客都有，也肯定写得比我好，我就不献丑了-.-,下面是我觉得写的蛮好的
<br>
NodeJS的安装[教程](https://blog.csdn.net/muzidigbig/article/details/80493880)
<br>
Git 的安装[教程](https://blog.csdn.net/it_hfzj/article/details/80693965)
<br>
![nodejs](/img/hexo1/1.jpg)

![git](https://lightfisher.github.io/img/hexo1/2.jpg)
<br>以上是nodejs和git的安装成功截图

##### 2.github的注册
	emmm，相信这些那么应该都有了吧，我就不进行这些无聊的操作了，我就讲一下注册完后的操作吧。注册完后，你得创建一个存储库，名字就是你 username.github.io (username是你注册的名字)

![emm](https://lightfisher.github.io/img/hexo1/3.jpg)

如上

##### 3.Hexo的安装
* 1.首先你要先进入git bash界面(应该桌面右键就有了，如果没有，你进入到开始菜单那里找到git，下面就有了)进入后的界面如下

![git bash](https://lightfisher.github.io/img/hexo1/4.jpg)

* 然后全局配置设置到淘宝源<br>

```
npm config set registry https://registry.npm.taobao.org
```

![registry](https://lightfisher.github.io/img/hexo1/5.jpg)

* 全局设置 user.email 和user.name<br>

```
git config --global user.email "your_emaill"
git config --global user.name "your_name"
```

这里的`your_email`和`your_name`就是你注册github的邮箱和用户名，截图如下：<br>

![git config](https://lightfisher.github.io/img/hexo1/6.jpg)

* 生成ssh密钥

```
cd ~/.ssh 
ssh-keygen -t rsa -C “your_email” #打自己的邮箱
```

![ssh-key](https://lightfisher.github.io/img/hexo1/7.jpg)

* 设置ssh key到GitHub 默认生成ssh key在C:\Users\username.ssh文件夹中，复制 id_rsa.pub文件到 github->settings->SSH and GPG key->new ssh key 如图 

![img](https://lightfisher.github.io/img/hexo1/8.jpg)

![setting](https://lightfisher.github.io/img/hexo1/9.jpg)

![img](https://lightfisher.github.io/img/hexo1/10.jpg)

![img](https://lightfisher.github.io/img/hexo1/11.jpg)

![img](https://lightfisher.github.io/img/hexo1/12.jpg)

* 检查ssh设置是否成功
```
ssh -T git@github.com
```

![check ssh](https://lightfisher.github.io/img/hexo1/13.jpg)

##### 4.安装hexo插件
* 输入以下代码

```
cd / #进入根目录，实际上就是git安装的根目录
pwd /
npm install hexo-cli -g #安装hexo
```

![install hexo](https://lightfisher.github.io/img/hexo1/14.jpg)

* 安装以后呢，你可以输入以下代码

```
cd /  
hexo init Hexo #我创建了一个他的项目框架
cd Hexo #进入你创建的那个目录
hexo generate #(可简写成 hexo g)
hexo server #(可简写成 hexo s)
```

>这个就是在你Git安装目录下进行初始化一个hexo博客项目，你可以直接到自己想要创建的地方进行`hexo init +<你的项目名称>` 

由于我懒得重新创了，我就用网上别人的截图，下面我会注明

![img](https://lightfisher.github.io/img/hexo1/15.jpg)

![img](https://lightfisher.github.io/img/hexo1/16.jpg)

![img](https://lightfisher.github.io/img/hexo1/17.jpg)

emmm，上面我之所以我输入hexo s -p 4005，相信聪明的人已经看出来了，因为默认4000被占用了，所以输入-p+端口号，后面这个-d相当于debug模式，基本不用管

* 到现在为止你已经完成了hexo的基本配置，你可以输入下面那个本地网址进行查看 [http://localhost:4000](http://localhost:4000),相信你一定是下面这张图，如果不是打电话联系，咳，开玩笑，如果真的出错，emmmm，下次我开个评论 TAT

![img](https://lightfisher.github.io/img/hexo1/18.jpg)

##### 5.上传到自己的github
* 首先安装部署到github插件依赖

```
npm install –save hexo-deployer-git
```

![img](https://lightfisher.github.io/img/hexo1/20.jpg)

* 打开你创建的项目下的配置文件(如果你跟着我做的话，应该是Hexo)

![img](https://lightfisher.github.io/img/hexo1/21.jpg)

* 然后修改其中的配置

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:lightfisher/lightfisher.github.io.git  #改成自己的用户名和用户名加域名
  branch: master
```
* 然后在部署到你的github<br>

```hexo deploy```可以简写成`hexo d`,代码我就不贴了

* 这是最关键的，你可以登上自己的网址`your_name.github.io`,your_name是你的github用户名。你可能要等个十几分钟，才可以看见,下面是我自己的博客

![finish](https://lightfisher.github.io/img/hexo1/22.jpg)


<br><br><br>
以上就是hexo+github的基本搭建，有空我会写，其他的配置，如,主题的配置，头像的设置，标签和分类的设置引用，第三发插件的设置等。<br>
So，Just have fun...