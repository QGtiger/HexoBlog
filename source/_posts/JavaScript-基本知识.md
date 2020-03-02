title: JavaScript 基本知识
author: Light Fish
date: 2019-07-25 15:29:23
tags:
---
# JavaScript 基础知识

>本篇博客简单收录了JavaScript的一些基本知识点

<!-- more -->

## 类的创建和继承

1）类的创建(es5)，new一个function，在这个function的prototype 里面增加属性和方法。

下面创建了一个Animal 类：

```
function Animal(name){
    this.name = name || 'Animal';
    this.sleep = function(){
        console.log(this.name+' is sleeping...')
    }
}
//原型方法
Animal.prototype.eat = function(food){
    console.log(this.name+'is eatting '+food);
}
```

2） 类的继承——原型链的继承

```
function Cat(){}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true
console.log(cat instanceof Cat); //true
```



构造继承：使用父类的构造函数来增强子类实例，等于复制了父类的实例属性（没用到原型）

```
function Cat(name){
Animal.call(this);
this.name = name || 'Tom';
}
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```

### 拓展

先看下面的代码

```
class Animal{
	constructor(name){
		this.name = name || 'Animal';
	}
	sleep(){
		console.log(this.name+" is sleeping...");
	}
}

let cat = new Animal('cat')
cat.sleep()
// cat is sleeping...
Animal.prototype.eat = (food)=>{console.log(this.name+" eat "+food)}
// (food)=>{console.log(this.name+" eat "+food)}
cat.eat("fish")
// cat eat fish

cat.__proto__.drink = (x)=>{console.log(this.name+" drink "+x)}
// (x)=>{console.log(this.name+" drink "+x)}
cat.drink("water")
// cat drink water
```

> （以上是ES6的写法）JavaScript 是通过原型来实现继承的，在JS一个构造函数默认都带有一个prototype属性，这个属性值是一个`对象`，同时这个prototype对象自带有一个constructor属性，这个属性指向这个构造函数，同时每一个实例都会有一个`__proto__`属性指向这个prototype对象，我们可以把这个叫做隐式原型
>
> 一下就是实例对象

```
cat
Animal {name: "cat"}
	name: "cat"
	__proto__:
        drink: (x)=>{console.log(this.name+" drink "+x)}
        eat: (food)=>{console.log(this.name+" eat "+food)}
        constructor: class Animal
        sleep: ƒ sleep()
        __proto__: Object
```

### 概念

> 每个对象都有一个私有属性: `__proto__`,这个属性指向它的构造函数的原型对象(prototype)。它的原型对象也有自己的原型对象。这样层层向上只至这个原型对象的属性为null。null没有自己的原型对象。
>
> 除了原型属性`__proto__`获取对象的原型,还可以使用ES6的方法Object.getPrototypeOf获取对象的原型

### 继承方法

当继承的函数被调用时，thi指向的当前继承的对象，而不是继承的函数所在的原型对象，如下

```javascript
var o = {
	a: 2,
	m(){
		return this.a;
	}
}
o.m()
// 2
var p = Object.create(o);
/*下面的两居和和上面的效果一样
	var p = {};
	p.__proto__ = o;
*/

p.a = 4
// 4
p.m()
// 4
```

### `__proto__`和prototype 的关系

> `__proto__`属性指向构造当前对象的构造函数的原型(`prototype`),它保证了对象实例能够访问在构造函数原型中定义的所有属性和方法。

> JavaScript中的属性prototype是一个指针，prototype指向**原型对象**，这个对象包含了所有实例共享的属性和方法。prototype指向的原型对象又有一个属性**constructor**,这个属性也是一个指针，指向原构造函数，即这个方法。

## JS 拖拽功能的实现

> 首先是三个事件，分别是mousedown，mousemove，mouseup，当鼠标点下后，需要一个tag标识。执行到mousemove里面。clientX，clientY标识的是鼠标的坐标。并且使用offsetX和offsetY来表示元素的初始坐标。

```
//获取元素
var dv = document.getElementById('dv');
var x = 0;
var y = 0;
var l = 0;
var t = 0;
var isDown = false;
//鼠标按下事件
dv.onmousedown = function(e) {
    //获取x坐标和y坐标
    x = e.clientX;
    y = e.clientY;

    //获取左部和顶部的偏移量
    l = dv.offsetLeft;
    t = dv.offsetTop;
    //开关打开
    isDown = true;
    //设置样式  
    dv.style.cursor = 'move';
}
//鼠标移动
window.onmousemove = function(e) {
    if (isDown == false) {
        return;
    }
    //获取x和y
    var nx = e.clientX;
    var ny = e.clientY;
    //计算移动后的左偏移量和顶部的偏移量
    var nl = nx - (x - l);
    var nt = ny - (y - t);

    dv.style.left = nl + 'px';
    dv.style.top = nt + 'px';
}
//鼠标抬起事件
dv.onmouseup = function() {
    //开关关闭
    isDown = false;
    dv.style.cursor = 'default';
}
```

## 异步加载js的方法

> defer属性和async

## 实现一个once的函数，传入函数参数只执行一次

```
function once(func){
    let tag = true;
    return function(){
        if(tag == true){
            func.apply(null,argument);
            tag = false;
        }
        return undefined;
    }
}
```

## 将原生的ajax封装成promise

```
var myNewAjax = function(url){
    return new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.open('get', url);
        xhr.send(data);
        xhr.onreadystatechange = function(){
            if(xhr.status == 200 && readyStatus == 4){
                var json = JSON.parse(xhr.responseText);
                resolve(json)
            }else if(xhr.readyStatus == 4 && xhr.status!=200){
                reject('error')
            }
        }
    })
}
```
## 代码的执行顺序

```javascript
setTimeout(function(){console.log(1)},0);
new Promise(function(resolve,reject){
	console.log(2);
	resolve();
}).then(function(){console.log(3)
}).then(function(){console.log(4)});
process.nextTick(function(){console.log(5)});
console.log(6);
```

具体参考如下文章

https://github.com/forthealllight/blog/issues/5

## 如何实现sleep的效果

### 1.while 循环

```
function sleep(ms){
    let start = Date.now();
    expire = start + ms;
    while(Date.now()<expire);
    console.log("sleep function");
    return;
}
```

### 2.promise 来实现

```
function sleep(ms){
    let temple = new Promise((resolve)=>{
        console.log(`sleeping ${ms}ms`);
        setTimeout(()=>{resolve(ms)}, ms);
    });
    return temple;
}

sleep(1000).then((ms)=>{
    console.log(`slept ${ms}ms`);
})
```

### 3.通过Generator函数来实现

```
function* sleep(ms){
    yield new Promise((resolve, reject)=>{
        console.log(`sleeping ${ms}ms...`);
        setTimeout(()=>{resolve(ms)}, ms);
    })
} 

sleep(1000).next().value.then((ms)=>{console.log(`slept ${ms}ms`)})
```

## 对象深度克隆的简单实现

> 这里我简单拓展下，数组，set，map或者类似数组的数据结构，可以用`...`来操作
>
> 如：

```javascript
a1 = [1,2,3]
b1 = [...a1]

a2 = new Map().set("a",1).set("b",2)
b2 = new Map([...a2])
```

> ES5的常用的对象克隆的一种方式。注意数组是对象，但是跟对象又有一定区别，所以我们一开始判断了一些类型，决定newObj是对象还是数组~

```
function deepClone(obj){
    let newObj = obj instanceof Array ? []:{};
    for(let item in obj){
        let temple = typeof obj[item] == 'object' ? deepClone(obj[item]):obj[item];
        newObj[item] = temple;
    }
    return newObj;
}
```

## js 判断类型

> 判断方法： typeof(), instanceof, Object.prototype.toString.call()等
>
> **注意**  ： 比如要判断数组，不能使用typeof ，因为它识别的是object

## 数组的常用方法

push()，pop()，shift()，unshift()，splice()，sort()，reverse()，map(), forEach, entries(), slice(), keys, values, every, some等

## 数组去重

- 1. indexOf 循环去重
  2. ES6 Set 去重 ; [...new Set(array)] 或者 Array.from(new Set(array))
  3. Object 键值对去重；把数组的值存成 Object 的 key 值，比如 Object[value1] = true，在判断另一个值的时候，如果 Object[value2]存在的话，就说明该值是重复的

## 闭包

什么是闭包：

闭包是指有权访问另外一个函数作用域中的变量的函数。

闭包就是函数的局部变量集合，只是这些局部变量在函数返回后会继续存在。闭包就是函数的“堆栈”在函数返回后并不释放。如下就是闭包的一个简单的实用方法(计数器)

```
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
 
add();
add();
add();
//计算器为3
```

## 去除字符串首尾空格

使用正则` (^\s*)|(\s*$)`

```javascript
"  aaa ".replace(/(^\s*)|(\s*$)/g,"")
"aaa"
```

## JS中继承实现的几种方式

1、原型链继承，将父类的实例作为子类的原型，他的特点是实例是子类的实例也是父类的实例，父类新增的原型方法/属性，子类都能够访问，并且原型链继承简单易于实现，缺点是来自原型对象的所有属性被所有实例共享，无法实现多继承，无法向父类构造函数传参。

2、构造继承，使用父类的构造函数来增强子类实例，即复制父类的实例属性给子类，

构造继承可以向父类传递参数，可以实现多继承，通过call多个父类对象。但是构造继承只能继承父类的实例属性和方法，不能继承原型属性和方法，无法实现函数服用，每个子类都有父类实例函数的副本，影响性能

3、实例继承，为父类实例添加新特性，作为子类实例返回，实例继承的特点是不限制调用方法，不管是new 子类（）还是子类（）返回的对象具有相同的效果，缺点是实例是父类的实例，不是子类的实例，不支持多继承

4、拷贝继承：特点：支持多继承，缺点：效率较低，内存占用高（因为要拷贝父类的属性）无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）

5、组合继承：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

6、寄生组合继承：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点

## 写一个函数，每秒打印

- 1. let 块级作用域

```javascript
for(let i = 0; i < 5; i++){
    setTimeout(function(){
        console.log(i)
    }, 1000*i)
}
```

这里一定要var，否则都是输出5

- 2.闭包

```javascript
for(var i = 0; i < 5; i++){
    (function(i){
        setTimeout(()=>{
            console.log(i)
        }, 1000*i)
    })(i)
}
```
## 综合深拷贝

```javascript
function Deepclone(obj){
    let copy;
    switch(typeof obj){
        case "undefined":
        break;
        case "number":
        copy = obj - 0;
        break;
        case "string":
        copy = obj + "";
        break;
        case "boolean":
        copy = obj;
        break;
        case "object":
        if(obj === null){
            copy = null;
        }else{
            if(Object.prototype.toString.call(obj).slice(8,-1) === "Array"){
                copy = [];
                for(var i = 0; i < obj.length; i++){
                    copy.push(Deepclone(obj[i]));
                }
            }else{
                copy = {};
                for(var j in obj){
                    copy[j] = Deepclone(obj[j])
                }
            }
        }
        break;
        default:
        copy = obj;
        break;
    }
    return copy;
}
```

