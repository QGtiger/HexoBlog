title: React 实践知识
author: Light Fish
date: 2020-02-13 12:33:04
tags:
---
# React 时间知识点

> 最近也是闲来无事，就想去较为深入的学习下React的开发应用，虽说之前也是稍微系统的学些了下，但只是在学习的基础上，并没有实践到真正的应用上，这次也是简单的实践了下。并借于本篇文章总结下一些常用的知识点。

<!--more-->

## 需求

> 我这次写的应用是算是一个我LightBlog的React重构吧。功能模块我就不细说了，一般就是前台的展示和后台的管理，中间加个登录。这次我先是写了登录和后台的总的页面布局，用的ui组件是`antd`。下面就是我的一些常用的知识点。

### Route 组件封装——验证是否登录

> 需求就是在路由的跳转之前就对该路由进行是否登录的验证

> 1. 登录后的cookie处理，一下就是cookie验证和存储所需要用到的函数。

```javascript
const LOGIN_COOKIE_NAME = 'sessionId'

export function isAuthenticated () {
  return _getCookie(LOGIN_COOKIE_NAME)
}

export function authenticateSuccess () {
  _setCookie(LOGIN_COOKIE_NAME, ...arguments)
}

export function logout () {
  _setCookie(LOGIN_COOKIE_NAME, '', 0)
}

function _getCookie (name) {
  let start, end
  if (document.cookie.length > 0) {
    start = document.cookie.indexOf(name + '=')
    if (start !== -1) {
      start = start + name.length + 1
      end = document.cookie.indexOf(';', start)
      if (end === -1) {
        end = document.cookie.length
      }
      return unescape(document.cookie.substring(start, end))
    }
  }
  return ''
}

function _setCookie (name, value, expire) {
  let date = new Date()
  date.setDate(date.getDate() + expire)
  document.cookie = name + '=' + escape(value) + '; path=/' +
    (expire ? ';expires=' + date.toGMTString() : '')
}
```

> 2. 登陆完以后就调用 _setCookie 函数，完成保存。但是如果之间输入路由就需要 我所要用到的 路由验证组件

```javascript
import React from 'react';
import { Route, Redirect } from 'react-router-dom';
import { isAuthenticated } from '../../utils/index';

const PrivateRoute = ({ component: Component, ...rest }) => (
    <Route {...rest} render={(props) => (
        !!isAuthenticated() ? <Component {...props}/> : <Redirect to={{
            pathname: '/login',
            state: {from: props.location}
        }}/>
    )}>

    </Route>
)

export default PrivateRoute;
```

以上就是 路由的验证组件——PrivateRoute, 通过验证方法 `isAuthenticated()` 来判断是否登录，**注意**，这里的 `{pathname: '/login', state:{from: props.location }}`就是储存了当前的路由，用户登录后的路由重新跳转。



> 3.登陆完以后的储存和跳转

```javascript
authenticateSuccess(username, 1)
const {from} = this.props.location.state || {from: {pathname: '/'}}
this.props.history.push(from)
```

### 组件的跳转页面 loading 显示和react-loadable的使用

> 这里使用 了 `nprogress` 和 `react-loadable` , `nprogress` 是用户页面的中间件跳转，而`react-loadable`减少 组件依赖文件的第一次加载，加快页面响应



```javascript
import React from 'react';
import Loadable from 'react-loadable';
import NProgress from 'nprogress';
import 'nprogress/nprogress.css';

class LoadingPage extends React.Component {
    //类似github页面加载的那个加载条
    componentWillMount(){
      NProgress.start()
    }
    componentWillUnmount(){
      NProgress.done()
    }
    render () {
      return (
        <div/>
      )
    }
  }
  
  const LoadableComponent = (component) => {
    return Loadable({
      loader: component,
      loading: ()=><LoadingPage/>
    })
  }
  
  export default LoadableComponent
```

### TypingCard 组件的编写

> TypingCard 是一个动态打字效果的组件

- 比如单纯的文字打字效果

```javascript
let divTyping = document.getElementById('text')
let i = 0,
	timer = 0,
	str = '用JS实现动态打字效果'

function typing () {
    if (i <= str.length) {
        divTyping.innerHTML = str.slice(0, i++) + '_'
        timer = setTimeout(typing, 200)
    }
    else {
        divTyping.innerHTML = str//结束打字,移除 _ 光标
        clearTimeout(timer)
    }
}
```

> 如果只是单纯的打印文字上面的函数就够了，但是想打印带标签的就需要再深入一下了，可以参考这篇[博客](https://www.cnblogs.com/coffeedeveloper/p/3852894.html)

#### 原理

- 获取打印的内容
- 将内容转化为字符和对象组成的数组中（对象是用来保存dom节点的）
- 获取数组的第一个元素并在原数组中删除，然后判断：是字符则打印；是对象，则创建dom节点，并重复第一步
- 数组长度为空时，判断对象是否有parent属性，有则回到第一步，没有则打印完毕

大致源码如下：



```javascript
class Typing {
    constructor(opts){
        this.opts = opts || {};
        this.source = opts.source; // 显示全部内容。可以display:none 隐藏掉
        this.output = opts.output; // 显示区域
        this.delay = opts.delay || 120;
        this.chain = {
            parent: null,
            dom: this.output,
            val: []
        };
        if(!(typeof this.opts.done === 'function')) this.opts.done = function(){

        };
    }

    init() {
        //初始化函数
        this.chain.val = this.convert(this.source, this.chain.val);
    }

    convert(dom, arr){
        //将dom节点下的所有子节点转化为数组
        let children = Array.from(dom.childNodes);
        for(let i = 0; i < children.length; i++){
            let node = children[i];
            if(node.nodeType === 3){ // 代表元素或属性中的文本内容
                arr = arr.concat(node.nodeValue.split(''))
            }else if(node.nodeType === 1){ //代表元素
                let val = [];
                val = this.convert(node, val);
                arr.push({
                    'dom': node,
                    'val': val
                })
            }
        }
        return arr;
    }

    print(dom, val, callback) {
        // 在 dom 后面添加元素
        setTimeout(function(){
            dom.appendChild(document.createTextNode(val));
            callback();
        }, this.delay)
    }

    play(ele){
        // 当打印最后一个字符，动画完毕，执行done
        if(!ele.val.length){
            if(ele.parent) this.play(ele.parent);
            else this.opts.done();
            return;
        }
        let current = ele.val.shift(); //去第一个元素，同时删除数组的第一个元素
        if(typeof current === 'string'){
            this.print(ele.dom, current, ()=>{
                this.play(ele); // 继续打印下一个字符
            })
        }else{
            let dom = current.dom.cloneNode(); //克隆节点，不可隆子节点，所有不用参数true
            ele.dom.appendChild(dom);
            this.play({
                parent: ele,
                dom,
                val: current.val
            })
        }
    }

    start() {
        this.init();
        this.play(this.chain);
    }
}

export default Typing;
```

> 以上只是将Tpying的核心代码封装成了一个类对象，传入的参数有，需要逐字显示的div内容`source`和打印在元素`output`下，其中的可选参数也有许多，如`delay`，`done`等。在正常的使用如下流程

- 1. 获取要打印的内容(this.source)，并将source内容转化为数组，文本节点就转化为字符串数组，将dom节点转化为对象，最后合并成一个数组
  2. 然后依次打印数组内的字符，若是遇到dom的转化对象，则需要先创建节点在打印节点内容，因为克隆的是对象，所以节点的样式都会存在。
  3. 源码如上，使用用法如下(封装了一个 TypingCard)

```javascript
import React from 'react';
import { Card } from 'antd';
import Typing from '../../utils/typing';

class TypingCard extends React.Component{
    static defaultProps = {
        title: '何时使用',
        source: '暂无内容',
        height: 136,
        delay: 50
    }

    componentDidMount() {
        console.log(this.source)
        const typing = new Typing({
            source: this.source,
            output: this.output,
            delay: this.props.delay
        })
        typing.start();
    }

    render() {
        return (
            <Card hoverable bordered={false} title={this.props.title} style={{minHeight:this.props.height}} id={this.props.id}>
                <div style={{display: 'none'}} ref={ref=>this.source=ref} dangerouslySetInnerHTML={{__html:this.props.source}}></div>
                <div ref={ref=>this.output=ref}></div>
            </Card>
        )
    }
}

export default TypingCard;
```

