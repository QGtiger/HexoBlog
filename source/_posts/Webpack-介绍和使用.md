title: Webpack 介绍和使用
author: Light Fish
tags:
  - webpack
categories:
  - webpack
date: 2019-05-19 14:19:00
---
# Webpack的简单介绍

>自从上次搭建了一个博客系统后也是对接下来的改进有点迷茫，所以就开始学习一下前端技术——Vue。至于Vue的一些组件的介绍就不说了，就简单说说前端工程化也就是做项目所用到的技术——Webpack。

<!-- more -->


## webpack简单原理

>前端的工作现在也已经不是简单的几个样式，几张图的事情了，项目比较大的到时候可能会多人合作。模块化、组件化、CSS预编译等概念也成为了经常讨论的话题。以前也是简单的了解过Gulp，只是实现js代码的合并和压缩，压缩的代码仍然是你写的代码，只是一些语法做了转换。而这次要介绍的前端工程化技术——Webpack，打包后的代码已经不是你写的代码了，夹杂着webpack自身的模块化处理代码。

![img](http://qnpic.top/webpack%5C1.jpg)

>上图是webpack的经典webpack模块化示意图，左边是业务上的各种格式化文件。这些文件需要通过加载器(loader)编译后，最后统一称为js、css、.png等静态资源。所以webpack就是来处理模块间的依赖关系，并把他们进行打包。

* 举例

>在webpack里直接在js文件中引入所需文件如带入css，`import 'src/styles/index.css'`。在打包后就会动态创建style标签并加载css样式。

### 应用场景

>webpack主要的应用场景是单页面富应用(SPA),SPA通常是一个html文件和一堆按需加载的js文件，他的结构很简单。都是后期动态加载的。你可能会问为什么要单页面，因为单页面是不需要刷新的。比如网易云音乐，如果你单击跳转刷新页面，你的当前播放音乐就会也刷新掉，这个就是很经典的应用场景哦~

## webpack 基础配置

>在学习webpack前你一定要去了解export和import的用法，这里就不介绍了。

### 安装webpack与webpack-dev-server

>1.首先你要确保安装了Node.js和npm，这里也不介绍

>2.创建一个目录，比如demo，使用npm初始化配置

```
npm init
```

>3.执行后会出现一系列选项，你可以回车键确认，完成后会生成一个package.json文件。

![img](http://qnpic.top/webpack%5C2.jpg)

>4.本地局部安装webpack

```
npm install webpack --save-dev
```

* --save-dev 会作为开发依赖来安装webpack，安装成功后package.json会多出一行配置

![img](http://qnpic.top/webpack%5C3.jpg)

>5.接着安装webpack-dev-server，他可以在开发环境中提供很多服务，如启动一个服务器，热更新、接口代理等。同样局部安装

```
npm install webpack-dev-server --save-dev
```

最后package.json文件

```
{
  "name": "demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.31.0",
    "webpack-dev-server": "^3.3.1"
  }
}

```

>6.至此你就很快可以开启webpack工程了

## 就是一个js文件而已

>归根到底，webpack就是一个`.js`的配置文件，首先在demo目录下床架一个js文件，`webpack.config.js`,初始化他的内容

```
var config = {


};

module.exports = config;
```

* **注意**，这里的module.exports = config,相当于export default config；由于没有安装ES6的编译插件所以不能直接使用ES6的语法

>然后在package.json里增加一个快速启动webpack-dev-server服务的脚本

```
//...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --config webpack.config.js"
  },
//...
```

>当运行`npm run dev`就是运行`webpack-dev-server --open --config webpack.config.js`，--config就是指向webpack-dev-server读取的配置文件。--open就会自动在浏览器中打开服务，默认127.0.0.1:8080，可以配置：

```
webpack-dev-server --host 172.172.172.1 --port 8000 --open --config webpack.config.js
```

>**主要：**webpack配置最重要的也是必选的就是入口(Entry)和出口(Output)，入口就是告诉webpack从哪里找依赖并编译。出口就是编译后的文件保存目录。

>所以在demo下创建main.js文件作为入口文件，然后在webpack.config.js文件中进行配置

```
var path = require('path');

var config = {
    entry: {
        main: './main'
    },
    output: {
        path: path.join(__dirname, './dist'),
        publicPath: '/dist/',
        filename: 'main.js'
    }
};

module.exports = config;
```

>`entry`里的mian就是我们配置的单入口，webpack会从mian.js文件开始工作，`output`path是存放打包的输出目录，是必填项。publicPath置顶资源文件的目录。filename就是输出文件名。

>在demo下创建index.html作为SPA的入口：

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Webpack App</title>
    </head>
    <div id="app">
        Hello World
    </div>
    <script type="text/javascript" src="/dist/main.js"></script>
</html>
```

* **<font style="color:red">注意</font>**：你现在就可以运行`npm run dev`，但是如果你遇到下面这张错误,有可能是package.json文件中的webpack与webpack-dev-server版本不对应的问题，我亲测有效，你可以修改,然后重新`npm install`就行了

```
  "devDependencies": {
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  }
```

![img](http://qnpic.top/webpack%5C4.jpg)

>运行结果

![img](http://qnpic.top/webpack%5C5.jpg)

>修改main.js文件，保存后页面机会动态修改，并没有刷新浏览器，自动更新，这就是webpack-dev-server的热更新。通过创建websocket连接实时响应

![gif](http://qnpic.top/webpack%5CGIF1.gif)

### 理解编译

>我们可以在开发者下看看webpack编译出的/dist/main.js是什么在network中可以看到确实是dist下的main.js

![img](http://qnpic.top/webpack%5C6.jpg)

>在资源下看到main.js有9000多行,里面都是webpack-dev-server的功能，开发有效

![img](http://qnpic.top/webpack%5C7.jpg)

## 逐步完善配置文件

>上一部分是通过entry和ouput已经启动了webpack项目。这里就深入了解。在webpack的世界里，每一个文件都是一个模块，比如.css、.js、.html、.jpg、.less等。对于不同的模块需要用到不同的加载器（loader）来处理，而加载器就是webpack的最重要的功能，通过不同的加载器可以对不同文件进行处理。比如要写一些CSS样式就需要style-loader和css-loader。可以通过npm安装

```
npm install css-loader --save-dev
npm install style-loader --save-dev
```

>然后在`webpack.config.js`中配置loader，增加对css的处理：

```
//...
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
//...
```

> **<font style="color:red">注意</font>**：在module对象的rules属性中可以指定一系列的loader，每一个loader必须包含`test`和`use`两个选项。这个配置意思是，当webpack编译过程中遇到require()和import()语句导入一个后缀名为css文件就是先通过`css-loader`，再通过`style-loader`，然后继续打包。编译顺序从后往前。

>在demo下创建style.css文件

```
#app{
    font-size: 24px;
    color: red;
}
```

>然后在main.js中导入

```
import './style.css';

document.getElementById('app').innerHTML = 'Hello Webpack';
```

>运行结果可以看到css样式动态创建style标签来导入的。但是都放在js里太占体积了。这个时候就是用到最后一个重要概念——插件(Plugin)

## 插件

>webpack的插件很抢到还可以定制。这里我们使用一个`extract-text-webpack-plugin`的插件把散落的css提取出来然后生成main.css，最终在index.html中通过link导入

```
npm install extract-text-webpack-plugin --save-dev
```

>然后在配置文件中修改

```
var path = require('path');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

var config = {
	//...
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ExtractTextPlugin.extract({
                    use: 'css-loader',
                    fallback: 'style-loader'
                })
            }
        ]
    },
    plugins: [
        // 重命名提取后的css文件
        new ExtractTextPlugin('main.css')
    ]
};

module.exports = config;
```

>在index.html中也是需要修改，添加link标签

![img](http://qnpic.top/webpack%5C9.jpg)

><font style="color: red;font-weight:700">总结:</font>webpack虽然概念新，看似很复杂其实他也是不过一个js的配置文件，只需要搞定`入口(entry)`、`出口(ouput)`、`加载器(loader)`和`插件(plugin)`就行了。简单的webpack项目也是如下目录

![img](http://qnpic.top/webpack%5C10.jpg)

## 单文件组件和vue-loader

>这次也是简单运用下webpack，毕竟上面也是简单讲解了webpack的工作流程，如果你想使用它你得有实践经验。比如我们在使用vue组件的时候，我们可以使用.vue文件，这样更加清晰使用，一般的.vue文件包含3部分，`<template>`,`<script>`和`<style>`。要webpack认识这个.vue文件，就需要配置加载器——vue-loader。

>在使用.vue需要安装如下依赖。因为要使用ES6语法，还需要安装babel和babel-loader等加载器

```
npm install --save vue
npm install --save-dev vue-loader
npm install --save-dev vue-style-loader
npm install --save-dev vue-template-compiler
npm install --save-dev vue-hot-reload-api
npm install --save-dev babel
npm install --save-dev babel-loader
npm install --save-dev babel-core
npm install --save-dev babel-plugin-transform-runtime
npm install --save-dev babel-preset-es2015
npm install --save-dev babel-runtime
```

>安装完了后，修改webpack.config.js配置文件来支持.vue和ES6的解析

```
var path = require('path');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

var config = {
    entry: {
        main: './main'
    },
    output: {
        path: path.join(__dirname, './dist'),
        publicPath: '/dist/',
        filename: 'main.js'
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader',
                options: {
                    loaders: {
                        css: ExtractTextPlugin.extract({
                            use: 'css-loader',
                            fallback: 'vue-style-loader'
                        })
                    }
                }
            },
            {
                test: /\.js$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: ExtractTextPlugin.extract({
                    use: 'css-loader',
                    fallback: 'style-loader'
                })
            }
        ]
    },
    plugins: [
        // 重命名提取后的css文件
        new ExtractTextPlugin('main.css')
    ]
};

module.exports = config;
```

>vue-loader在编译.vue文件时，会对template和style，script分别处理，所以在vue-loader选项里多了options选项来进一步对不同语言进行配置。比如对css进行处理时，先通过css-loader来解析，然后把解析结果给vue-style-loader来处理。放你的技术多样化的时候，可以指定不同的语言，如`<template lange="jade">`和`<style lange="less">`然后配置loaders就行了

>在demo下创建`.babelrc`文件，写入babel配置，webpack会依赖此配置文件来使用babel来编译ES6代码

```
{
    "presets": ["es2015"],
    "plugins": ["transform-runtime"],
    "comments": false
}
```

>至此就可以使用.vue文件，在demo创建app.vue写入

```
<template>
    <div>Hello {{name}}</div>
</template>   

<script>
export default {
    data () {
        return {
            name: 'Lightfish'
        }
    }
}
</script>

<style scoped>
    div{
        color: #f60;
        font-size: 24px;
        font-weight: 700
    }
</style>
```

>加了scoped，只有在使用app.vue才会加载样式。然后在main.js入口文件修改

```
import Vue from 'vue';
import App from './app.vue';


//创建Vue实例
new Vue({
    el: '#app',
    render: h => h(App)
})
```

>运行就能看到界面已经动态修改，但是其实我这里全部重新配置运行的时候时出错的，因为这里面也是上面的那个小问题，配置依赖之间还得相互依赖版本，所以，得去好好分清，如果你直接npm install是安装最新版本的，可能会不兼容，你可以看看一下依赖版本

```
  "devDependencies": {
    "babel": "^6.23.0",
    "babel-core": "^6.24.0",
    "babel-loader": "^6.4.1",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-es2015": "^6.24.0",
    "babel-runtime": "^6.23.0",
    "css-loader": "^0.27.3",
    "extract-text-webpack-plugin": "^2.1.0",
    "file-loader": "^0.10.1",
    "html-webpack-plugin": "^2.28.0",
    "style-loader": "^0.16.1",
    "url-loader": "^0.5.8",
    "vue-hot-reload-api": "^2.0.11",
    "vue-loader": "^11.3.4",
    "vue-style-loader": "^2.0.5",
    "vue-template-compiler": "^2.2.6",
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  },
```

## vue组件相互依赖

>在创建`title.vue`和`button.vue`

title.vue
```
<template>
    <h1>
        <a :href="'#'+title">{{ title }}</a>
    </h1>
</template>
<script>
export default {
    props: {
        title: {
            type: String,
            default: 'title'
        }
    }
}
</script>
<style scoped>
    h1 a{
        color: #3399ff;
        font-size: 24px
    }
</style>

```

button.vue
```
<template>
    <button @click="handleClick" :style="styles">
        <slot></slot>
    </button>
</template>
<script>
export default {
    props: {
        color: {
            type: String,
            default: '#00cc66'
        }
    },
    computed: {
        styles () {
            return {
                background: this.color
            }
        }
    },
    methods: {
        handleClick (e){
            this.emit('click', e);
        }
    }
}
</script>
<style scoped>
    button{
        border:0;
        outline: none;
        color: #fff;
        padding: 4px 8px;
    }
    button:active{
        position: relative;
        top: 1px;
        left: 1px;
    }
</style>
```

>然后在修改根实例app.vue组件

```
<template>
    <div>
        <v-title :title="title"></v-title>
        <v-button @click="handleClick" :color="color">点击按钮</v-button>
    </div>
</template>   

<script>
import vTitle from './title.vue'
import vButton from './button.vue'
export default {
    components: {
        vTitle,
        vButton
    },
    data () {
        return {
            title: "vue 组件",
            color: "red"
        }
    },
    methods: {
        handleClick (e) {
            console.log(e);
        }
    }
}
</script>

<style scoped>
</style>
```

>记得引入组件哦~~

## 用于生产环境

>安装url-loader和file-loader来支持图片和字体等文件

>修改配置文件

```
//...
    module: {
        rules: [
            //...
            {
                test: /\.(gif|jpg|png|woff|svg|eot|ttf)\??.*$/,
                loader: 'url-loader?limit=1024'
            }
				//...
};

module.exports = config;
```

>当遇到gif、png和ttf等文件时，url-loader会把它们一起编译到dist目录下，‘？limit=1024’是指如果这个文件小于1kb，就以base64加载

* 照一张图片放在images目录下，然后在app.vue中加载

```
<template>
    <div>
        <v-title :title="title"></v-title>
        <v-button @click="handleClick" :color="color">点击按钮</v-button>
        <p>
            <img src="./images/root.jpg" alt="" style="width:200px">
        </p>
    </div>
</template>   

```

效果图
<br>
![img](http://qnpic.top/webpack%5C11.jpg)

### 打包

>在生产环境中需要打包成一个SPA，所以需要安装以下两个模块

```
npm install --save-dev webpack-merge
npm install --save-dev html-webpack-plugin
```

>为了方便开发和生产环境的切换，在demo下再创建一个webpack.prod.config.js,编译打包，在package.json中加入build打包命令

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --config webpack.config.js",
    "build": "webpack --progress --hide-modules --config webpack.prod.config.js"
  },
```

>webpack.prod.config.js代码：

```
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var merge = require('webpack-merge');
var webpackBaseConfig = require('./webpack.config.js');

// 清空基本配置的插件列表
webpackBaseConfig.plugins = [];

module.exports = merge(webpackBaseConfig, {
    output: {
        publicPath: '/dist/',
        // 将入口文件重命名为带有 20 位 hash 值的唯一文件
        filename: '[name].[hash].js'
    },
    plugins: [
        new ExtractTextPlugin({
            // 提取 css，并重命名为带有 20 位 hash 值的唯一文件
            filename: '[name].[hash].css',
            allChunks: true
        }),
        // 定义当前 node 环境为生产环境
        new webpack.DefinePlugin({
            'process.env': {
                NODE_ENV: '"production"'
            }
        }),
        // 压缩 js
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            }
        }),
        // 提取模板，并保存入口 html 文件
        new HtmlWebpackPlugin({
            filename: '../index_prod.html',
            template: './index.ejs',
            inject: false
        })
    ]
});
```

>上述安装的webpack-merge模块就是用于合并两个webpack的配置文件，所以prod的配置是在webpack.config.js的基础上拓展的。打包后的文件也是加了20位的hash值，是唯一的，第一时间加载静态文件

>html-wenpack-plugin是用来生成html文件的，它通过template选项指定模板index.ejs然后输出到filename指定的目录下，也就是demo/index_prod.html,index.ejs代码如下

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Webpack App</title>
    <link rel="stylesheet" href="<%= htmlWebpackPlugin.files.css[0] %>">
</head>
<body>
    <div id="app"></div>
    <script type="text/javascript" src="<%= htmlWebpackPlugin.files.js[0] %>"></script>
</body>
</html> 
```

>运行`npm run build`就会在demo下生成一个dist目录，里面就是静态资源,想这些静态文件的运用也是简单，只需要index_prod.html和dist目录下的静态文件，放到服务器上就行了。

<br><br><br>So,这次的博客也是简单的对webpack的一些基础知识进行了讲解，对webpack有了更深的了解，So，Just have fun...