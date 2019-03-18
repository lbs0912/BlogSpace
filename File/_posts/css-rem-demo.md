---
title: 移动网页rem实现伸缩式布局设计
date: 2017-05-31 14:35:26
categories: Front-end
tags: [Front-end,CSS]
keywords: CSS
toc: true
---




* 本文主要对CSS中rem单位进行介绍，对rem实现伸缩式布局设计进行介绍。



<!--more-->

## px em rem
* **em: 相对于父元素设置字体大小**
* **rem: 相对于根元素`<html>`设置字体大小**



`rem`是CSS3新引进的一个度量单位，其兼容性如下表所示。对于移动端，IOS系统6.1版本以上都支持，android系统2.1版本以上系统都支持。对于PC端，大部分主流浏览器都支持。**IE6~8无法支持`rem`度量单位。**


 ![css3-rem-compatible-big.JPG](http://ol3kbaay9.bkt.clouddn.com/css3-rem-compatible-big.JPG)

*注：可在 [Can I use CSS](http://caniuse.com/#search=rem)上查询 rem 的兼容性。*

## 为什么使用rem
使用`rem`单位和`em`单位可以实现**Responsive设计**。

相比`em`单位，`rem`单位避开了很多**层级关系**，只要指定根元素HTML的`font-size`值，后续所有字体大小都将以为参考。而使用em单位，需要知道每个元素的父辈元素的大小，十分繁琐。


## rem值计算
默认情况下，浏览器根元素`<html>`的`font-size`是16px，则`16px=1rem`。其他px值和rem值的转换如下所示。

```
| px |      rem      | 
---------------------- 
| 12 | 12/16 = .75   |  
| 14 | 14/16 = .875  | 
| 16 | 16/16 = 1     | 
| 18 | 18/16 = 1.125 | 
| 20 | 20/16 = 1.25  | 
| 24 | 24/16 = 1.5   | 
| 30 | 30/16 = 1.875 | 
| 36 | 36/16 = 2.25  | 
| 42 | 42/16 = 2.625 | 
| 48 | 48/16 = 3     |
```

为了px和rem的转换方便，一般将根元素HTML的`font-size`设置为10px，则`10px=1rem`，此时`12px=1.2rem`，转换的计算量大大减小了。

一般采取`font-size: 62.5%`来将根元素HTML的`font-size`设置为10px。

```css
html { 
	font-size: 62.5%; 
	/* 10 ÷ 16 × 100% = 62.5% */ 
}
```

```
| px |     rem     | 
------------------------- 
| 12 | 12/10 = 1.2 | 
| 14 | 14/10 = 1.4 |
| 16 | 16/10 = 1.6 | 
| 18 | 18/10 = 1.8 | 
| 20 | 20/10 = 2.0 | 
| 24 | 24/10 = 2.4 | 
| 30 | 30/10 = 3.0 | 
| 36 | 36/10 = 3.6 | 
| 42 | 42/10 = 4.2 | 
| 48 | 48/10 = 4.8 |
```

## Demo
创建一个静态网页，对DOM元素和图片元素，采用rem单位设定其大小。通过该Demo说明rem单位在响应式设计中的应用。


本Demo代码可以在[我的Github](https://github.com/lbs0912/CSS-rem-Demo)获取。

在以iphone 5s屏幕的320px宽度为基准的设计稿中，根元素大小设置为20px。采用rem单位对header，footer和img元素进行大小设定。iphone 5s中页面布局如下图所示。

![css-rem-demo-1.JPG](http://ol3kbaay9.bkt.clouddn.com/css-rem-demo-1.JPG)

采用rem进行响应式设计之后，ipad（触发屏幕旋转操作后）中的页面样式如下图所示。
![css-rem-demo-3.JPG](http://ol3kbaay9.bkt.clouddn.com/css-rem-demo-3.JPG)


而未采用rem进行响应式设计的样式如下图所示。
![css-rem-demo-2.JPG](http://ol3kbaay9.bkt.clouddn.com/css-rem-demo-2.JPG)



从上图可以看出，响应式设计后的页面布局，在手机和平板（触发屏幕旋转操作后）上大体呈现相同布局。而未采用响应式设计的页面，在平板上的显示效果很差。

下面介绍下该Demo中核心功能代码。本Demo代码可以在[我的Github](https://github.com/lbs0912/CSS-rem-Demo)获取。



```css
/* css file */
header,footer{
	width: 100%;
	line-height: 1.5rem;
	font-size: 0.8rem;
	color: #fff;
	text-align: center;
	background-color: #999;
}
img{
	display: block;
	width: 14rem;
	margin: 1.2rem auto;
}
```
```javascript
//JavaScript file
docEl.style.fontSize = 20*(clientWidth/320)+"px";  
//设置根元素font-size
```

从上述代码可以看出，该响应式功能的实现，依赖于对根元素大小的设置和rem单位的应用。宽度为320px的设计稿中，根元素的大小为20px。则屏幕为clientWidth的屏幕，其根元素的大小为`20*(clientWidth/320)`。本Demo中，采用JS监听DOM加载和屏幕旋转事件。如果该事件被监听到，则会查询屏幕宽度值clientWidth，动态改变根元素的大小值。


也可以采用CSS中的媒体查询实现对根元素大小的设定。

```css
html{
	font-size: 20px;
}
@media only screen and (min-width: 401px) {
	html{
		font-size: 25px !important;
	}
}
@media only screen and (min-width: 428px) {
	html{
		font-size: 26.75px !important;
	}
}
@media only screen and (min-width: 481px) {
	html{
		font-size: 30px !important;
	}
}
```





## 参考资料
* [web app变革之rem](https://isux.tencent.com/web-app-rem.html)
* [移动网页rem实现伸缩式布局设计](http://www.toutiao.com/i6424385567504466434/)
## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 