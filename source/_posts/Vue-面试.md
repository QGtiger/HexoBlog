title: Vue 面试
author: Light Fish
date: 2019-07-17 14:06:41
tags:
---
# Vue 面试整理

> 本篇博客就来简单整理下我学过的Vue前端框架

<!-- more -->

## 1.什么是MVVM

> MVVM 是 Model-View-ViewModel 的缩写，MVVM是一种设计理念。Model 层代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑； View代表 UI组件(视图层)；ViewModel 是一个同步View和Model 的 对象(桥梁)。



> 在MVVM中Model和View并没有直接的联系，而是通过ViewModel 进行交互。也正是因为这样，开发者就只需要关注业务逻辑，不需要手动操作DOM。

## 2. MVVM和MVC的区别，它和其他框架(jquery)的区别

> MVC和MVVM的区别并不是很大，都是一种设计理念。只是MVC中的Controller演变成了MVVM中的ViewModel。MVVM主要解决 MVC中大量的DOM操作使得页面渲染性能降低，加载速度变量，影响用户体验。
>
> vue 和jquery的区别是，vue是数据驱动，而jquery是事件驱动，vue少了很多的Dom操作

## 3.Vue的优点

- 1.低耦合，View和Model之间的低耦合
- 2.可重用性。组件之间的调用。一个组件也可以被另一个View调用
- 3.独立开发。开发人员可以专注业务逻辑和数据的开发，设计人员可以专注于页面的设计
- 4.可测试性。

## 4.组件的传递

> 父组件向子组件的传递
>
> 父组件标签上定义传值： eg = 'data'  父组件中 data (){return {data: 'egdata'}}
>
> 子组件 通过 props 接收数据， props: ['eg']

> 子组件向父组件传递
>
> 子组件通过$emit 方法传递

### 子组件向子组件传递

> 这里可以使用`bus`,或者使用 状态管理工具Vuex

## 5.路由的跳转

### 1.声明式(标签式跳转)

> 使用 `router-link` 实现跳转

`<router-link to="/Register">注册</router-link>`

### 2.编程式(js 跳转)

> 使用js跳转

```
this.$router.push('/Register');  //这里的/Register 是路由词典中配置的
```



## 6.vue-cli 是怎样使用自定义组件的

> 1.现在你的components目录中新建你的组件文件，如loginForm.vue, script 一定要使用export default{}

> 2.在需要的页面（组件）中导入： import loginForm from '@/components/loginForm.vue'

> 3.注入vue的子组件中的components 属性中： components: {loginForm}

> 4.在试图中view中使用，如loginForm，使用时 login-form

## 7.Vuex 面试相关

### 1.vuex是什么，怎么使用

> vue 框架的状态管理。在main.js中引入store,注入。

```
import store from './store'


new Vue({
el:'#app',
store
})
```

> 上面的 './store'  就是自己创建一个store文件夹，里弥漫配置vuex.store,

### 2.vuex 有哪些属性

> 有五种，分别是 State、Getter、Mutation、Action、Module

- vuex的State 属性

  > 1.vuex 既是一个仓库，仓库里面放了很多的对象。而State就是数据源存放地，对应于一般vue 的data
  >
  > 2.state里面的存放的数据是响应式的，vue组件从store中的数据发生改变，依赖于这个数据的组件也会发生更新
  >
  > 3.他可以通过 mapState 把全局的state和 getters映射到当前组件的computed计算属性

- vuex Getter 属性

  > 1.getters 可以对State进行计算属性，它就是Store的计算属性
  >
  > 2.虽然在组件内也可以做计算属性，但是getters可以在多组件之间复用
  >
  > 3.如果一个状态在一个组件内使用，可以不用getters

- vuex 的Mutation 属性

  > Action 类似于mutation，不同在于：Action提交的是mutation，而不是直接变更状态；Action可以包含异步操作

### 3.不用Vuex带来什么问题

- 可维护性下降
- 可读性下降
- 耦合度上升

## 8. 生命周期问题

> 总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后

- 在beforeCreate阶段，vue实例的挂载元素el和数据对象data都是undefined,还未初始化。在create阶段，vue实例的数据对象data有了，el还没有
- 载入前/后：在beforeMount阶段，vue实例的$el和data都初始化了，但在还是挂载之前的虚拟的dom节点，data.message还未替换。在mounted 阶段，vue实例挂载完成，data.message成功渲染
- 更新前/后: 当data变化，会出发beforeUpdate 和updated方法
- 销毁前/后：在执行destroy方法后

### 什么是声明周期

答： Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。

### 生命周期的作用

答：它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。 