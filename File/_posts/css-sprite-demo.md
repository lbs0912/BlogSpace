---
title: CSS Sprite 雪碧图应用
date: 2017-05-29 14:35:26
categories: Front-end
tags: [Front-end,CSS]
keywords: CSS
toc: true
---



* 本文主要对CSS Sprite 雪碧图的应用进行记录和归纳。
* [CSS Sprite | 慕课网](http://www.imooc.com/learn/93)
* 本文对应的Demo可以在[我的Github](https://github.com/lbs0912/CSS-Sprite-Demo.git)获取。


<!--more-->


## 简介
为了减少HTTP请求数量，加速网页内容显示，很多网站的导航栏图标、登录框图片等，使用的并不是`<image>`标签，而是CSS Sprite雪碧图。本文分析了CSS Sprite雪碧图的实现原理，详细讲解制作方法，实现完整效果，对CSS Sprite雪碧图技术进行记录和归纳。

![css-sprite-lesson-1.jpg](http://ol3kbaay9.bkt.clouddn.com/css-sprite-lesson-1.jpg)

以淘宝网首页为例，其导航条加载的[雪碧图](http://gtms01.alicdn.com/tps/i1/T1iG48FDpfXXX8QSDI-400-340.png)为

![T1iG48FDpfXXX8QSDI-400-340.png](http://ol3kbaay9.bkt.clouddn.com/T1iG48FDpfXXX8QSDI-400-340.png)

## CSS Sprite优点
* 有效地减少HTTP请求数量
* 加速内容显示

## 使用CSS Sprite的场景

* 静态图片，不随用户信息的变化而变化。
* 小图片（一般3~5K），图片容量比较小。（大的轮播图一般直接采用image标签）
* 加载量比较大
* 一些大图不建议拼成雪碧图。

## CSS Sprite实现原理
* 设置一个可显示的窗口或区域，来显示雪碧图的一部分。例如

```css
li{
	width: 40px;
	height: 60px;
}
```

* 通过设置雪碧图的位置(`background-position`)来进行雪碧图的滑动，从而显示不同的内容。默认显示的是从雪碧图的原点`(0,0)`开始显示`width * height`区域的大小。因此，如果要想显示雪碧图的其他部分，可以通过设置`background-position`的值实现。注意，考虑到坐标系的方向，`background-position`的偏移量一般为**负值**。

```css
li{
	background: url("test.png");
	background-position: -40px 0px;  /*显示第1列第2行的图片*/
	
	background: url("test.png") -40px 0px;  /*简写形式*/
	
}
```



![css-sprite-lesson-4.jpg](http://ol3kbaay9.bkt.clouddn.com/css-sprite-lesson-4.jpg)

## 制作CSS Sprite
* 方法1：PS手动拼图
* 方法2：使用sprite工具自动生成

此处介绍一款CSS Sprite自动生成工具——[CSSGaga](http://www.99css.com/1524/)。其余CSS Sprite工具链接
* [Go! Png | Tencent ](http://alloyteam.github.io/gopng/)
* [CSS Sprite Generate](https://www.toptal.com/developers/css/sprite-generator)
* [CSS Sprite | NPM](https://segmentfault.com/a/1190000002910313)

## Demo 1
使用一张雪碧图制作一个静态页面Demo，格式仿照淘宝首页的导航栏。

![css-sprite-lesson-3.jpg](http://ol3kbaay9.bkt.clouddn.com/css-sprite-lesson-3.jpg)

本文对应的Demo可以在[我的Github](https://github.com/lbs0912/CSS-Sprite-Demo.git)获取。核心代码如下。

```vbscript-html
<li class="cat-1">  <!--category-->
	<i></i>
	<h3>服装内衣</h3>
</li>
<li class="cat-2"> 
	<i></i>
	<h3>鞋包配饰</h3>
</li>
<li class="cat-3"> 
	<i></i>
	<h3>运动户外</h3>
</li>
<li class="cat-4"> 
	<i></i>
	<h3>珠宝手表</h3>
</li>
```

```css
/*雪碧图设置*/
li i{
	background: url("../img/sidebar.png");
	display: inline;
	width: 30px;   /* 设定显示区域大小为30*24*/
	height: 24px;
	float: left;
	margin: 5px 10px 0 10px;
}
.cat-1 i{background-position: 0px 0px;}
.cat-2 i{background-position: 0px -24px;}  /*显示区域的高度*/
.cat-3 i{background-position: 0px -48px;}
.cat-4 i{background-position: 0px -72px;}
.cat-5 i{background-position: 0px -96px;}
.cat-6 i{background-position: 0px -120px;}
.cat-7 i{background-position: 0px -144px;}
.cat-8 i{background-position: 0px -168px;}
.cat-9 i{background-position: -40px 0px;}  /*显示区域的宽度30px + 空白边界10px*/
.cat-10 i{background-position: -40px -24px;}
```
## Demo 2

![logo-status.png](http://ol3kbaay9.bkt.clouddn.com/logo-status.png)
使用如上所示的雪碧图，创建如下登录界面。

![css-sprite-lesson-2.jpg](http://ol3kbaay9.bkt.clouddn.com/css-sprite-lesson-2.jpg)

本文对应的Demo可以在[我的Github](https://github.com/lbs0912/CSS-Sprite-Demo.git)获取。核心代码如下。

```vbscript-html
<!-- html file -->
<input type="button" class="button-log-in">
<hr>
<input type="button" class="button-register">
```
```css
/*CSS file */
.button-log-in{
	background: url("./img/logo-status.png") 0px 0px;
}
.button-register{
	background: url("./img/logo-status.png")  0px -38px;
}
```



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 