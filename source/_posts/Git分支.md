title: Git分支
author: Lightfish
tags:
  - Git操作
  - ''
categories:
  - Git
date: 2018-12-13 17:05:00
---
# Git分支的简单讲解

>上一个博客也是写了Git的简单操作和使用，我也是好好的去恶习了一下，今天就来说一下Git的分支。

<!-- more -->

## 讲解环境

>我把上次github的仓库给删了，重新创建了一个,这样可能看的更加的清楚

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7ktkij31gg0q6n25.jpg)

>在本地我也是删除了以上的那个文件夹，重新来一遍，理一下思路

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7nziej30gj09dac1.jpg)

## 讲解过程

>1. 先在`master`分支下创建`README.md`和`a.txt`，并输入一些信息`Hello Git`和`This is a.txt`。(初始化Git仓库就有`master`分支)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7qnf9j30gf0a640k.jpg)

### 创建分支

>创建分支是用`git branch <branch_name>`

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7tyxmj30bu01vaa7.jpg)

>嘿嘿`fatal: Not a valid object name: 'master'`,你如果出现这个错误，是因为没有提交对象，要先commit一次`master`分支才是真正的建立,所以我们就`commit`咯

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7wgaej30ga0ab76t.jpg)

>**注意** `git branch`就是参看当前的分支哦~

### 切换分支

>切换分支是用`git checkout <branch_name>`

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd7z13kj30cf02p0t4.jpg)

切换到`feature1`

>这里可以再扩展一下 `git checkout -b <branch_name>` 就是创建并跳转 记住哦是`git checkout -b`

### 删除本地的分支

```
git branch -d feature1 # 删除分支
```

>如果你在要删除的分支下，创建一些东西但是没有合并到主分支，他就可能就显示一些小错误，你可以使用`git branch -D`加你要删除的分支名。一般大写的都有一定的强制型

### 分支的功能

>下面我们就用实例来了解git分支的作用和基本功能

>我们先在`feature1`的分支下，创建一个`b.txt`，并输入信息用于等会辨认

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd85khgj30ga0a4gnr.jpg)

>在`feature1`分支下创建了b.txt并commit一次。commit完了后，我们就可以进行比较

>`ls`查看`feature1`分支下的文件，发现有`a.txt`,这是因为我们是在`master`分支下创建了`feature1`的分支，你可以理解成在`master`下又引申出去了一条`feature1`分支，所以有着master分支下的a.txt

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd88h3bj30d001ujrf.jpg)

>然后，我们在切换到master分支下查看

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd8bba8j30gg049wf4.jpg)

>上面可以看到，在`master`分支下，只有原先的`a.txt`和`README.md`文件，这是符合我上述的理解

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxd8fuxzj30tf08twf2.jpg)

## 上传到远程仓库

>上传到github上

```
git remote add origin https://github.com/QGtiger/git_test.git
git push -u origin master
```
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfobfvxj30g406zdhq.jpg)

>相信这里我们就能很好的理解master的用意了，就是上传到远程仓库的master分支上，这个`-u`是为了第一次以后推送就只需要`git push`就行

>那能不能上传到分支上呢? 相信大家都大致能推理出来，就是用下列的代码

```
git push origin feature1 # 后面这个feature1就是本地的分支名称哦
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfofaibj315s0ep0xa.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfotsaaj310l0do0x0.jpg)

>而且远程仓库的分支确实比master分支多了一个b.txt

### 上传的时候修改分支的名称

>在后面加`:`和你想要的分支名称

```
git push origin feature1:f2 # 后面这个feature1就是本地的分支名称哦
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfoxfg6j31200fm434.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfp3cqcj30e70f7756.jpg)

### 删除远程仓库的分支

```
git push origin :f2 # 在你要删除的分支前加':'
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxfp6py6j31200fktcx.jpg)


<br><br><br>这次的博客就基本到这了，讲解了git仓库的基本操作，So<br><br>Just for fun...