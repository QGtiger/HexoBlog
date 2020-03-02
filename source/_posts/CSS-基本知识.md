title: CSS 基本知识
author: Light Fish
tags: []
categories: []
date: 2019-07-31 12:50:00
---
# CSS 基础

> 简单的CSS知识点

<!-- more -->

## 盒子模型

> 用来装页面元素的巨型区域。CSS中的盒子模型包括IE盒子模型和标准W3C盒子模型

box-sizing:  border-box;  padding-box;  content-box

一个盒子有margin、border、padding、content

**标准盒子模型**

就是width，height是最里面的content的宽高

**IE盒子模型**

width，height是content+padding+border

## 画0.5px的线

1） 采用meta viewport

2） 采用transform: scale()的方式

## trasition和animation的区别

> https://blog.csdn.net/makerbeen/article/details/80477612

## Flex 布局

> 采用 Flex 布局的元素，称为 Flex 容器，简称 “容器”。
>
> 容器默认有两个轴： 水平的主轴(main axis)和垂直的交叉轴(cross axis)

### 容器的属性

```
1. flex-direction
2. flex-wrap
3. flex-flow
4. justify-content
5. align-items
6. align-content
```

#### 1.flex-direction 属性

flex-direction 属性决定主轴的方向

- `row`（默认值）：主轴为水平方向，起点在左端。下至上
- `row-reverse`：主轴为水平方向，起点在右端。上至下
- `column`：主轴为垂直方向，起点在上沿。左至右
- `column-reverse`：主轴为垂直方向，起点在下沿。右至左

#### 2.flex-wrap属性

它可能取三个值。

（1）`nowrap`（默认）：不换行。

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071007.png)

（2）`wrap`：换行，第一行在上方。

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071008.jpg)

（3）`wrap-reverse`：换行，第一行在下方。

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071009.jpg)

#### 3.flex-flow

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

#### 4.justify-content属性

`justify-content`属性定义了项目在主轴上的对齐方式。水平对齐

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

#### 5.align-items属性

`align-items`属性定义项目在交叉轴上如何对齐。垂直方向

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

#### 6.align-content属性

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

> ```css
> .box {
> align-content: flex-start | flex-end | center | space-between | space-around | stretch;
> }
> ```

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png)

该属性可能取6个值。

> - `flex-start`：与交叉轴的起点对齐。
> - `flex-end`：与交叉轴的终点对齐。
> - `center`：与交叉轴的中点对齐。
> - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
> - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
> - `stretch`（默认值）：轴线占满整个交叉轴。

## 浮动清除

方法一：使用带clear属性的空元素

在浮动元素后使用一个空元素如`<div class="clear"></div>`，效果如下:<div class="clear"></div>并在CSS中赋予.clear{clear:both;}属性即可清理浮动。亦可使用`<br class="clear" />`或`<hr class="clear" />`,效果：<hr class="clear" />来进行清理。

方法二：使用CSS的overflow属性

给浮动元素的容器添加overflow:hidden;或overflow:auto;可以清除浮动，另外在 IE6 中还需要触发 hasLayout ，例如为父元素设置容器宽高或设置 zoom:1。

在添加overflow属性后，浮动元素又回到了容器层，把容器高度撑起，达到了清理浮动的效果。

方法三：给浮动的元素的容器添加浮动

给浮动元素的容器也添加上浮动属性即可清除内部浮动，但是这样会使其整体浮动，影响布局，不推荐使用。

方法四：使用邻接元素处理

什么都不做，给浮动元素后面的元素添加clear属性。

方法五：使用CSS的:after伪元素

结合:after 伪元素（注意这不是伪类，而是伪元素，代表一个元素之后最近的元素）和 IEhack ，可以完美兼容当前主流的各大浏览器，这里的 IEhack 指的是触发 hasLayout。

给浮动元素的容器添加一个clearfix的class，然后给这个class添加一个:after伪元素实现元素末尾添加一个看不见的块元素（Block element）清理浮动。

## 垂直居中

法一：父元素display:flex,align-items:center;

法二：元素绝对定位，top:50%，margin-top：-（高度/2）

法三：高度不确定用transform：translateY（-50%）

法四：父元素table布局，子元素设置vertical-align:center;

## 元素消失

1. opacity：0，该元素隐藏起来了，但不会改变页面布局，并且，如果该元素已经绑定一些事件，如click事件，那么点击该区域，也能触发点击事件的
2. visibility：hidden，该元素隐藏起来了，但不会改变页面布局，但是不会触发该元素已经绑定的事件
3. display：none，把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删除掉。