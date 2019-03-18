---
title: React项目——画廊应用
date: 2017-08-29 19:35:26
categories: React
tags: [Front-end,React]
keywords: Front-end,React
toc: true
---




* 本文针对慕课网上视频教学，记录React项目——画廊应用的创建过程。
* [Demo效果](http://liubaoshuai.com/react-gallery/)
* [GitHub | 脱缰的哈士奇](https://github.com/lbs0912/)
* [React项目——画廊应用（上） | 慕课网](http://www.imooc.com/learn/507)
* [React项目——画廊应用（下） | 慕课网](http://www.imooc.com/learn/652)
* [源码参考1 | GitHub](https://github.com/daydayupsnail/react-practice-gallery/)
* [源码参考2 | GitHub](https://github.com/cllgeek/gallery-by-react/)


<!--more-->


## Yeoman
* [Yeoman](http://yeoman.io/)
* [Getting started | Yeoman](http://yeoman.io/learning/)


Yeoman其实是三个工具的集合
* `YO`：Yeoman核心工具，项目工程依赖目录和文件生成工具，项目生产环境和编译环境生成工具
* `GRUNT`：前端构建工具
* `BOWER`：Web开发的包管理器，概念上类似npm，npm专注于nodeJs模块，而bower专注于CSS、JavaScript、图像等前端相关内容的管理


1. 安装Yeoman：`npm install -g yo`
2. 查看Yeoman版本： `yo -version`
3. 在[Yeoman官网](http://yeoman.io/generators/)选择项目生成器`Generators`，此处选择`generator-react-webpack`并进行安装`npm install -g generator-react-webpack`。
4. 创建项目，生成项目框架

```
# Create a new directory, and `cd` into it:
mkdir my-new-project && cd my-new-project

# Run the generator
yo react-webpack
```
5. 第4步骤中，项目生成时，选择Sass，不使用路由（单页应用）。
6. 项目生成器安装之后，运行`npm start`运行项目，访问`http://localhost:8000/`查看效果。 
7. 安装Chrome扩展程序`React Developer Tools`，方便调试React程序。在Chrome的控制台，可以在`React`目录可以调试程序。


## 项目准备

### CSS样式兼容性

1. 安装`npm install autoprefixer-loader --save-dev`，支持CSS中自动添加前缀，支持多浏览器。
2. 修改webpack的配置信息，打开`cfg`目录下的`default.js`文件，对`loaders`信息进行修改。添加`autoprefixer-loader`信息。


```javascript
//before modify
{
    test: /\.css$/,
    loader: 'style-loader!css-loader'
},
{
    test: /\.scss/,
    loader: 'style-loader!css-loader!sass-loader?outputStyle=expanded'
},

//after modify
// add info : autoprefixer-loader?{browsers:["last 2 version"]}
{
    test: /\.css$/,
    loader: 'style-loader!css-loader!autoprefixer-loader?{browsers:["last 2 version"]}'
},
{
    test: /\.scss/,
    loader: 'style-loader!css-loader!autoprefixer-loader?{browsers:["last 2 version"]}!sass-loader?outputStyle=expanded'
},
```


### Datas层处理
1. 在`src`目录下创建`Datas`目录，并创建`ImageDatas.json`文件，用于存放图片信息。在`src | images`目录下存放对应的图片。


```json
[
	{
		"filename": "1.jpg",
		"title": "lbs0912",
		"des": "hello world，20170829"
	},
	{
		"filename": "2.jpg",
		"title": "lbs0912",
		"des": "hello world，20170829"
	}
]
```
2. 安装JSON加载器，`npm install json-loader --save-dev`。
3. 修改webpack的配置信息，打开`cfg`目录下的`default.js`文件，对`loaders`信息进行修改。添加`json-loader`信息。


```
{
    test: /\.json/,
    loader: 'json-loader'
},
```

4. 在`Main.js`文件中引入图片信息JSON文件。

```javascript
//json file
var imageDatas = require("../datas/ImageDatas.json");

//获取JSON数组中每张图片的URL信息
function getImageURL(imageDataArr){
	for(let i=0,j=imageDataArr.length;i<j;i++){
		let singleImageData = imageDataArr[i];
		singleImageData.imageURL = require('../images/'+singleImageData.fileName);
		imageDataArr[i] = singleImageData;
	}
	return imageDataArr;
}
//将图片JSON信息转换为图片URL信息
imageDatas = getImageURL(imageDatas);


//由于上述函数只执行1次，可以采用立即执行函数表达式
imageDatas = (function getImageURL(imageDataArr){
	for(let i=0,j=imageDataArr.length;i<j;i++){
		let singleImageData = imageDataArr[i];
		singleImageData.imageURL = require('../images/' + singleImageData.fileName);
		imageDataArr[i] = singleImageData;
	}
	return imageDataArr;
})(imageDatas);
```


## 项目开发
1. 图片位置确定
2. 图片旋转




## 项目打包和发布
* 运行`npm run dist`进行打包，会生成`dist`目录。
* 在项目开发时候，是直接在一级域名下访问的（`http://localhost:8000/`）。项目发布时候，有可能会在二级域名下访问（如`http://liubaoshuai.com/react-gallery/`）。这个时候，图片和一些资源的路径就要使用相对路径，不能使用绝对路径。因此，需要做出如下修改。


```javascript
//修改webpack.config.js的publicPath信息
//最新版本react-webpack中，是修改cfg目录下的default.js文件
//修改之前为  publicPath: '/assets/',
//修改之后
publicPath: 'assets/',



//在最后打包生成的dist目录下的index.html中，修改app.js的路径
//dist/index.html
//修改前
<script type="text/javascript" src="/assets/app.js"></script>

//修改后
<script type="text/javascript" src="./assets/app.js"></script>
```

* 使用`git subtree push --prefix=dist origin gh-pages`创建`gh-pages`分支，并把master分支的`dist`目录下内容复制到该分支上。参考[如何用Github的gh-pages分支展示自己的项目](http://www.cnblogs.com/MuYunyun/p/6082359.html)了解更多。
* 访问`http://liubaoshuai.com/react-gallery/`查看项目效果。