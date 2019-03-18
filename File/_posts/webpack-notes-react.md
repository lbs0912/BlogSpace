
---
title: Webpack Notes-2-React环境配置-模块和插件
date: 2017-08-29 14:35:48
categories: Front-end
tags: [Front-end,Webpack]
keywords: Front-end,Webpack
---




* 本文主要记录webpack的模块和插件,并对webpack中配置React开发环境进行了介绍。
* [初学webpack与react](https://segmentfault.com/a/1190000003768578#articleHeader11)   ！！！
* [webpack | Segmentfault](https://segmentfault.com/a/1190000006178770)！！！
* [Webpack for React](http://www.pro-react.com/materials/appendixA/)
* [Webpack Official Website](https://webpack.js.org/)
* [Webpack GitHub](https://webpack.github.io/)
* Run `webpack --display-error-details` to show errors' detail information.


<!--more-->



## 模块
Webpack允许使用不同的模块类型，但是`底层`必须使用同一种实现。所有的模块可以直接在盒外运行。

* ES6 模块

```
import MyModule from './MyModule.js';
```

* CommonJS

```
var MyModule = require('./MyModule.js');
```

* AMD

```
define(['./MyModule.js'], function (MyModule) {
});
```



## webpack高级功能

* [webpack | Segmentfault](https://segmentfault.com/a/1190000006178770)！！！

### Source Maps - 使调试更容易
通过简单的配置，webpack就可以在打包时生成`source maps`。这为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。

在webpack的配置文件中配置source maps，需要配置`devtool`。

在`webpack.config.js`文件中进行如下配置

```
module.exports = {
  devtool: 'eval-source-map'   //中小型项目
  //...
}

```

### 构建本地服务器

安装`webpack-dev-server`服务器，可以实现浏览器监听你的代码的修改，并自动刷新显示修改后的结果。
```
npm install --save-dev webpack-dev-server
```

在`webpack.config.js`文件中进行如下配置

```
//webpack.config.js
devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    historyApiFallback: true,//不跳转
   	inline: true//实时刷新
}
```

在`package.json`中的`scripts`对象中添加如下命令，用以开启本地服务器

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open"
},
```

在终端中输入`npm run server`即可在本地的`8080`端口查看结果。

## Loaders
通过使用不同的`loader`，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件。对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。


Loaders需要单独安装

**Loaders需要在`webpack.config.js`中的`modules`关键字下进行配置**。Loaders的配置包括以下几方面
* `test`：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
* `loader`：loader的名称（必须）
* `include/exclude`:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
* `query`：为loaders提供额外的设置选项（可选）



## webpack中配置React开发环境

* [初学webpack与react](https://segmentfault.com/a/1190000003768578#articleHeader11) !!!
* [Webpack 配置 React 开发环境](https://hulufei.gitbooks.io/react-tutorial/webpack.html)
* [webpack | Segmentfault](https://segmentfault.com/a/1190000006178770)的`Babel`章节


### React React-DOM

```
npm install --save react react-dom
```

### Babel

Babel其实是几个模块化的包，其核心功能位于`babel-core`的`npm`包中，webpack可以把其不同的包整合在一起使用，对于每一个你需要的功能或拓展，你都需要安装单独的包（用得最多的是解析Es6的`babel-preset-es2015`包和解析JSX的`babel-preset-react`包）。

```
//安装依赖包
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
```

在`webpack.config.js`文件的`module`关键字下进行如下配置。

```
//webpack.config.js
module: {
    rules: [
        {
            test: /(\.jsx|\.js)$/,
            use: {
                loader: "babel-loader",
                options: {
                    presets: [
                        "es2015", "react"
                    ]
                }
            },
            exclude: /node_modules/
        }
    ]
}
```




## 模块
*  Babel
*  css-loader
*  style-loader 
*  Less Loader ： CSS预处理
*  Sass Loader
*  Stylus Loade



## 插件 Plugins
插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。

Loaders和Plugins常常被弄混，但是它们其实是完全不同的东西。
* **loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个**
* **插件并不直接操作单个文件，它直接对整个构建过程其作用。**

### 插件使用方法
* 第1步：通过npm安装插件
* 第2步：在`webpack.config.js`文件的`plugins`关键字部分添加该插件的一个实例

```
plugins: [
    //添加一个版权声明插件
    //通过这个插件，打包后的JS文件显示“版权所有，翻版必究”的字样
    new webpack.BannerPlugin('版权所有，翻版必究')
],
```




## Reference
[1] [Getting started webpack tutorial ](https://webpack.github.io/docs/tutorials/getting-started/)

[2] [Webpack List of Tutorials](https://webpack.github.io/docs/list-of-tutorials.html) 

[3] [webpack 教程](https://www.zfanw.com/blog/webpack-tutorial.html)

[4] [初学webpack与react](https://segmentfault.com/a/1190000003768578)

[5] [Webpack中文指南](http://zhaoda.net/webpack-handbook/index.html) 

[6] [webpack-dev-server使用方法](https://segmentfault.com/a/1190000006670084)
