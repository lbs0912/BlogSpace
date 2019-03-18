---
title: CSS实现三角形和平行四边形
date: 2017-05-21 23:21:00
tags: [Front-end,CSS]
categories: Front-end
---




* 本文主要记录如何用CSS实现三角形和平行四边形。
* CSS实现三角形的方法主要包括border方法（最常用），"◆"字符实现和transform实现这3种方法。

<!--more-->

## 利用border实现三角形
### 引入
对于一个div元素，设置其`width`和`height`时，

```vbscript-html
<div class="format"></div>
```
添加其背景颜色为蓝色，并添加红色边框，其效果如下图所示。

```css
.format{
	width: 100px;
	height: 100px;
	background: #0000ff;
	border: 20px solid #ff0000;
}
```

![css-triangle-2.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-2.JPG)


如果分别指定`border-top`，`border-right`，  `border-bottom`和`border-left`，则有如下效果。

```css
.format{
	width: 100px;
	height: 100px;
	background: #0000ff;
	border-top: 20px solid #000;
	border-right: 20px solid #ff0000; 
	border-left: 20px solid #ff0000;
	border-bottom: 20px solid #000; 
}
```
![css-triangle-3.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-3.JPG)

最后，设置div的宽度和高度都为0px，则会出现如下效果。

```css
.format{
	width: 0px;
	height: 0px;
	border-top: 20px solid #000;
	border-right:20px solid #ff0000; 
	border-bottom: 20px solid #000; 	
	border-left: 20px solid #ff0000;
}
```

![css-triangle-4.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-4.JPG)

### 上下对顶三角形

在上述基础上，设置一个div元素的`width`和`height`为0px，并设置`border-left`和`border-right`显示方式为透明`transparent`。

```css
.format{
	width: 0px;
	height: 0px;
	border-top: 20px solid #ff0000;
	border-right:20px solid transparent; 
	border-bottom: 20px solid #ff0000; 	
	border-left: 20px solid transparent;
}

/* 或者，采用如下写法 */
.format{
	width: 0px;
	height: 0px;
	border-width: 20px;
	border-style: solid; 
	border-color: #ff0000 transparent; 	
}
```

![css-triangle-5.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-5.JPG)

### 倒置三角形
在上述基础上，将`border-bottom`的宽度设置为0。

```css
.format{
	width: 0px;
	height: 0px;
	border-width: 20px 20px 0px 20px;
	border-style: solid; 
	border-color: #ff0000 transparent; 	
}

/* 或者，采用如下写法 */
.format{
	width: 0px;
	height: 0px;
	border-top: 20px solid #ff0000; 
	border-right:20px solid transparent; 
	border-left: 20px solid transparent;	
}
```

![css-triangle-6.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-6.JPG)

### 正置三角形
将`border-top`的宽度设置为0。

```css
.format{
	width: 0px;
	height: 0px;
	border-width: 0px 20px 20px 20px;
	border-style: solid; 
	border-color: #ff0000 transparent; 	
}

/* 或者，采用如下写法 */
.format{
	width: 0px;
	height: 0px;
	border-right:20px solid transparent; 
	border-bottom: 20px solid #ff0000; 	
	border-left: 20px solid transparent;	
}
```

![css-triangle-7.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-7.JPG)

### 向右三角形
将`border-right`的宽度设置为0。

```css
.format{
	width: 0px;
	height: 0px;
	border-top: 20px solid transparent; 
	border-bottom: 20px solid transparent; 	
	border-left: 20px solid red;	
}
```
![css-triangle-8.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-8.JPG)

### 向左三角形
将`border-left`的宽度设置为0。

```css
.format{
	width: 0px;
	height: 0px;
	border-top: 20px solid transparent; 
	border-right: 20px solid red;
	border-bottom: 20px solid transparent; 	
	
}
```
![css-triangle-9.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-9.JPG)

### 半边三角形
下面展示如何显示上述三角形的一半。

```css
.triangle-top-right{
	width: 0px;
	height: 0px;
	border-top: 20px solid red; 
	border-right: 20px solid transparent;	
}

.triangle-top-left{
	width: 0px;
	height: 0px;
	border-top: 20px solid red; 
	border-left: 20px solid transparent;	
}

.triangle-bottom-left{
	width: 0px;
	height: 0px;
	border-bottom: 20px solid red; 
	border-left: 20px solid transparent;	
}

.triangle-bottom-right{
	width: 0px;
	height: 0px;
	border-bottom: 20px solid red; 
	border-right: 20px solid transparent;	
}
```


![css-triangle-10.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-10.JPG)

### 注意
上述示例中，利用border属性实现三角形时，把需要隐藏的border设置为透明`transparent`。

但是，在IE6下将边框设置为transparent时，会出现黑影，并不会透明。

处理方法是，在IE6下将`border-style`设置为`dashed`方式。

### 应用：对话框实现

对于如下图中对话框的下三角，其实现方法为设置两个三角形，一个采用背景色，一个采用边框色，然后利用定位重叠在一起。2个三角形位置错开的像素值为边框的宽度值。


![css-triangle-11.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-11.JPG)


```vbscript-html
<div class="message-box">
	<span>利用border实现三角形</span>
	<!--第1个三角形 显示对话框的三角形边框-->
	<div class="triangle-down msgBox-border"></div>
	<!--第2个三角形 显示对话框的三角形背景色-->
	<div class="triangle-down msgBox-bgColor"></div>
</div>
```

```css
/*对话框矩形样式*/
.message-box{
	width: 240px;
	height: 60px;
	text-align: center;
	line-height: 60px;
	background: #E9FBE4;
	border: 2px solid #C9E9C0;
	color:#0C7823;
	box-shadow: 1px 2px 3px #E9FBE4;
	border-radius: 8px;
	position: relative;  /*为三角形绝对定位提供参照*/
}

/*向下三角形样式*/
.triangle-down{
	position: absolute;  /*相对于父辈元素绝对定位*/
	width: 0px;
	height: 0px;
	border-top: 16px solid #E9FBE4;  /*默认显示对话框背景色*/
	border-right: 16px solid transparent;
	border-left: 16px solid transparent;
	left: 30px;
	/*overflow:hidden;*/
}

.msgBox-border{
	border-top: 16px solid  #C9E9C0;  /*重写边框颜色*/
	top:62px;  /*height+border = (60+2)px*/
}

.msgBox-bgColor{
	top:60px; /*height = 60px */
}
```

## 利用border实现平行四边形

根据上述介绍，若利用border实现平行四边形，可以将平行四边形分为3个部分：左三角形，中间矩形和右三角形。

但是，如果每次绘制平行四边形都要创建3个元素，就会显得过于麻烦。所以此处借助伪元素`:befor`和`:after`实现。



![css-triangle-14.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-14.JPG)

为了将三角形与矩形无缝拼接到一起，多处属性要保持一致，所以使用类似 Less, Sass, Stylus 等 CSS 预处理器来写这段代码会更容易维护，下面给出 Scss 版本的代码。（[CodePen链接](http://codepen.io/jerryzou/pen/ZGNGWZ?editors=110)）





```css
//三角形的宽高
$height: 24px;
$width: 12px;

//对平行四边形三部分的颜色进行赋值
@mixin parallelogram-color($color) {
  background: $color;
  &:before { border-color: transparent $color $color transparent; }
  &:after { border-color: $color transparent transparent $color; }
}

//单个三角形的样式
@mixin triangle() {
  content: '';
  display: block;
  width: 0;
  height: 0;
  position: absolute;
  border-style: solid;
  border-width: $height/2 $width/2;
  top: 0;
}

//平行四边形的样式
.para {
  display: inline-block;
  position: relative;
  padding: 0 10px;
  height: $height;
  line-height: $height;
  margin-left: $width;
  color: #fff;

  &:after {
    @include triangle();
    right: -$width;
  }

  &:before {
    @include triangle();
    left: -$width;
  }

  @include parallelogram-color(red);
}
```

需要注意的是，如果通过 `$height`、`$width` 设置的三角形斜率太小或太大都有可能造成渲染出锯齿，所以使用起来要多多测试一下不同的宽高所得到的视觉效果如何。

## 利用"◆"字符实现三角形

* `◆`符号可以利用特殊键盘符号输入。
* 字符法实现上述`应用：对话框实现`中三角形效果，其基本思想和上述方法相同，即用两个字符用绝对定位去模拟，让两个字符错开的像素值为边框宽度值即可。
* 设置`◆`字符的大小，可以创建一个菱形。让其中一个菱形颜色和背景色相同，另外一个菱形颜色和边框颜色相同。二者错开一定的距离，即可实现对话框的三角形效果。
* 字符的大小是由`font-size`决定的，`width` 和 `height`无法决定字符的大小。但是为了保险起见，可以将宽度值和高度值设置成`font-size`的大小，这样各个浏览器去生成这个字符的时候能保持大小一致。

![css-triangle-12.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-12.JPG)


```vbscript-html
<div class="message-box">
	<span>利用◆实现三角形</span>
	<!--第1个三角形 显示对话框的三角形边框-->
	<div class="triangle-character msgBox-border">◆</div>
	<!--第2个三角形 显示对话框的三角形背景色-->
	<div class="triangle-character msgBox-bgColor">◆</div>
</div>
```

```css
/*对话框矩形样式*/
.message-box{
	width: 240px;
	height: 60px;
	text-align: center;
	line-height: 60px;
	background: #E9FBE4;
	border: 2px solid #C9E9C0;
	color:#0C7823;
	box-shadow: 1px 2px 3px #E9FBE4;
	border-radius: 8px;
	position: relative;  /*为三角形绝对定位提供参照*/
}

/*菱形样式*/
.triangle-character{
	position: absolute;  /*相对于父辈元素绝对定位*/
	width: 20px;
	height: 20px;
	left: 30px;
	font: normal 20px "宋体";   /*字符的大小和字体也有关系*/
}

.msgBox-border{
	color: #C9E9C0;
	top: 50px;   /* 48+2 = 50*/
}

.msgBox-bgColor{
	color: #E9FBE4;
	top: 48px;  /* height(60)-fontSize(20)/2-border(2) = 48*/
}
```

## 利用 CSS3 transfrom 旋转45度实现三角形
创建一个带`border`的 `div` ，设置背景色和相邻的两个边框的颜色，然后旋转45 度实现。

![css-triangle-13.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-13.JPG)


`transform`属性是CSS3的属性，存在兼容性问题，并且旋转会对性能造成损耗，不推荐使用该方案。

该方法的详细情况参见[3种CSS实现三角形方式 | CSDN](http://blog.csdn.net/huanghui8030/article/details/16984933)。


```vbscript-html
<div class="message-box">
  <span>我是利用 css transfrom 属性字符实现的</span>
  <div class="triangle-css3 transform ie-transform-filter"></div>
</div>
```

```css
.message-box {
    position:relative;
    width:240px;
    height:60px;
    line-height:60px;
    background:#E9FBE4;
    box-shadow:1px 2px 3px #E9FBE4;
    border:1px solid #C9E9C0;
    border-radius:4px;
    text-align:center;
    color:#0C7823;
}
.triangle-css3 {
    position:absolute;
    bottom:-8px;
    bottom:-6px;
    left:30px;
    overflow:hidden;
    width:13px;
    height:13px;
    background:#E9FBE4;
    border-bottom:1px solid #C9E9C0;
    border-right:1px solid #C9E9C0;
}
.transform {
    -webkit-transform:rotate(45deg);
    -moz-transform:rotate(45deg);
    -o-transform:rotate(45deg);
    transform:rotate(45deg);
}
/*ie7-9*/
.ie-transform-filter {
    -ms-filter: "progid:DXImageTransform.Microsoft.Matrix(
        M11=0.7071067811865475,
        M12=-0.7071067811865477,
        M21=0.7071067811865477,
        M22=0.7071067811865475,
    SizingMethod='auto expand')";
    filter: progid:DXImageTransform.Microsoft.Matrix(
        M11=0.7071067811865475,
        M12=-0.7071067811865477,
        M21=0.7071067811865477,
        M22=0.7071067811865475,
    SizingMethod='auto expand');
}
```


## 利用transform实现平行四边形



![css-triangle-15.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-15.JPG)

利用`transform:skew`倾斜属性实现平行四边形。

```vbscript-html
<style type="text/css">
.city {
  display: inline-block;
  padding: 5px 20px;
  border: 1px solid #44a5fc;
  color: #333;
  transform: skew(-20deg);
}
</style>

<div class="city">上海</div>
```
上述代码，将实现如下效果。可以发现，边框中的文字”上海“也被倾斜了，这并不是我们希望的。CSS对整个div进行了倾斜，导致中间的文字也是倾斜的。因此，需要添加一个内层元素，并对内层元素做一次逆向的倾斜，从而得到想要的效果。

![css-triangle-16.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-16.JPG)


最终程序和效果图如下所示，另附[CodePen演示](http://codepen.io/jerryzou/pen/BNeNwV?editors=110)。
```vbscript-html
<style type="text/css">
.city {
  display: inline-block;
  padding: 5px 20px;
  border: 1px solid #44a5fc;
  color: #333;
  transform: skew(-20deg);
}

.city div {
  transform: skew(20deg);
}
</style>

<div class="city">
  <div>上海</div>
</div>
```

![css-triangle-17.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-17.JPG)


## transform:skew属性
参考[CSS3属性transform详解之（旋转:rotate,缩放:scale,倾斜:skew,移动:translate）](http://blog.csdn.net/er_ba/article/details/47062229)了解更多。


* `transform: skew`用于表示倾斜。
* 用法： `transform: skew(30deg)`  或者 `transform: skew(30deg, 30deg)`。其中，参数表示倾斜角度，单位deg。
* **一个参数时：表示水平方向的倾斜角度。**
* **两个参数时，第一个参数表示水平方向的倾斜角度，第二个参数表示垂直方向的倾斜角度。**
* **skew的默认原点(即`transform-origin`)是这个物体或元素的中心点。**

下面，给出具体的示例。
* skewX(30deg)

![css-triangle-18.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-18.JPG)
* skewY(10deg)

![css-triangle-19.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-19.JPG)
* skew(30deg, 10deg)


![css-triangle-20.JPG](http://ol3kbaay9.bkt.clouddn.com/css-triangle-20.JPG)

## 参考资料
* [详解css画一个三角形 | 百度经验](http://jingyan.baidu.com/article/08b6a591a3208914a809222f.html)
* [CSS实现三角形图标的原理](http://www.tuicool.com/articles/3eaINn)
* [纯CSS写三角形-border法](http://www.jb51.net/article/42513.htm)
* [纯CSS绘制三角形](http://www.cnblogs.com/blosaa/p/3823695.html)
* [3种CSS实现三角形方式 | CSDN](http://blog.csdn.net/huanghui8030/article/details/16984933)
* [用 CSS 实现三角形与平行四边形](http://jerryzou.com/posts/use-css-to-paint-triangle-and-parallelogram/)
* [CSS3属性transform详解之（旋转:rotate,缩放:scale,倾斜:skew,移动:translate）](http://blog.csdn.net/er_ba/article/details/47062229)




 

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 