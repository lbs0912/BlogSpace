---
title: Waves.js
date: 2016-09-10 21:13:11
tags: [Waves.js,Front-end] 
categories: Front-end

---

## Waves.js简介
**Waves.js**是一款JavaScript插件库，其遵循Google的[Material Design](https://material.google.com/),可以实现波浪式的点击动画效果。
Waves.js is a javascript library whose click effect is inspired by Google's Material Design.

<!-- more -->

## Waves.js用法
### 包含waves.js插件所需的文件
在HTML文件中包含waves插件所需的waves.js或waves.min.js和waves.css或waves.min.css文件。此类文件可以在waves.js插件的[Github主页](https://github.com/fians/Waves)上下载。

```  html
 <link rel="stylesheet" type="text/css" href="./css/waves.min.css" />
 <script type="text/javascript" src="./js/waves.min.js"></script>

```
### 创建HTML元素并添加类样式
在HTML文件中创建所需的元素。此处以按钮为例，进行说明。

```  html
<a href="#" class="waves-effect waves-button">Click Test 1</a><br/>
<button type="button" class="waves-effect waves-button">
	Click Test 2
</button><br/>
<!-- 为了显示waves.js的点击动画效果，需要使用i标签包裹input标签 -->
<i class="waves-effect waves-button waves-input-wrapper" 
	style="width:85px;height:36px;">
    <input class="waves-button-input" type="submit" value="Button C">
</i>

```
为了显示waves.js的点击动画效果，需要为元素添加waves-effect和waves-button类样式。需要特别指出的是，对于input标签，需要使用i标签包裹input元素，并为i标签添加waves-input-wrapper类样式，为input标签添加
waves-button-input类样式。

### 激活waves.js点击效果
在JavaScript文件中激活waves.js插件的点击效果。

``` html
<script type="text/javascript">
    Waves.displayEffect();
</script>

```

最终效果如下图所示。
![Waves Demo](http://oda53d5ps.bkt.clouddn.com/waves.js-1.gif)

## waves.js 效果示例
### Waves on buttons
![Waves on buttons](http://oda53d5ps.bkt.clouddn.com/waves.js-2.gif)
#### Flat buttons
#### Flat buttons
### Waves on icons
![Waves on icons](http://oda53d5ps.bkt.clouddn.com/waves.js-3.gif)
#### Plain Icons 
#### Colored Icons 
### Waves on others
![Waves on others](http://oda53d5ps.bkt.clouddn.com/waves.js-4.gif)
#### DIVs
#### Images



## 参考文档
1. [Waves.js on Github](https://github.com/fians/Waves)
2. [Waves.js Demo](http://www.yyyweb.com/demo/waves/index.html#)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 