title: Hexo+github搭建个人博客四
author: Lightfish
tags:
  - hexo搭建
  - hexo
toc: true
categories:
  - Hexo
date: 2018-12-16 16:26:00
---
# Hexo+github搭建个人博客

>一直拖了很久，今天就来为你们来点干货，这次我会讲解一个干货，如，word文字统计，点击爱心效果展示，live2d可爱动漫人物的设置，网易云音乐的设置，添加网站运行时间

<!-- more -->

### 1.文字统计和阅读时长的设置

>这个动能其实已经集成过的，有兴趣的话看[官网介绍](https://www.npmjs.com/package/hexo-wordcount)。这里的话，我就简单描述一遍。

#### 1.安装hexo-wordcount

```
npm i --save hexo-wordcount
```
#### 2.文件配置

>在`Hexo\themes\yilia\layout\_partial\post`下创建word,ejs文件

```
<div style="margin-top:10px;">
    <span class="post-time">
      <span class="post-meta-item-icon">
        <i class="fa fa-keyboard-o"></i>
        <span class="post-meta-item-text">  字数统计: </span>
        <span class="post-count"><%= wordcount(post.content) %>字</span>
      </span>
    </span>

    <span class="post-time">
      &nbsp; | &nbsp;
      <span class="post-meta-item-icon">
        <i class="fa fa-hourglass-half"></i>
        <span class="post-meta-item-text">  阅读时长: </span>
        <span class="post-count"><%= min2read(post.content) %>分</span>
      </span>
    </span>
</div>
```

效果基本应该和我一样

### 2.点击爱心效果设置

#### 1.在yilia/source文件下创建clicklove.js,加入以下代码

```
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```
![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxbno7f7j30p60e5jta.jpg)


#### 2.在`Hexo\themes\yilia\layout\_partial`下配置

* 修改`footer.ejs`文件，因为这个文件基本每个布局都会用到，所以在文件尾添加一下代码

```
<script type="text/javascript" src="/clicklove.js"></script>
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxbns0roj30nl0fd0vs.jpg)

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxbo19s1j30v805x74m.jpg)

### 3.设置可爱的动漫小人

#### 1.安装模块

>hexo博客根目录选择`cmd`命令窗口或者`git bash` 输入以下代码，安装插件

```
npm install --save hexo-helper-live2d
```

>各种模型[展示](https://huaji8.top/post/live2d-plugin-2.0/)

```
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```

>选择好对应的模型，使用 `npm install` 模型的包名来安装，比如我选择的的是live2d-widget-model-koharu 模型包

```
npm install live2d-widget-model-koharu
```

#### 2.配置
>打开个人Hexo博客文件根目录下的 `_config.yml` 文件，在最后添加一下代码

```
#二次元
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-haruto #这个是你要修改的
  display:
    position: left #在屏幕上的显示位置
    width: 85 #显示宽度
    height: 170 #显示高度
  mobile:
    show: false #手机端是否显示
```

![img](https://ws1.sinaimg.cn/large/006bO2RVly1fywxbo3z7mj30zi0hb0xm.jpg)

#### 3.**注意** 

>当你换了动漫人物，发现在本地并没有展示出来的时候，或者明明设置了宽高时，不用慌，你可以`hexo clean`以下，再`hexo g`生成静态文件，`hexo s`启动本地服务看看，这样应该就行了。

### 4.网站运行时间的设置

#### 1.在前面提及到footer.ejs中修改，添加以下代码

```
<span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span>  #这个就是显示的文字，注意加的位置，要显示出来
<script>
    var now = new Date(); 
    function createtime() { 
        var grt= new Date("12/03/2018 12:49:00");//此处修改你的建站时间或者网站上线时间 
        now.setTime(now.getTime()+250); 
        days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days); 
        hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours); 
        if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum); 
        mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;} 
        seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum); 
        snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;} 
        document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 "; 
        document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒"; 
    } 
setInterval("createtime()",250);
</script>
```
<br><br><br>
以上就是全部博客内容了，So<br><br>Just have fun...