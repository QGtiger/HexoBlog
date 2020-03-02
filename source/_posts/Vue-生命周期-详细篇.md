title: Vue 生命周期(详细篇)
author: Light Fish
date: 2020-02-13 12:44:46
tags:
---
# Vue生命周期

> 每个 Vue 实例创建时，都会经历一系列的初始化过程，同时也会调用相应的生命周期钩子。

<!--more-->

## 各个生命周期

> 先放一张生命周期图

![vue 生命周期](http://qnpic.top/2020-2-13%5C1.png)

>以上可以大致看出Vue的整个生命周期和各个生命周期钩子

### 钩子函数

* beforeCreate

* created

* beforeMount

* mounted

* beforeUpdate

* updated

* beforeDestroy

* destroyed

>运行代码看运行结果

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Vue生命周期</title>
</head>
<body>
    <div id="app">
        <h1>{{message}}</h1>
    </div>

    <script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.min.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: 'Vue生命周期'
            },
            beforeCreate: function () {
                console.group('-----beforeCreate-----');
                console.log("%c%s", "color:red", "el      :" + this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            created: function () {
                console.group('-----created-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            beforeMount: function () {
                console.group('-----beforeMount-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            mounted: function () {
                console.group('-----mounted-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            beforeUpdate: function () {
                console.group('-----beforeUpdate-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            updated: function () {
                console.group('-----updated-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            beforeDestroy: function () {
                console.group('-----beforeDestroy-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            },
            destroyed: function () {
                console.group('-----destroyed-----');
                console.log("%c%s", "color:red", "el:     :" + this.$el);
                console.log(this.$el);
                console.log("%c%s", "color:red", "data    :" + this.$data);
                console.log("%c%s", "color:red", "message :" + this.message);
            }
        });
    </script>
</body>
</html>
```

>结果如下

![img](http://qnpic.top/2020-2-13%5C2.jpg)

> 由此可见，初始化的时候只调用了四个生命周期，分别是`beforeCreate`,`created`,`beforeMounte`,`mounted`.

#### 1. beforeCreate 和 created之间的生命周期

>由上可见，`beforeCreate`在初始化阶段并data并没有与数据进行绑定，而`created` data 就成功的对数据进行绑定，但是此时并没有`挂载`。el并不可用.

#### 2. created 和 beforeMounte之间的生命周期

![img](http://qnpic.top/2020-2-13%5C3.png)

>在这一阶段，很明显首先会判断对象是否有 `el 选项（option）`。如果有的话就继续向下编译；如果没有则停止编译，也就意味着停止了生命周期，直到在该 Vue 实例上调用 `vm.$mount(el)`。

做个试验，把 `el`注释掉，则

![img](http://qnpic.top/2020-2-13%5C4.jpg)

可以看到，当没有 el 选项时，代码运行到 created 的时候就停止了。

从图中可以看出，当判断有 el 选项时，代码会继续向下执行，接下来会判断是否有 template 选项。

* 1.如果 Vue 实例对象中有 template 选项，则将其作为模板编译成 render 函数。
* 2.如果没有 template 选项，则将外部 HTML（outerHTML）作为模板编译。
* 3.由此可以看出，template 中的模板优先级要高于 outerHTML 的优先级。
* 4.而假如初始化实例的时候，有render函数则先渲染render。
* 5.render 函数选项 > template 选项 > outerHTML

#### beforeMount 和 mounted 生命周期之间

>在`beforeMount`生命，此时已经给vue实例对象添加了 $el 成员，而他们之间主要就是替换挂载的Dom元素。在以下图片可以看出，beforeMount 的el对象并没有真正的显示内容，还是{{message}} 来占位的，因为此时还没有被挂到页面上，还是 JavaScript 中的虚拟 DOM 形式存在的。在 mounted 之后才发生了变化。

![img](http://qnpic.top/2020-2-13%5C5.jpg)

#### beforeUpdate 和 updated 之间的生命周期

>之间的生命周期如下

![img](http://qnpic.top/2020-2-13%5C6.png)

>从图中我们可以看到，当 vue 中的数据发生改变，会触发对应组件的重新渲染`（re-render）`，先后调用 beforeUpdate 和 updated 钩子函数。 在控制台中输入 `vm.message = '组件数据更新'`

![img](http://qnpic.top/2020-2-13%5C7.jpg)

>这里需要注意的一点是，两个函数中的 data 都已经更新为 “触发组件更新”，那区别是什么呢？

**注意**

>**这里的 update 指的是，view 层的 update，而不是 vue 对象中 data 属性的 update**。所以，这两个函数都发生在 data 改变之后（很容易理解，因为就是 data 属性的改变才触发了这两个函数），区别是 `beforeUpdate` 发生在 view 层的改变之前，也就是页面还没有重新渲染，此时页面仍然显示 “Vue的生命周期”（beforeUpdate 也是在页面重新渲染前修改 data 的最后时机）；而 updated 是发生在 view 层改变之后，也就是此时的页面已经重新渲染为 “触发组件更新”。

#### beforeDestroy 和destroyed之间的生命周期

![img](http://qnpic.top/2020-2-13%5C8.png)

>beforeDestroy 是在实例被销毁之前调用，在这一步，实例仍然完全可以调用。
destroyed 函数在实例被销毁之后调用，此时 vue 实例指示的所有东西都会解绑，所有的事件监听器会被移除，所有的子实例也会被销毁。

