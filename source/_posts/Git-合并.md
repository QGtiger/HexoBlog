title: Git 合并
author: Lightfish
date: 2018-12-26 16:56:24
tags:
---
# Git中的合并操作

>上次的博客，我们讲解了Git的分支基本作用，这里，我们就来拓展一下，Git中的合并操作

<!--more-->

## git log

>情景提要，上次我们是在`master`分支下有着`a.txt`和`README.md`，而创建的`feature1`分支下有`b.txt`文件

>然后回归我们的主题，再次之前，我们先来学习一下`git log`的操作，这个操作是查看git操作的日志，所以，让我们学习一下,为了更好的讲解这个较为重要的命令行`git log`,我提前使用`git merge feature1`的合并操作。So

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwhmc3q0j30fx04paas.jpg)

>在上图中，我们先是用了`git merge`操作将`feature1`分支合并到了`msater`的主分支,并用`git branch -d feature1`删除了分支

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwhmhbejj30fx060q41.jpg)

>`git log`查看当前分支的log日志，可以看到，有我前面合并操作的日志，因为`feature1`分支中只有`b.txt`所以就只有`add b.txt`

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwhmjhx9j30eq02bmxf.jpg)

>`git log --oneline` 这个命令就是上图，能让log界面更加的简洁,另外 `git log --oneline -3`就是查看前几位

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwirwc68j30gg0dzq57.jpg)

>`git show <commit id>` 如果你想看某个操作更加详细的日子，你就可以用这个命令，后面的的id就是你想看的日志id，可以清楚的看到，我们这个日志就只是添加了一个b.txt，而且b.txt的内容都有显示

>`git log --all --decorate --oneline --graph`，这个命令能让log日志看起来更加的顺眼,虽然现在看不出来，但是确实有用

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwiryv6aj30es02874n.jpg)

>上述的`git log --all --decorate --oneline --graph`,虽然有用(emmm,等会再说哪有用),但是命令实在是太长了，所以我这里在教一点干货,输入

```
vi ~/.gitconfig  #修改git的配置环境

# 输入以下代码
[alias]
	co = checkout
	br = branch
	ci = commit
	st = status
	dog = log --all --decorate --oneline --graph
```

>上述代码就是可以简易的输入，`git log --all --decorate --oneline --graph`就等于`git dog`

![alias](https://ws1.sinaimg.cn/large/006bO2RVly1fywwis1vtyj30gk0c03zd.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwjs4owtj30fi05i0tv.jpg)

## log日志讲解

>我们先`git checkout -b f1`创建分支并跳转到`f1`分支,然后在`f1`分支下生成`c.txt`，并添加`This is c.txt`,然后commit一下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwjs8g63j30gi0dcju5.jpg)

>因为前面我们配置了config，所以我们就可以使用简洁字母进行操作。另外我们可以看一眼log日志第一行的`Head -> f1`,可以理解成指针指向f1分支，也就是在f1分支下进行了后面的`add c.txt`操作，我们用`git show 11293e0`可以详细查看我们进行了什么操作

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwjscechj30g208udhf.jpg)

>上图中我们可以看到，`merge`用的是`Fast-foward`方式，这个方式是直接帮你提交了，逻辑并不是很好/有兴趣可以去研究一下，输入`git merge --help`就能看官方文档了。这里我就推荐大家用`git merge f1 --no-ff`,这个方法能让我们的日志看的更加的清晰。如下图

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwkt5wlwj30fg05njsb.jpg)

>我先是跳转到f1的分支，创建了一个d.txt文件，并提交一次，然后<br>

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwkt9vn3j30ey084ta8.jpg)

>回主master分支，它也提示了比master分支多了2个commits

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwktcjdyj30fw02fgly.jpg)

>输入`git merge f1 --no-ff`时，它会提醒了进行适当的修改，你可以不改，按一下`esc`，并按`shift'+:'`，shift加冒号，输入`wq`退出,然后输入`git log`查看，你会发现它先是在f1的分支下add一次，然后合并到master主分支。如果你还是看不出来，这个时候你就可以输入`git log --all --decorate --oneline --graph`或者是配置完了的`git dog`，`git log --oneline`也还可以，但是显然前者更加的显而易见

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwlk510kj30gd0bc76s.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwlk934gj30fk045dgd.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwlkd3ntj30fr039q3g.jpg)

## 上传到远程仓库

>我们用`git push`方法，上传到远程仓库，**注意**，这里的上传默认是`master`分支

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwqu14kuj31260byn16.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwqu4xxsj30wm0dyjt4.jpg)

>所以我们用`git push origin f1:feature1`,本地是f1分支，仓库是feature1，如果名称不对，会自动创建一个。

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwqu96u1j30ej02lq3b.jpg)

>我们先在远程仓库的master分支下修改a.txt，然后在本地的master分支下进行`git pull`操作，如果要在master分支下

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwrwl5tqj30tz08yab6.jpg)

>上述的操作，只能将github上的修改的pull到master分支内，f1分支并没有修改

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwrwoe08j30gj0a375y.jpg)

>所以我们如果想要f1分支和master分支同步，我们进入f1分支，然后执行`git merge`操作

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwrwr8e6j30g607jgmx.jpg)

>但是我们也是可以用`git rebase master`的方法合并，因为我也是不是十分的清楚这两者的关系，所以我就简单得把我的理解阐述一遍

### merge rebase 区别

>merge 操作试讲两个分支的历史连接在一起;合并操作;更加关注历史记录

>rebase 如上就是f1分支移动到master分支的最后一次commit，不会像merge一样又一次不必要的commit，没有提交操作，只是复制

>rebase 黄金法则：绝对不要再公共分支使用rebase master

## 分支冲突

>我们先新建分支f2，并修改a.txt。然后切换到f1分支，修改a.txt文件。然后进入f2分支进行合并(merge)，这个时候就会发生冲突(记得修改要commit一次)，如下：

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwt6vy9uj30gi0e3go9.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywwt6ylkwj30g407hmyc.jpg)

>上图发生的，a.txt就变成第二个红线框，框内上面的就是本来自己的(Head),下面那个就是冲突的。所以我们就要修改，第一种方法就是直接用编辑器修改，另外一种就是输入`git mergetool`,并敲回车,就可以进去对所有发生冲突的文件进行修改(这里就不贴图了，记住这个`git mergetool`就行)

>对了修改后，会生成一个`a.txt.orig`，这个就是发生冲突的记录

<br><br><br>这次的博客就到这里了，So<br><br>Just have fun...