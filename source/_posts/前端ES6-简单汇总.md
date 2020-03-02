title: 前端ES6 简单汇总
author: Light Fish
date: 2019-07-15 19:59:00
tags:
---
>ES6的一些知识点

<!-- more -->

# Babel 转码器

> Babel ES6转码器 
>
> l\\\配置文件 .babelrc
>
> babel-core 
>
> babel-register

## let const

> let 代码快中有效 块级作用域
>
> 暂时性死区  先声明再调用
>
> 不存在变量提升
>
> 不允许重复声明 在同一作用域  在函数内不能重复声明参数
>
> IIFE 立即执行函数  
>
> 注意：ES5不允许块级作用域中声明函数。ES6可以。
>
> 全局不能访问块级作用域中的变量

## 变量解析赋值

> 模式匹配  let [a,b,c] = [1,2,3] || let [a,b,c] = [1,2,3,4]  || let [a, [[b], b]] = [1,[[2,3],4]]，let [,,z] = [1,2,3] 一定要可迭代对象
>
> 允许默认值 var [f = true] = []   // f = true
>
> `null`，不严格相等于`undefined`，导致默认值不会生效
>
> 注意：如果默认值是一个函数，那么是惰性取值

> > **对象的解构赋值** var {f, s} = {f : 1, s :2}  注意：zhe是 : 
> >
> > ```javascript
> > var { foo: baz } = { foo: 'aaa', bar: 'bbb' }; // bax "aaa"
> > ```
> >
> > 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。前者是模式不是变量
> >
> > 注意： let和const不能重复赋值，所以
> >
> > ```javascript
> > let foo;
> > ({foo} = {foo: 1}); // 成功
> > 
> > let baz;
> > ({bar: baz} = {bar: 1}); // 成功
> > 
> > ```
> >
> > 上面代码中，`let`命令下面一行的圆括号是必须的，否则会报错。因为解析器会将起首的大括号，理解成一个代码块，而不是赋值语句。
> >
> > 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。
> >
> > ```javascript
> > var arr = [1, 2, 3];
> > var {0 : first, [arr.length - 1] : last} = arr;
> > first // 1
> > last // 3
> > ```
>
> > **字符串结构赋值**
> >
> > 字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。
> >
> > ```javascript
> > const [a, b, c, d, e] = 'hello';
> > a // "h"
> > b // "e"
> > c // "l"
> > d // "l"
> > e // "o"
> > ```
> >
> > 类似数组的对象都有一个`length`属性，因此还可以对这个属性解构赋值。
> >
> > ```javascript
> > let {length : len} = 'hello';
> > len // 5
> > ```

#### 数值和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

上面代码中，数值和布尔值的包装对象都有`toString`属性，因此变量`s`都能取到值。

### 用处

**（1）交换变量的值**

```javascript
[x, y] = [y, x];
```

上面代码交换变量`x`和`y`的值，这样的写法不仅简洁，而且易读，语义非常清晰。

**（2）从函数返回多个值**

函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
var [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
var { foo, bar } = example();
```

**（3）函数参数的定义**

解构赋值可以方便地将一组参数与变量名对应起来。

```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

**（4）提取JSON数据**

解构赋值对提取JSON对象中的数据，尤其有用。

```javascript
var jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

上面代码可以快速提取JSON数据的值。

**（5）函数参数的默认值**

```javascript
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
}) {
  // ... do stuff
};
```

指定参数的默认值，就避免了在函数体内部再写`var foo = config.foo || 'default foo';`这样的语句。

**（6）遍历Map结构**

任何部署了Iterator接口的对象，都可以用`for...of`循环遍历。Map结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

如果只想获取键名，或者只想获取键值，可以写成下面这样。

```javascript
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```

**（7）输入模块的指定方法**

加载模块时，往往需要指定输入那些方法。解构赋值使得输入语句非常清晰。

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

## 字符串的扩展

> 加强了对Unicode的支持

JavaScript内部，字符以UTF-16的格式储存，每个字符固定为`2`个字节。对于那些需要`4`个字节储存的字符（Unicode码点大于`0xFFFF`的字符），JavaScript会认为它们是两个字符。

```javascript
var s = "𠮷";

s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271
```

上面代码中，汉字“𠮷”（注意，这个字不是”吉祥“的”吉“）的码点是`0x20BB7`，UTF-16编码为`0xD842 0xDFB7`（十进制为`55362 57271`），需要`4`个字节储存。对于这种`4`个字节的字符，JavaScript不能正确处理，字符串长度会误判为`2`，而且`charAt`方法无法读取整个字符，`charCodeAt`方法只能分别返回前两个字节和后两个字节的值。

`codePointAt`方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。

```javascript
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}

is32Bit("𠮷") // true
is32Bit("a") // false
```

#### String.fromCodePoint

ES5提供`String.fromCharCode`方法，用于从码点返回对应字符，但是这个方法不能识别32位的UTF-16字符（Unicode编号大于`0xFFFF`）。

```javascript
String.fromCharCode(0x20BB7)
// "ஷ"
```

上面代码中，`String.fromCharCode`不能识别大于`0xFFFF`的码点，所以`0x20BB7`就发生了溢出，最高位`2`被舍弃了，最后返回码点`U+0BB7`对应的字符，而不是码点`U+20BB7`对应的字符。

ES6提供了`String.fromCodePoint`方法，可以识别`0xFFFF`的字符，弥补了`String.fromCharCode`方法的不足。在作用上，正好与`codePointAt`方法相反。

```javascript
String.fromCodePoint(0x20BB7)
// "𠮷"
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
// true
```

上面代码中，如果`String.fromCodePoint`方法有多个参数，则它们会被合并成一个字符串返回。

注意，`fromCodePoint`方法定义在`String`对象上，而`codePointAt`方法定义在字符串的实例对象上。

#### 字符串的遍历器接口

> for...of...

```javascript
for (let codePoint of 'foo') {
  console.log(codePoint)
}
```

除了遍历字符串，这个遍历器最大的优点是可以识别大于`0xFFFF`的码点，传统的`for`循环无法识别这样的码点。

```javascript
var text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "

for (let i of text) {
  console.log(i);
}
// "𠮷"
```

#### at

ES5对字符串对象提供`charAt`方法，返回字符串给定位置的字符。该方法不能识别码点大于`0xFFFF`的字符。

```javascript
'abc'.charAt(0) // "a"
'𠮷'.charAt(0) // "\uD842"
```

上面代码中，`charAt`方法返回的是UTF-16编码的第一个字节，实际上是无法显示的。

目前，有一个提案，提出字符串实例的`at`方法，可以识别Unicode编号大于`0xFFFF`的字符，返回正确的字符。

```javascript
'abc'.at(0) // "a"
'𠮷'.at(0) // "𠮷"
```

这个方法可以通过[垫片库](https://github.com/es-shims/String.prototype.at)实现

#### normalize

> 合成符号和重音字符

#### includes,startsWith,endsWith

#### repeat

#### padStart，padEnd 补全

#### 模板字符串	\`${}\`

```javascript
const tmpl = addrs => `
  <table>
  ${addrs.map(addr => `
    <tr><td>${addr.first}</td></tr>
    <tr><td>${addr.last}</td></tr>
  `).join('')}
  </table>
`;
```

如果需要引用模板字符串本身，在需要时执行，可以像下面这样写。

```javascript
// 写法一
let str = 'return ' + '`Hello ${name}!`';
let func = new Function('name', str);
func('Jack') // "Hello Jack!"

// 写法二
let str = '(name) => `Hello ${name}!`';
let func = eval.call(null, str);
func('Jack') // "Hello Jack!"
```

## 正则拓展

#### 正则字符串 match search replace split

#### u修饰符 正确识别码点大于`0xFFFF`的Unicode字符

#### y修饰符 

> `y`修饰符确保匹配必须从剩余的第一个位置开始，这也就是“粘连”的涵义。

```javascript
var s = 'aaa_aa_a';
var r1 = /a+/g;
var r2 = /a+/y;

r1.exec(s) // ["aaa"]
r2.exec(s) // ["aaa"]

r1.exec(s) // ["aa"]
r2.exec(s) // null
```

#### sticky 属性判断是否是y修饰符

#### flags属性 返回修饰符

> /abc/ig.flags // 'gi'

#### s修饰符：dotAll模式

> 让`.`匹配任意字符，如`\n`,`\t`等

## 数值的拓展

### 二进制和八进制的表示

> 二进制 0b 或者 0B   八进制 0o 或者 0O  （转十进制用`Number`）

### Number.isFinite(), Number.isNaN()

> isFinite 判断是否有范围，isNaN 判断是否为NaN

### Number.parseInt()  Number.parseFloat()

### Number.isInteger()

> 判断是否为整数，JavaScript内部，3===3.0

### Number.EPSILON 

> 新增极小值，判断浮点数是否精确

### 安全整数和Number.isSafeInteger()

> JavaScript中表示的整数范围是-2^53~2^53,超过就无法精确表示了如，Math.pow(2,53) === Math.pow(2,53)+1 // true

### Math.trunc()

> `Math.trunc`方法去除小数，和Number.parseInt的简单区别为 Number.parseInt("1s") // 1 
>
> 而 Math.trunc("1s") //NaN

### Math.sign()

> `Math.sign()` 方法判断一个数的正负

它会返回五种值。

- 参数为正数，返回+1；
- 参数为负数，返回-1；
- 参数为0，返回0；
- 参数为-0，返回-0;
- 其他值，返回NaN。

### Math.cbrt()

> `Math.cbrt()`方法是计算一个的立方根，非数值型先用Number方法转化，否则是NaN

### Math.clz32()

JavaScript的整数使用32位二进制形式表示，`Math.clz32`方法返回一个数的32位无符号整数形式有多少个前导0。

```javascript
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1000) // 22
Math.clz32(0b01000000000000000000000000000000) // 1
Math.clz32(0b00100000000000000000000000000000) // 2
```

### Math.fround

> 返回一个数的单精度浮点数

### Math.hypot

> 返回所有参数的平方和的平方根

```javascript
Math.hypot(3, 4);        // 5
Math.hypot(3, 4, 5);     // 7.0710678118654755
Math.hypot();            // 0
Math.hypot(NaN);         // NaN
Math.hypot(3, 4, 'foo'); // NaN
Math.hypot(3, 4, '5');   // 7.0710678118654755
Math.hypot(-3);          // 3
```

## 数组的拓展

### Array.from()

> `Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。

```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).forEach(function (p) {
  console.log(p);
});

Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```

`Array.from`方法则是还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，即必须有`length`属性。因此，任何有`length`属性的对象，都可以通过`Array.from`方法转为数组，而此时扩展运算符就无法转换。

```javascript
Array.from({ length: 3 });
// [ undefined, undefined, undefined ]
```

`Array.from`还可以接受第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```javascript
function typesOf () {
  return Array.from(arguments, value => typeof value)
}
typesOf(null, [], NaN)
// ['object', 'object', 'number']

Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```

### Array.of

> `Array.of`方法用于将一组值，转换为数组。不同于Array.from 的是，这个是一组值，而Array.from 是类似数组或者可迭代对象

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```

这个方法的主要目的，是弥补数组构造函数`Array()`的不足。因为参数个数的不同，会导致`Array()`的行为有差异。

```javascript
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```

### 数组实例的copyWithin()

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```

上面代码表示将从3号位直到数组结束的成员（4和5），复制到从0号位开始的位置，结果覆盖了原来的1和2。

下面是更多例子。

```javascript
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}
```

### 数组实例的find()和findIndex

```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5
```

上面代码找出数组中第一个小于0的成员。

```javascript
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

上面代码中，`find`方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组

```javascript
[NaN].findIndex(y => Object.is(NaN, y))
// 0
```

弥补了`indexOf`方法无法识别数组的`NaN`成员 [NaN].indexOf(NaN) // -1



### 数组实例的fill()

`fill`方法使用给定值，填充一个数组。

```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```

上面代码表明，`fill`方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```javascript
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

### 数组实例的entries(), keys()和value()

ES6提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象，可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

### 数组的方法

```javascript
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

### 数组空位的处理

ES5会跳过，ES6当做undefined 处理

## 函数的拓展

允许参数默认值

### 与解构赋值默认值结合使用

```javascript
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined, 5
foo({x: 1}) // 1, 5
foo({x: 1, y: 2}) // 1, 2
foo() // TypeError: Cannot read property 'x' of undefined
```

上述代码采用了对象解构赋值

下面是另一个对象的解构赋值默认值的例子。

```javascript
function fetch(url, { body = '', method = 'GET', headers = {} }) {
  console.log(method);
}

fetch('http://example.com', {})
// "GET"

fetch('http://example.com')
// 报错
```

上面代码中，如果函数`fetch`的第二个参数是一个对象，就可以为它的三个属性设置默认值。

上面的写法不能省略第二个参数，如果结合函数参数的默认值，就可以省略第二个参数。这时，就出现了双重默认值。

```javascript
function fetch(url, { method = 'GET' } = {}) {
  console.log(method);
}

fetch('http://example.com')
// "GET"
```

### 函数的length属性

指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，`length`属性将失真。

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

如果设置了默认值的参数不是尾参数，那么`length`属性也不再计入后面的参数了。

```javascript
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```

### 应用

利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

```javascript
function throwIfMissing() {
  throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {
  return mustBeProvided;
}

foo()
// Error: Missing parameter
```

### rest参数

> 用户获取函数多余参数，这样就不需要arguments对象 了，函数的length参数不包含rest参数

```javascript
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

const sortNumbers = (...numbers) => numbers.sort();

add(2, 5, 3) // 10
```

#### 拓展运算符

> 拓展运算符 `...`，将一个数组转化为用逗号隔开的参数排序

```
console.log([1,2,3])  // [1,2,3]

console.log(...[1,2,3]) // 1 2 3

用户函数的调用

let arr = [1,2,3]
arr.push(...[1,2,3]) // [1,2,3,1,2,3]
```

#### 代替数组apply方法

```javascript
// ES5的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);
```

```javascript
// ES5
[1, 2].concat(more)
// ES6
[1, 2, ...more]

Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
[...'hello']  // ['h', 'e', 'l', 'l', 'o']
```

**Map和Set结构，Generator函数**

扩展运算符内部调用的是数据结构的Iterator接口，因此只要具有Iterator接口的对象，都可以使用扩展运算符，比如Map结构。

```javascript
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```

Generator函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。

```javascript
var go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

### 函数name 属性

### 箭头函数

## 对象的拓展

> 允许直接写入变量和函数，作为对象的属性和方法。

```javascript
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}

// 等同于
var baz = {foo: foo};
```

> 方法简写

```javascript
var o = {
  method() {
    return "Hello!";
  }
};

// 等同于

var o = {
  method: function() {
    return "Hello!";
  }
};
```

### 属性名表达式

## Symbol

> ES6 Symbol 就是为了防止属性名的冲突而ES6才增加上去的

```
let s = Symbol();

typeof s
// "symbol"

var s1 = Symbol('foo');
var s2 = Symbol('foo');

s1 // Symbol(foo)
s2 === s1 // false
```

如果 Symbol 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 Symbol 值。

```javascript
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj); // 调用toString 方法
sym // Symbol(abc)
```

### 作为属性名的Symbol

> 防止不当心覆盖属性

```javascript
var mySymbol = Symbol();

// 第一种写法
var a = {};
a[mySymbol] = 'Hello!';

// 第二种写法  究极注意这里的属性名要用[]，括起来
var a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

> 注意以上的第二种方法，一定要用[]括起来，否则就是字符串"mySymbol" 键名

### Symbol.for, Symbol.keyFor

```javascript
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');

s1 === s2 // true
```

> Symbol.for() 和 Symbol()的区别是： 前者不会每次调用都会生成新的Symbol，会先检查key是否存在，Symbol() 就会生成

上面代码中，由于`Symbol()`写法没有登记机制，所以每次调用都会返回一个不同的值。

`Symbol.keyFor`方法返回一个已登记的 Symbol 类型值的`key`。

```javascript
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

## Set和Map数据结构

> ES6提供了新的 数据结构Set。它类似于数组和Python中的set差不多

```javascript
var s = new Set();

[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

```javascript
// 例一
var set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
var items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
function divs () {
  return [...document.querySelectorAll('div')];
}

var set = new Set(divs());
set.size // 56

// 类似于
divs().forEach(div => set.add(div));
set.size // 56
```

上述代码也展示一种去除数组重复成员的方法

```
//去除数组的重复成员
[...new Set(array)]
```

**注意**：两个对象总会不相等的

```
{} === {} // false

let set = new Set();

set.add({});
set.size // 1

set.add({});
set.size // 2
```

### Set 实例的属性和方法

Set结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `add(value)`：添加某个值，返回Set结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。

### 遍历操作

> Set结构有四种遍历操作，默认的遍历方法就是values() 方法

- keys()  返回键名的遍历器
- values() 返回键值的遍历器
- entries() 返回键值对的遍历器
- forEach() 使用回调函数遍历每个成员

而且数组的map和filter方法也是可以用于set，也因此很容易实现并集和交集

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

如果想在遍历操作中，同步改变原来的Set结构，目前没有直接的方法，但有两种变通方法。一种是利用原Set结构映射出一个新的结构，然后赋值给原来的Set结构；另一种是利用`Array.from`方法。

```javascript
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```

### map数据结构

> Javascript的对象，本质上是键值对的集合，但是传统上只能使用字符串当做键，这让它在使用上带来了很大的讽刺。所以，为了解决这一个问题，ES6提供了Map数据结构

```javascript
var m = new Map();
var o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

作为构造函数，Map也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

```javascript
var map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

注意，只有对同一个对象的引用，Map结构才将其视为同一个键。这一点要非常小心。

```javascript
var map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined
```

上面代码的`set`和`get`方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此`get`方法无法读取该键，返回`undefined`。

**遍历方法**

Map原生提供三个遍历器生成函数和一个遍历方法。

- `keys()`：返回键名的遍历器。
- `values()`：返回键值的遍历器。
- `entries()`：返回所有成员的遍历器。
- `forEach()`：遍历Map的所有成员。

结合数组的`map`方法、`filter`方法，可以实现Map的遍历和过滤（Map本身没有`map`和`filter`方法）。

```javascript
let map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

let map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生Map结构 {1 => 'a', 2 => 'b'}

let map2 = new Map(
  [...map0].map(([k, v]) => [k * 2, '_' + v])
    );
// 产生Map结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```

此外，Map还有一个`forEach`方法，与数组的`forEach`方法类似，也可以实现遍历。

```javascript
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

`forEach`方法还可以接受第二个参数，用来绑定`this`。

```javascript
var reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
```

上面代码中，`forEach`方法的回调函数的`this`，就指向`reporter`。

### 与其他数据结构的相互转换

1) Map 转化为数组，使用拓展运算符，Array.from

```javascript
let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

2) 数组转为Map，使用new Map() 就行

```javascript
new Map([[true, 7], [{foo: 3}, ['abc']]])
// Map {true => 7, Object {foo: 3} => ['abc']}
```

3) Map 转化为对象

所有Map键都是字符串才能转化为对象

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

4) 对象转化为Map

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// [ [ 'yes', true ], [ 'no', false ] ]
```

5) Map转化为JSON

Map的键名要全是字符串 哦

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

6) JSON转化为Map

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes":true,"no":false}')
// Map {'yes' => true, 'no' => false}
```
## Proxy和Reflect

> Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

```javascript
var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(`getting ${key}!`);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(`setting ${key}!`);
    return Reflect.set(target, key, value, receiver);
  }
});
```

上面代码对一个空对象架设了一层拦截，重定义了属性的读取（`get`）和设置（`set`）行为。这里暂时先不解释具体的语法，只看运行结果。对设置了拦截行为的对象`obj`，去读写它的属性，就会得到下面的结果。

```javascript
obj.count = 1
//  setting count!
++obj.count
//  getting count!
//  setting count!
//  2
```



```javascript
var proxy = new Proxy(target, handler);
```

Proxy 对象的所有用法，都是上面这种形式，不同的只是`handler`参数的写法。其中，`new Proxy()`表示生成一个`Proxy`实例，`target`参数表示所要拦截的目标对象,即如果没有`Proxy`的介入，操作原来要访问的就是这个对象，`handler`参数也是一个对象，用来定制拦截行为。

如果`handler`没有设置任何拦截，那就等同于直接通向原对象。

```javascript
var target = {};
var handler = {};
var proxy = new Proxy(target, handler);
proxy.a = 'b';
target.a // "b"
```

上面代码中，`handler`是一个空对象，没有任何拦截效果，访问`handler`就等同于访问`target`。

**技巧**

一个技巧是将Proxy对象，设置到`object.proxy`属性，从而可以在`object`对象上调用。

```javascript
var object = { proxy: new Proxy(target, handler) };
```

Proxy 实例也可以作为其他对象的原型对象。

```javascript
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

let obj = Object.create(proxy);
obj.time // 35
```

上面代码中，`proxy`对象是`obj`对象的原型，`obj`对象本身并没有`time`属性，所以根据原型链，会在`proxy`对象上读取该属性，导致被拦截。

**针对函数的proxy**

同一个拦截器函数，可以设置拦截多个操作。

```javascript
var handler = {
  get: function(target, name) {
    if (name === 'prototype') {
      return Object.prototype;
    }
    return 'Hello, ' + name;
  },

  apply: function(target, thisBinding, args) {
    return args[0];
  },

  construct: function(target, args) {
    return {value: args[1]};
  }
};

var fproxy = new Proxy(function(x, y) {
  return x + y;
}, handler);

fproxy(1, 2) // 1
new fproxy(1,2) // {value: 2}
fproxy.prototype === Object.prototype // true
fproxy.foo // "Hello, foo"
```

### Proxy实例的方法

#### 1. get方法

```javascript
var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError("Property \"" + property + "\" does not exist.");
    }
  }
});

proxy.name // "张三"
proxy.age // 抛出一个错误
```

**实用例子**：实现数组的负数索引

```javascript
function createArray(...elements) {
  let handler = {
    get(target, propKey, receiver) {
      let index = Number(propKey);
      if (index < 0) {
        propKey = String(target.length + index);
      }
      return Reflect.get(target, propKey, receiver);
    }
  };

  let target = [];
  target.push(...elements);
  return new Proxy(target, handler);
}

let arr = createArray('a', 'b', 'c');
arr[-1] // c
```

#### 2. set方法

`set`方法用来拦截某个属性的赋值操作。

假定`Person`对象有一个`age`属性，该属性应该是一个不大于200的整数，那么可以使用`Proxy`保证`age`的属性值符合要求。

```javascript
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于age以外的属性，直接保存
    obj[prop] = value;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```

**实用实例**

有时，我们会在对象上面设置内部属性，属性名的第一个字符使用下划线开头，表示这些属性不应该被外部使用。结合`get`和`set`方法，就可以做到防止这些内部属性被外部读写。

```javascript
var handler = {
  get (target, key) {
    invariant(key, 'get');
    return target[key];
  },
  set (target, key, value) {
    invariant(key, 'set');
    target[key] = value;
    return true;
  }
};
function invariant (key, action) {
  if (key[0] === '_') {
    throw new Error(`Invalid attempt to ${action} private "${key}" property`);
  }
}
var target = {};
var proxy = new Proxy(target, handler);
proxy._prop
// Error: Invalid attempt to get private "_prop" property
proxy._prop = 'c'
// Error: Invalid attempt to set private "_prop" property
```

#### 3.apply 方法

`apply`方法拦截函数的调用、call和apply操作。

```javascript
var handler = {
  apply (target, ctx, args) {
    return Reflect.apply(...arguments);
  }
};
```

下面是另外一个例子。

```javascript
var twice = {
  apply (target, ctx, args) {
    return Reflect.apply(...arguments) * 2;
  }
};
function sum (left, right) {
  return left + right;
};
var proxy = new Proxy(sum, twice);
proxy(1, 2) // 6
proxy.call(null, 5, 6) // 22
proxy.apply(null, [7, 8]) // 30
```

上面代码中，每当执行`proxy`函数（直接调用或`call`和`apply`调用），就会被`apply`方法拦截。

另外，直接调用`Reflect.apply`方法，也会被拦截。

```javascript
Reflect.apply(proxy, null, [9, 10]) // 38
```

#### 4. has 方法

`has`方法用来拦截`HasProperty`操作，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是`in`运算符。

```javascript
var handler = {
  has (target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};
var target = { _prop: 'foo', prop: 'foo' };
var proxy = new Proxy(target, handler);
'_prop' in proxy // false
```

如果原对象的属性名的第一个字符是下划线，`proxy.has`就会返回`false`，从而不会被`in`运算符发现。

`has`拦截只对`in`循环生效，对`for...in`循环不生效，导致不符合要求的属性没有被排除在`for...in`循环之外。

#### 5.construct 方法

`construct`方法是用来拦截`new` 命令的

```javascript
var p = new Proxy(function() {}, {
  construct: function(target, args) {
    console.log('called: ' + args.join(', '));
    return { value: args[0] * 10 };
  }
});

new p(1)
// "called: 1"
// 10
```

`construct`方法返回的必须是一个对象，否则会报错。

### this问题

```javascript
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};

const proxy = new Proxy(target, handler);

target.m() // false
proxy.m()  // true
```

上面代码中，一旦`proxy`代理`target.m`，后者内部的`this`就是指向`proxy`，而不是`target`。
## Iterator和for...of循环

**Iterator 的概念**

JavaScript原有的表示“集合”的数据结构，主要是数组（Array）和对象（Object），ES6又添加了Map和Set。在ES6中，有些数据结构原生具备Iterator接口（比如数组），即不用任何处理，就可以被`for...of`循环遍历，有些就不行（比如对象）。原因在于，这些数据结构原生部署了`Symbol.iterator`属性

### 数据结构默认的Iterator接口

一种数据结构只要部署了Iterator接口，我们就称这种数据结构是”可遍历的“（iterable）。

ES6规定，默认的Iterator接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）

```javascript
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```

上面代码中，对象`obj`是可遍历的（iterable），因为具有`Symbol.iterator`属性。执行这个属性，会返回一个遍历器对象。该对象的根本特征就是具有`next`方法。每次调用`next`方法，都会返回一个代表当前成员的信息对象，具有`value`和`done`两个属性。

在ES6中，有三类数据结构原生具备Iterator接口：数组、某些类似数组的对象、Set和Map结构。

```javascript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

一个对象如果要有可被`for...of`循环调用的Iterator接口，就必须在`Symbol.iterator`的属性上部署遍历器生成方法（原型链上的对象具有该方法也可）。

```javascript
class RangeIterator {
  constructor(start, stop) {
    this.value = start;
    this.stop = stop;
  }

  [Symbol.iterator]() { return this; }

  next() {
    var value = this.value;
    if (value < this.stop) {
      this.value++;
      return {done: false, value: value};
    } else {
      return {done: true, value: undefined};
    }
  }
}

function range(start, stop) {
  return new RangeIterator(start, stop);
}

for (var value of range(0, 3)) {
  console.log(value);
}
```

上面代码是一个类部署Iterator接口的写法。`Symbol.iterator`属性对应一个函数，执行后返回当前对象的遍历器对象。

下面是通过遍历器实现指针结构的例子。

```javascript
function Obj(value) {
  this.value = value;
  this.next = null;
}

Obj.prototype[Symbol.iterator] = function() {
  var iterator = {
    next: next
  };

  var current = this;

  function next() {
    if (current) {
      var value = current.value;
      current = current.next;
      return {
        done: false,
        value: value
      };
    } else {
      return {
        done: true
      };
    }
  }
  return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next = two;
two.next = three;

for (var i of one){
  console.log(i);
}
// 1
// 2
// 3
```

上面代码首先在构造函数的原型链上部署`Symbol.iterator`方法，调用该方法会返回遍历器对象`iterator`，调用该对象的`next`方法，在返回一个值的同时，自动将内部指针移到下一个实例。

下面是另一个为对象添加Iterator接口的例子。

```javascript
let obj = {
  data: [ 'hello', 'world' ],
  [Symbol.iterator]() {
    const self = this;
    let index = 0;
    return {
      next() {
        if (index < self.data.length) {
          return {
            value: self.data[index++],
            done: false
          };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};
```

对于类似数组的对象（存在数值键名和length属性），部署Iterator接口，有一个简便方法，就是`Symbol.iterator`方法直接引用数组的Iterator接口。

### 调用Iterator接口的场合

有一些场合会默认调用Iterator接口（即`Symbol.iterator`方法），除了下文会介绍的`for...of`循环，还有几个别的场合。

**（1）解构赋值**

对数组和Set结构进行解构赋值时，会默认调用`Symbol.iterator`方法。

```javascript
let set = new Set().add('a').add('b').add('c');

let [x,y] = set;
// x='a'; y='b'

let [first, ...rest] = set;
// first='a'; rest=['b','c'];
```

**（2）扩展运算符**

扩展运算符（...）也会调用默认的iterator接口。

```javascript
// 例一
var str = 'hello';
[...str] //  ['h','e','l','l','o']

// 例二
let arr = ['b', 'c'];
['a', ...arr, 'd']
// ['a', 'b', 'c', 'd']
```

上面代码的扩展运算符内部就调用Iterator接口。

实际上，这提供了一种简便机制，可以将任何部署了Iterator接口的数据结构，转为数组。也就是说，只要某个数据结构部署了Iterator接口，就可以对它使用扩展运算符，将其转为数组。

```javascript
let arr = [...iterable];
```

**（3）yield\***

yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。

```javascript
let generator = function* () {
  yield 1;
  yield* [2,3,4];
  yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```

**（4）其他场合**

由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。下面是一些例子。

- for...of
- Array.from()
- Map(), Set(), WeakMap(), WeakSet()（比如`new Map([['a',1],['b',2]])`）
- Promise.all()
- Promise.race()

### 拓展

在原生的有遍历接口的数据结构上可以直接覆盖修改

```javascript
var str = new String("hi");

[...str] // ["h", "i"]

str[Symbol.iterator] = function() {
  return {
    next: function() {
      if (this._first) {
        this._first = false;
        return { value: "bye", done: false };
      } else {
        return { done: true };
      }
    },
    _first: true
  };
};

[...str] // ["bye"]
str // "hi"
```

上面代码中，字符串str的`Symbol.iterator`方法被修改了，所以扩展运算符（`...`）返回的值变成了`bye`，而字符串本身还是`hi`。

### Iterator接口和Generation函数

`Symbol.iterator`方法的最简单实现，还是使用下一章要介绍的Generator函数。

```javascript
var myIterable = {};

myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};
[...myIterable] // [1, 2, 3]

// 或者采用下面的简洁写法

let obj = {
  * [Symbol.iterator]() {
    yield 'hello';
    yield 'world';
  }
};

for (let x of obj) {
  console.log(x);
}
// hello
// world
```

数组原生具备`iterator`接口（即默认部署了`Symbol.iterator`属性），`for...of`循环本质上就是调用这个接口产生的遍历器，可以用下面的代码证明。

```javascript
const arr = ['red', 'green', 'blue'];

for(let v of arr) {
  console.log(v); // red green blue
}

const obj = {};
obj[Symbol.iterator] = arr[Symbol.iterator].bind(arr);

for(let v of obj) {
  console.log(v); // red green blue
}
```

上面代码中，空对象`obj`部署了数组`arr`的`Symbol.iterator`属性，结果`obj`的`for...of`循环，产生了与`arr`完全一样的结果。

## Generator 函数

> 和Python中的生成器理解相同，执行Generator函数会返回一个遍历器对象，用到yield，下面就是一个标准的Generator函数。**Generator函数就是遍历器生成函数**

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```

```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

### yield语句

由于Generator函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield`语句就是暂停标志。

yield* 使用方法

```javascript
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  var length = a.length;
  for (var i = 0; i < length; i++) {
    var item = a[i];
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  }
};

for (var f of flat(arr)) {
  console.log(f);
}
// 1, 2, 3, 4, 5, 6
```

### next 函数参数

```javascript
function* f() {
  for(var i=0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; }
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

上面代码先定义了一个可以无限运行的Generator函数`f`，如果`next`方法没有参数，每次运行到`yield`语句，变量`reset`的值总是`undefined`。当`next`方法带一个参数`true`时，当前的变量`reset`就被重置为这个参数（即`true`），因此`i`会等于-1，下一轮循环就会从-1开始递增。

再看一个例子。这个看懂，Generator函数的next函数基本就过了

```javascript
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

注意，由于`next`方法的参数表示上一个`yield`语句的返回值，所以第一次使用`next`方法时，不能带有参数。V8引擎直接忽略第一次使用`next`方法时的参数，只有从第二次使用`next`方法开始，参数才是有效的。从语义上讲，第一个`next`方法用来启动遍历器对象，所以不用带有参数。

**注意**： Generator函数里面除了yield，还有return、throw。这里介绍return，return是跳出遍历器，return的值不在循环里。如下

```javascript
function *foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

### 例子

下面是一个利用Generator函数和`for...of`循环，实现斐波那契数列的例子。

```javascript
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    [prev, curr] = [curr, prev + curr];
    yield curr;
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```

对象添加遍历器

```javascript
function* objectEntries(obj) {
  let propKeys = Reflect.ownKeys(obj);  //或者Object.keys 获取Object中的key

  for (let propKey of propKeys) {
    yield [propKey, obj[propKey]];
  }
}

let jane = { first: 'Jane', last: 'Doe' };	

for (let [key, value] of objectEntries(jane)) {
  console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe
```

### yield* 语句

从语法角度看，如果`yield`命令后面跟的是一个遍历器对象，需要在`yield`命令后面加上星号，表明它返回的是一个遍历器对象。这被称为`yield*`语句。

如果在Generater函数内部，调用另一个Generator函数，默认情况下是没有效果的。

```javascript
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  foo();
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "y"
```

上面代码中，`foo`和`bar`都是Generator函数，在`bar`里面调用`foo`，是不会有效果的。

这个就需要用到`yield*`语句，用来在一个Generator函数里面执行另一个Generator函数。

```javascript
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```

实际上，任何数据结构只要有Iterator接口，就可以被`yield*`遍历。

```javascript
function* inner() {
  yield 'hello!';
}

function* outer1() {
  yield 'open';
  yield* [1,2,];
  yield 'close';
}
i = outer1()
outer1 {<suspended>}
i.next()
{value: "open", done: false}
i.next()
{value: 1, done: false}
i.next()
{value: 2, done: false}
i.next()
{value: "close", done: false}
i.next()
{value: undefined, done: true}
```

如果被代理的Generator函数有`return`语句，那么就可以向代理它的Generator函数返回数据。

```javascript
function *foo() {
  yield 2;
  yield 3;
  return "foo";
}

function *bar() {
  yield 1;
  var v = yield *foo();
  console.log( "v: " + v );
  yield 4;
}

var it = bar();

it.next()
// {value: 1, done: false}
it.next()
// {value: 2, done: false}
it.next()
// {value: 3, done: false}
it.next();
// "v: foo"
// {value: 4, done: false}
it.next()
// {value: undefined, done: true}
```

### 好例子

下面是一个稍微复杂的例子，使用`yield*`语句遍历完全二叉树。

```javascript
// 下面是二叉树的构造函数，
// 三个参数分别是左树、当前节点和右树
function Tree(left, label, right) {
  this.left = left;
  this.label = label;
  this.right = right;
}

// 下面是中序（inorder）遍历函数。
// 由于返回的是一个遍历器，所以要用generator函数。
// 函数体内采用递归算法，所以左树和右树要用yield*遍历
function* inorder(t) {
  if (t) {
    yield* inorder(t.left);
    yield t.label;
    yield* inorder(t.right);
  }
}

// 下面生成二叉树
function make(array) {
  // 判断是否为叶节点
  if (array.length == 1) return new Tree(null, array[0], null);
  return new Tree(make(array[0]), array[1], make(array[2]));
}
let tree = make([[['a'], 'b', ['c']], 'd', [['e'], 'f', ['g']]]);

// 遍历二叉树
var result = [];
for (let node of inorder(tree)) {
  result.push(node);
}

result
// ['a', 'b', 'c', 'd', 'e', 'f', 'g']
```

## ES6 Promise对象

> 异步编程的一种解决方案
>
> 所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。

### 基本用法

ES6规定，Promise对象是一个构造函数，用来生成Promise实例。

下面代码创造了一个Promise实例。

```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise实例生成以后，可以用`then`方法分别指定`Resolved`状态和`Reject`状态的回调函数。

```javascript
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

下面是一个用Promise对象实现的Ajax操作的例子。

```javascript
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

上面代码中，`getJSON`是对XMLHttpRequest对象的封装，用于发出一个针对JSON数据的HTTP请求，并且返回一个Promise对象。需要注意的是，在`getJSON`内部，`resolve`函数和`reject`函数调用时，都带有参数。