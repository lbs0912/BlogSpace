---
title:  CSS实现loading图
date: 2017-06-13 11:35:48
categories: Front-end
tags: [Front-end,CSS]
keywords: CSS Loading Demo
---





* 本文中Demo源码可以在[我的Github](https://github.com/lbs0912/CSS-Loading-Demo)获取。
* [CSS3实现鸡蛋饼饼状图loading等待转转转](http://www.zhangxinxu.com/wordpress/2014/04/css3-pie-loading-waiting-animation/)
* [用纯CSS实现饼状Loading等待图](http://www.toutiao.com/i6421747200820249089/)


<!--more-->


## Demo 1


本Demo源码可以在[我的Github](https://github.com/lbs0912/CSS-Loading-Demo)获取。

下图为一个gif图片，大小为27K。下面考虑采用CSS实现该效果。 CSS实现的版本中大小只有1.44K，存储上远远优于gif版本，并且不需要请求HTTP获取gif图片。

![small-loading.gif](http://ol3kbaay9.bkt.clouddn.com/small-loading.gif)

采用CSS实现上图Demo，将其划分为2大部分：外圆和内圆。

### 外圆实现

![icon-spin-s.png](http://ol3kbaay9.bkt.clouddn.com/icon-spin-s.png)

外圆实现比较简单。引入上图所示的图片，让其旋转即可。

```vbscript-html
<div class="wrap">
    <div class="outer"></div>
    <!--外圆部分-->
</div>
```

```css
.wrap { 
	width: 64px; 
	height: 64px; 
	position: relative; 
}
.outer { 
	position: absolute; 
	width: 100%; 
	height: 100%; 
	background:url("http://www.zhangxinxu.com/study/201404/icon-spin-s.png") no-repeat; 
	animation: spin 800ms infinite linear; 
}

@keyframes spin {
	0%   { transform: rotate(360deg); }
	100% { transform: rotate(0deg); }
}
```
### 内圆实现

借鉴煎饼做法，有如下图示。

* 状态1

![css-loading-demo-1.png](http://ol3kbaay9.bkt.clouddn.com/css-loading-demo-1.png)

![css-loading-demo-2.png](http://ol3kbaay9.bkt.clouddn.com/css-loading-demo-2.png)

* 状态2

![css-loading-demo-1.png](http://ol3kbaay9.bkt.clouddn.com/css-loading-demo-3.png)

![css-loading-demo-2.png](http://ol3kbaay9.bkt.clouddn.com/css-loading-demo-4.png)

从上图可以总结分析，得出内圆的实现思路
* 添加类名`inner`（其作用相当于真蛋饼）。`inner`用于实现圆和背景色（深色）。
* 类`spiner`（其作用相当于真鸡蛋）用于实现半圆的360度逆时针旋转。其颜色为浅蓝色。
* 类`filter`（其作用相当于假鸡蛋）构建一个半圆，定位在右侧，与旋转元素颜色相同（浅蓝色）。后面180度隐藏。
* 类`masker`（其作用相当于假蛋饼）构建一个半圆，定位在左侧，与背景色相同（深蓝色）。`spiner`旋转前180度过程中，`masker`隐藏，之后显示遮盖。
* 动画开始时，`spiner`（真鸡蛋）初始位置在左侧，旋转前180度中，`masker`（假蛋饼）位于左侧，状态为隐藏。因此，初试状态显示为全浅色。随着`spiner`的旋转，逐渐露出`inner`（真蛋饼）的深色部分。
* `spiner`（真鸡蛋）旋转180度后，左侧显示`inner`（真蛋饼）的深色。进入后180度旋转后，`masker`（假蛋饼）显示，背景为深色，`filter`（假鸡蛋）位于右侧，开始隐藏。随着`spiner`的旋转，浅色部分逐渐减小。旋转至左侧的`spiner`浅色部分被`masker`遮挡。
* 以深蓝色所占比例描述，上述动画实现了深蓝色从0度~360度的显示。
* **注意不同元素的z-index值。**
* 元素前半程显示，后半程隐藏的动画实现

```css
/*元素前半程显示，后半程隐藏*/
@keyframes second-half-hide {
	0% {
		opacity: 1;
	}
	50%, 100% {
		opacity: 0;
	}
}
```
 
 * 元素前半程隐藏，后半程显示的动画实现

```css
/*元素前半程隐藏，后半程显示*/
@keyframes second-half-show {
	0% {
		opacity: 0;
	}
	50%, 100% {
		opacity: 1;
	}
}
```

* 设定动画时间

```css
.spiner { 
	transform-origin: right center; 
	animation: spin .8s infinite linear; 
}
.filler { 
	animation: second-half-hide .8s steps(1, end) infinite; 
}
.masker { 
	animation: second-half-show .8s steps(1, end) infinite; 
}
```


至此，饼状load图基本完成。但是仔细观察可以发现，深色部分将从0度增加到360度，然后再次从0度到360度，中间并不是连贯的。

为了解决该问题，其方法为
* 再引入一个`inner2`类。并颠倒深蓝色和浅蓝色的设置
* 设置spiner类和filter类为深蓝色。inner2类和mask类为浅蓝色。
* 和上述动画效果恰好相反，实现浅蓝色所占比例从0到360度显示。即深蓝色比例从360度到0度显示。

```html
<div class="inner">
    <div class="spiner"></div>
    <div class="filler"></div>
    <div class="masker"></div>
</div>
<div class="inner2">
    <div class="spiner"></div>
    <div class="filler"></div>
    <div class="masker"></div>
</div>
```

### 总结
至此，用CSS实现加载效果完成。
* inner类旋转周期设为0.8s，实现深蓝色比例从0到360度显示。 inner2类旋转周期设为0.8s，实现深蓝色比例从360到0度显示。
* 第1个0.8s内，inner类显示，inner2类隐藏。其效果为深蓝色比例从0到360度显示。
* 第2个0.8s内，inner2类显示，inner类隐藏。其效果为深蓝色比例从360到0度显示。
* 外圆旋转周期为0.8s。内圆交替显示周期为1.6s，是外圆旋转周期的2倍。内圆旋转周期也是0.8s。

本Demo源码可以在[我的Github](https://github.com/lbs0912/CSS-Loading-Demo)获取。

#### CSS版本的优点
采用CSS实现该效果。 CSS实现的版本中大小只有1.44K，存储上远远优于gif版本（大小为27k），并且不需要请求HTTP获取gif图片。


 

## Demo 2
 ![css-loading-demo-2.gif](http://ol3kbaay9.bkt.clouddn.com/css-loading-demo-2.gif)
 
本Demo源码可以在[我的Github](https://github.com/lbs0912/CSS-Loading-Demo)获取。

### 实现思路
* 上述加载图有8个小圆点组成，可以设置对应的8个div。
* 每个小圆点从最出的透明度为0，逐渐透明度增加，形状进行放大。将该变化封装为一个动画。
* 8个小圆点按照时间顺序逐次调用该动画即可。该功能可以通过动画延迟实现。

```css
animation: loading 1s 0.6s infinite linear;  
/* 动画延迟0.6s开始*/
```
### 源码

```html
<!DOCTYPE html>
<html>
<head>
	<title>CSS Loading Demo 2</title>
	<meta charset="utf-8">
	<style type="text/css">
	.loadingTip {
		position: fixed;
		top: 50%;
		left: 50%;
		-webkit-transform: translate3d(-50%, -50%, 0);
		z-index: 10001;
	}
	.loadingTip div {
		background-color: #000;
		width: 15px;
		height: 15px;
		border-radius: 100%;
		margin: 2px;
		-webkit-animation-fill-mode: both;
		animation-fill-mode: both;
		position: absolute;
	}
	@-webkit-keyframes loading {
		50% {
			opacity: 0.3;
			-webkit-transform: scale(0.4);
			transform: scale(0.4);
		}
		100% {
			opacity: 1;
			-webkit-transform: scale(1);
			transform: scale(1);
		}
	}
	.loadingTip > div:nth-child(1) {
		top: 25px;
		left: 0;
		-webkit-animation: loading 1s 0s infinite linear;
		animation: loading 1s 0s infinite linear;
	}
	.loadingTip > div:nth-child(2) {
		top: 17.04545px;
		left: 17.04545px;
		-webkit-animation: loading 1s 0.12s infinite linear;
		animation: loading 1s 0.12s infinite linear;
	}
	.loadingTip > div:nth-child(3) {
		top: 0;
		left: 25px;
		-webkit-animation: loading 1s 0.24s infinite linear;
		animation: loading 1s 0.24s infinite linear;
	}
	.loadingTip > div:nth-child(4) {
		top: -17.04545px;
		left: 17.04545px;
		-webkit-animation: loading 1s 0.36s infinite linear;
		animation: loading 1s 0.36s infinite linear;
	}
	.loadingTip > div:nth-child(5) {
		top: -25px;
		left: 0;
		-webkit-animation: loading 1s 0.48s infinite linear;
		animation: loading 1s 0.48s infinite linear;
	}
	.loadingTip > div:nth-child(6) {
		top: -17.04545px;
		left: -17.04545px;
		-webkit-animation: loading 1s 0.6s infinite linear;
		animation: loading 1s 0.6s infinite linear;
	}
	.loadingTip > div:nth-child(7) {
		top: 0;
		left: -25px;
		-webkit-animation: loading 1s 0.72s infinite linear;
		animation: loading 1s 0.72s infinite linear;
	}
	.loadingTip > div:nth-child(8) {
		top: 17.04545px;
		left: -17.04545px;
		-webkit-animation: loading 1s 0.84s infinite linear;
		animation: loading 1s 0.84s infinite linear;
	}
	</style>
</head>
<body>
	<div class="loadingTip">
		<div></div>
		<div></div>
		<div></div>
		<div></div>
		<div></div>
		<div></div>
		<div></div>
		<div></div>
	</div>
</body>
</html>
```


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 