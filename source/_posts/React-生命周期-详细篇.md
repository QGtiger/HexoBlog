title: React 生命周期(详细篇)
author: Light Fish
date: 2020-02-13 21:14:07
tags:
---
# React 生命周期

> 在React 中也是存在着许多的生命周期，比如经常使用的`componentDidMount`,`render`, `componentWillMount`等。本篇文章就详细阐述一下各个生命周期的应用。

<!--more-->

## 前言

> 组件会随着组件的`props`和`state`改变和发生变化，他们的DOM也会发生相应的变化。

> **注意：**一个组件就是一个状态机；对于特定的输入，它总会返回一致的输出。

React 组件提供了`生命周期`和`钩子函数`去响应组件不同时刻的状态、组件的生命周期大致分为如下三个时期：

* 1.实例化

* 2.存在期

* 3.销毁期

>其中，钩子函数是我们重点关注的地方，下图就是各个钩子函数的顺序关系

![img](http://qnpic.top/2020-02-13%5C1.png)

### 实例化

>首次调用，有以下方法被调用(注意顺序，从上而下)

* 1.getDefaultProps <br/>
  这个方法是用来设置组件默认props，组件生命周期只会调用一次。但是只适合`React.createClass`直接创建的组件，使用ES6/ES7创建的这个方法不可使用，ES6/ES7 可以使用的方法如下：
 
 ```
  // es7
class Component {
  static defaultProps = {}
}
// 或者也可以在外面定义es6
// Compnent.defaultProps
 ```
  
* 2.getInitialState <br/>
	设置 state 初始化，在这个方法中你已经可以访问到`this.props`. `getInitialState` 只适合在React.createClass 使用。使用ES6如下
 
 ```
 class Component extends React.Component{
  constructor(props){
    super(props);
    this.state = {
      render: true,
    }
  }
}
 ```
 
 或者这样
 
 ```
 class Component extends React.Component{
  state = {
      render: true
  }
  render(){return false;}
}
 ```
 
* 3.componentWillMount <br/>
 	该方法会在组件首次渲染之前调用，这个是在 render 方法调用前可修改 state 的最后一次机会。这个方法很少用到。
    
* 4.render <br/>
	这个方法以后大家都应该会很熟悉，JSX 通过这里，解析成对应的虚拟 DOM，渲染成最终效果。格式大致如下：
    
```
class Component extends React.Component{
  render(){
    return (
       <div></div>
    )
  }
}
```

* 5.componentDidMount <br/>
	这个方法在首次真实的 DOM 渲染后调用（仅此一次）当我们需要访问真实的 DOM 时，这个方法就经常用到。如何访问真实的 DOM 这里就不想说了。当我们需要请求外部接口数据，一般都在这里处理。
    

### 存在期

> 实例化后，当`props`或者`state`发生变化时，下面方法依次被调用:

* 1.componentWillReceiveProps <br/>
	没当我们通过父组件更新子组件 props 时（这个也是唯一途径），这个方法就会被调用
    
```
componentWillReceiveProps(nextProps){}
```

* 2.shuldComponentUpdate <br/>
	字面意思，是否应该更新组件，默认返回 true。当返回 false 时，后期函数就不会调用，组件不会在次渲染。
 
```
shouldComponentUpdate(nextProps,nextState){}
```

* 3.componentWillUpdate <br/>
	字面意思组件将会更新，props 和 state 改变后必调用。

* 4.render <br/>
	跟实例化时的render一样，不多说

* 5.componentDidUpdate <br/>
	这个方法在更新真实的 DOM 成功后调用，当我们需要访问真实的 DOM 时，这个方法就也经常用到。
    
### 销毁期

>销毁阶段只有一个函数被调用

* componentWillUnmount <br/>
	**注意：**每当组件使用完成，这个组件就必须从DOM中销毁，此时该方法就会被调用。当我们在组件中使用了 setInterval，那我们就需要在这个方法中调用 clearInterval。如果手动使用了 addEventListener 绑定了事件，也需要解绑事件。