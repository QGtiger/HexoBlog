title: Git 的一些基本操作和讲解
author: Lightfish
toc: true
tags:
  - Git操作
categories:
  - Git
date: 2018-12-13 12:22:55
---
# Git 的一些基本操作和讲解

>Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。 [1]  Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

<!-- more -->

## Git 很强大，接下来我就讲解一下简单强大的命令

#### 1.`git version` 查看当前git版本

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhc2qh4j30g6054wf1.jpg)

#### 2.全局设置用户

```
git config --global user.name "..."  #你的账号，比如github
git cinfig --global user.email "..."  #你的邮箱地址，github账户邮箱
```
查看你的账户哦~

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhc5aqtj30b102kglq.jpg)

`cat ~/.gitconfig` 查看你的git配置,上面就是查看

#### 3.初始化git仓库，并查看状态

* `git init`初始化git仓库，初始化后会有一个.git的隐藏文件，下面我教一个如何命令行查看隐藏文件

* `ll -la` 查看隐藏文件

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhckd9wj30f803hq3l.jpg)

* `touch <file>` 命令相当于你新建一个文件 后面跟文件名(记得加后缀)

* `git status` 查看当前git仓库的一个状态

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhc93roj30gi0didir.jpg)

#### 4.git暂存区

* `git add <file>` 后面加文件名  就是把后面的文件放到暂存区，你可以用上述git status方法查看变化

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhcbnwbj30ft06pgmg.jpg)

* **注意**：上述方法只是放到git仓库的暂存区，还没有真正的提交，你也可以用

* `git rm --cached <file>` 的方法把暂存区的删除

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhcee3yj30g00cjtai.jpg)

* 上面的 `git add -A` -A 就是all，就是目录下所有文件加入暂存区

#### 5.git的提交

* `git commit -m "..."`  git commit就是提交，-m 就是可以加备注，""里面写你要加的备注，告诉别人你干了什么

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhcvwv3j30fn059q3v.jpg)

#### 6.git到你的仓库

>为了显示，我重新创建一个存储库

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhczk4oj30ie0ezgnb.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxhd4vbvj30t40l2tdf.jpg)

>1.`git remote add origin https://github.com/QGtiger/git_test.git`就是跟远程的github仓库创立链接，可以用户多个远程仓库,`origin` 是一个名字，意思是远程仓库，约定俗成的名字，约定俗成的东西就不建议修改

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkb4ggwj30dt03y3zd.jpg)

>创立链接以后，我们就可以进行`git push`操作

* `git push -u origin master` **origin**就是远程仓库的名称，**master**就是远程仓库的分支名称，默认就是master,**-u** 第一次以后推送就只需要git push就行，**注意** 当你git push的时候可能会叫你输入github的账号和密码，你也可以ssh免密上传,下次有空在讲

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkb7myij30gd04yaba.jpg)

* 登上github就可以看到变化

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkbixp7j30z10ceq8c.jpg)

#### 7.除了上述，我们本地创建git，然后，与远程仓库建立连接，我们也可以直接克隆下来

>`git clone https://github.com/QGtiger/git_test.git `  远程克隆一个仓库 后面那个就是远程克隆的仓库链接,git会克隆下来，文件名是git_test,因为我本地当前目录下有git_test文件夹，你可以在后面加空格+文件夹名字

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkbs3hxj30fe05d0tv.jpg)

>然后，我们可以简单查看目录，然后进行修改，`vi` 命令相当于进行文本文件的编辑,具体的使用方法你可以自行百度，这里不多详述

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkburipj30fp04s757.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkbx57hj30g80ajaam.jpg)
检查是都修改成功
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkc1tc5j30cl02rwer.jpg)

>`git push` 推送到github
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkc4coij30gd0acjtp.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkcb43kj30ul0f70ux.jpg)

#### `git pull` 将远程仓库的拉到本地，相当于更新

>当远程仓库被别人修改的，也就是前面的`git_test`git本地仓库，这个时候，你就可以切换到git_test 目录下进行下列操作 

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxkch14wj30fs08ptal.jpg)

<br><br><br>
今天的Git简单操作就讲到这里,So<br><br>Just have fun...