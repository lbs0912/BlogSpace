---
title: Webpack Notes-1-安装和初步使用
date: 2017-08-28 14:35:48
categories: Front-end
tags: [Front-end,Webpack]
keywords: Front-end,Webpack
---




* 本文主要记录webpack的安装和初步使用。
* [Webpack Official Website](https://webpack.js.org/)
* [Webpack GitHub](https://webpack.github.io/)
* [webpack | Segmentfault](https://segmentfault.com/a/1190000006178770)
* Run `webpack --display-error-details` to show errors' detail information.


<!--more-->

## Geting started
Read [Getting started webpack tutorial ](https://webpack.github.io/docs/tutorials/getting-started/) to know the basic concept and usage of webpack.



## webpack优点
1. webpack 是以 commonJS 的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。

2. 能被模块化的不仅仅是JS了。

3. 开发便捷，能替代部分grunt/gulp的工作，比如打包、压缩混淆、图片转base64等。

4. 扩展性强，插件机制完善，特别是支持**React热插拔**（见 [react-hot-loader](https://github.com/gaearon/react-hot-loader) ）的功能让人眼前一亮。

## webpack VS gulp grunt
WebPack可以看做是**模块打包机**。webpack可以分析项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其**转换**和**打包**为合适的格式供浏览器使用。


其实，Webpack和gulp，grunt并没有太多的可比性。

**Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。**

**对比发现，Webpack的处理速度更快更直接，能打包更多不同类型的文件。**

* Grunt和Gulp的工作方式

在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，工具之后可以自动替你完成这些任务。


![grunt-gulp-1.png](http://ojx8u3g1z.bkt.clouddn.com/grunt-gulp-1.png)


* Webpack的工作方式

把项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用`loaders`处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

![webpack-usage-1.png](http://ojx8u3g1z.bkt.clouddn.com/webpack-usage-1.png)

## Install webpack
Change to your project directory and run the following command. 
```
npm init   # or  npm init -y 
npm install webpack --save-dev
npm install css-loader --save-dev
npm install style-loader --save-dev
```
> Webpack can only handle JavaScript natively, so we need the `css-loader` to process CSS files. We also need the `style-loader` to apply the styles in the CSS file.
> 
> Run `npm install css-loader style-loader` to install the loaders (They need to be installed locally, without `-g`)
>                 --- [Getting started webpack tutorial](https://webpack.github.io/docs/tutorials/getting-started/)

##  Create the project directory
```
WebpackApp
|__ dist
|	|__ styles
|   |__ js
|		|__ bundle.js 
|	|__ index.html
|__ src
|	|__ styles
|	|__ js
|		|__ index.js(OR entry.js)
|       |__ component.js
|__ node_modules
|__ package.json
|__ webpack.config.js
```
The `bundle.js` file is created automatically.

## Webpack Command
### Command List
```cmake
webpack
webpack src.js target.js
# Some environments may require double quotes: –module-bind "css=style!css"
webpack src.js target.js --module-bind 'css=style!css'
webpack --progress --colors 

webpack --progress --colors --watch 

webpack-dev-server
webpack-dev-server --progress --colors --open
webpack-dev-server --progress --colors --inline --open
```
### Pack CSS files
*  `webpack src.js target.js --module-bind 'css=style!css'`

If you want to pack CSS files in your application, you should install `css-loader` and `style-loader`. And then import CSS files in your JS files.

> webpack can only handle JavaScript natively, so we need the `css-loader` to process CSS files. We also need the `style-loader` to apply the styles in the CSS file.


```
npm install css-loader --save-dev
npm install style-loader --save-dev
```
```
// index.js
require("!style!css!./style.css");

//之后运行 webpack src.js target.js 进行打包

//在webpack2.0以上版本运行上述代码，会出现如下错误
/*
It's no longer allowed to omit the '-loader' suffix when using loaders.

You need to specify 'css-loader' instead of 'css',

see https://webpack.js.org/guides/migrating/#automatic-loader-module-name-extension-removed
@ ./entry.js 1:0-40
*/
```

**需要注意的是，webpack2.0以上版本，不再支持`!style!css!`这种缩写方式，需要将其写全**

```
require("!style-loader!css-loader!./style.css");

//之后运行 webpack src.js target.js 进行打包
```


If you don't want to write such a long require command `require("!style!css!./style.css");`,  you can bind file extensions to loaders so you just need to write
```
//index.js
require("./style.css")
```
Run the compilation with
```
//webpack2.0版本之前
webpack src.js target.js --module-bind 'css=style!css' 

//webpack2.0版本之后
//[Loader|GitBook](http://zhaoda.net/webpack-handbook/loader.html)
webpack src.js target.js --module-bind 'css=style-loader!css-loader'
//windows10 x64 系统下需要使用双引号
webpack src.js target.js --module-bind "css=style-loader!css-loader"
```
> Some environments may require double quotes: –module-bind “css=style!css”

### webpack.config.js
*  `webpack` & `webpack-dev-server`

You can directly run these command if you set your webpack configuration in `webpack.config.js` file.


手动创建`webpack.config.js`文件。

```
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            //2.0版本前
            { test: /\.css$/, loader: "style!css" } 
            //2.0版本后
            { test: /\.css$/, loader: "style-loader!css-loader" }  
        ]
    }
};
```

### webpack options
* `webpack-dev-server --progress --colors --open`

`--progress` can display some kind of  compilation progress bar. `--colors` can use different colors to show the information. `--open` can automatically open `localhost:8080/webpack-dev-server` in your default browser.
* `webpack-dev-server --progress --colors --inline --open`

`--inline` indicates using `inline mode` of `web-dev-server`. `--open` can automatically open `localhost:8080` in your default browser.

### npm run build
```
//package.json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"   
    // add 
    // JSON文件不支持注释，实际添加时候去掉该注释
  },
```
Add ` "build": "webpack"` to your `package.json` file. And then you can run `npm run build` instead of `webpack`  command to bundle your project.
```
npm run build
```

##  Webpack Watch Mode
Webpack's watch mode watches files for changes. If any change is detected, it'll run the compilation again.**(Just rerun the compilation, it can not refresh the browser)**
```cmake
webpack --watch
# OR
webpack --progress --watch
# OR
webpack --progress --colors --watch   
```
After each compilation, you will need to manually refresh your browser to see the changes.

Go to [Webpack Official Website](https://webpack.js.org/guides/development/#webpack-watch-mode) for more information.

##  Webpack-dev-server
Webpack-dev-server provides you with a server and live reloading. This is easy to setup.

Go to [Webpack Official Website](https://webpack.js.org/guides/development/#webpack-dev-server) for more information.

### Install webpack-dev-server globally
```
npm i webpack-dev-server -g
```
### Automatic Cmplilation and Refresh
The webpack-dev-server supports multiple modes to automatically refresh the page:
* Iframe mode (page is embedded in an iframe and reloaded on change)

In this mode, you shuould run the following command and open `localhost:8080/webpack-dev-server` in your browser.
```cmake
# iframe mode
webpack-dev-server
webpack-dev-server --progress --colors
webpack-dev-server --progress --colors --open
```
* Inline mode (a small webpack-dev-server client entry is added to the bundle which refresh the page on change)

In this mode, you shuould run the following command and open `localhost:8080` in your browser.
```cmake
# inline mode
webpack-dev-server --progress --colors --inline
webpack-dev-server --progress --colors --inline --open
```

Each mode also supports `Hot Module Replacement`.



### npm run dev

在`package.json`添加`dev`设置。

```
//package.json
{
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  }
}
```

配置完成后，可以在命令行里运行 `npm run dev` ，运行的时候，会执行 `dev` 属性里的值。

编译完成后，可以访问`http://localhost:8080`查看效果。

上述配置顶的意义
* `webpack-dev-server` - 在 `localhost:8080` 建立一个Web服务器
* `devtool eval` - 为代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
* `progress` - 显示合并代码进度
* `colors` - 命令行中显示颜色
* `content-base build` - 指向设置的输出目录


当项目改动时候，`npm run dev`会自动编译，打包，刷新浏览器。

参考[React和webpack](https://segmentfault.com/a/1190000003768578#articleHeader1)了解更多。


**错误处理**
* 在运行`npm run dev`的时候，如果出现如下错误

```
 Can't resolve 'webpack/hot' in 'C:\Users\lbs\Desktop\webpack\Demo2-React\node_modules\webpack-dev-server\client'
```
* 解决办法
    1. 全局安装 `npm install -g webpack-dev-server`
    2. 本地安装 `npm install --save webpack-dev-server`
    3. 全局安装 `npm install -g webpack`
    4. linking to the global installation  `npm link webpack`
    5. 参考[issue | GitHub](https://github.com/darul75/web-react/issues/12)了解更多。



Go to [Webpack Official Website](http://webpack.github.io/docs/webpack-dev-server.html) for more information.

## Reference
[1] [Getting started webpack tutorial ](https://webpack.github.io/docs/tutorials/getting-started/)

[2] [Webpack List of Tutorials](https://webpack.github.io/docs/list-of-tutorials.html) 

[3] [webpack 教程](https://www.zfanw.com/blog/webpack-tutorial.html)

[4] [初学webpack与react](https://segmentfault.com/a/1190000003768578)

[5] [Webpack中文指南](http://zhaoda.net/webpack-handbook/index.html) 

[6] [webpack-dev-server使用方法](https://segmentfault.com/a/1190000006670084)
