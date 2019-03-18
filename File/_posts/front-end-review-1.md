---
title: 前端面试复习-1
date: 2017-04-14 11:35:48
categories: Front-end
tags: [Front-end]
keywords: 前端面试复习
---



* 本文主要对前端面试中常考察的知识点进行记录和归纳。


<!--more-->

## 盒子模型Box Model
CSS盒子模型Box Model本质上是一个盒子，封装周围的HTML元素，盒子模型包括外边距margin，边框border，内边距padding和实际内容content。



内边距padding，边框border和外边距margin都是可选的，默认值是0。

盒子模型分为标准盒子模型和IE盒子模型。

### 标准W3C盒子模型

![box-model-w3c.jpg](http://ol3kbaay9.bkt.clouddn.com/box-model-w3c.jpg)
标准盒子模型中，设置元素的宽度width和高度height属性值，是不包括padding和border的，仅仅计算了内容content区域。

`width= content`




### IE盒子模型

![box-model-ie.jpg](http://ol3kbaay9.bkt.clouddn.com/box-model-ie.jpg)

IE盒子模型中，设置元素的宽度width和高度height属性值，是包括padding和border的。

`width=border+padding+content`

### 如何选择盒子模型

为了网页能兼容所有的浏览器，推荐在网页中使用标准W3C盒子模型。
* 方法1
在网页顶部添加`<!DOCTYPE html>`声明，表示使用标准W3C盒子模型。否则，各个浏览器会根据自己的理解去解析网页。
* 方法2
使用`box-sizing`来改变使用的盒子模型。

```css
/* Keyword values */
box-sizing: content-box;
box-sizing: border-box;

/* Global values */
box-sizing: inherit;
box-sizing: initial;
box-sizing: unset;
```

**content-box** ：This is the initial and default value as specified by the CSS standard. The width and height properties are measured including only the content, but not the padding, border or margin. 

Note: Padding, border & margin will be outside of the box e.g. 
IF `.box {width: 350px;}` 
THEN you apply `{border: 10px solid black;}` RESULT {rendered in the browser} a box of width: 370px.

**border-box** ：The width and height properties include the content, the padding and border, but not the margin. This is the box model used by Internet Explorer when the document is in Quirks mode. 

Note that padding and border will be inside of the box e.g.  
`.box {width: 350px; border: 10px solid black;}` leads to a box rendered in the browser of width: 350px. 

The content box can't be negative and is floored to 0, making it impossible to use border-box to make the element disappear.

Here the dimension is calculated as, width = border + padding + width of the content, and height = border + padding + height of the content.

该属性是CSS3新值的属性，使用时候注意添加`-webkit`, `-moz`等前缀。IE8以及以上版本支持该属性。

参考[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)了解更多。



## 常用的浏览器和其内核

参考资料
* [常用的浏览器内核](http://www.qdfuns.com/notes/27225/35cc7a9c864b49c727570b13c41a2c07.html)
* [浏览器与内核总结](http://www.cnblog.me/2015/04/06/broswer-kernel/)
* [浏览器内核](http://www.cnblogs.com/zichi/p/5116764.html)

### 浏览器内核

* Trident
Trident(['traɪdnt])内核的代表产品是IE浏览器，故又称为IE内核。Trident也称为MSHTML，是微软开发的一种排版引擎。使用Trident渲染引擎的浏览器包括：IE，遨游MaxThon，世界之窗浏览器The Word，搜狗浏览器Avant，腾讯TT，Netspace 8等。
* Gecko
Gecko是一套开源的，以C++编写的网页排版引擎。其代表产品是Mozilla Firefox浏览器。Gecko是最流行的排版引擎之一，仅次于Trident。
* WebKit
Webkit内核代表作品Safari和Chrome浏览器。Chromewebkit 是一个开源项目，包含了来自KDE项目和苹果公司的一些组件，主要用于Mac OS系统，它的特点在于源码结构清晰、渲染速度极快。
* Presto
Presto内核代表作品Opera。Presto是由Opera Software开发的浏览器排版引擎，供Opera 7.0及以上使用。它取代了旧版Opera 4至6版本使用的Elektra排版引擎，包括加入动态功能，例如网页或其部分可随着DOM及Script语法的事件而重新排版。Opera新版本浏览器已经放弃了该内核，改用webkit(blink)核。

* Chromium/Blink
Blink 其实是 WebKit 的分支，如同 WebKit 是 KHTML 的分支。Google 的 Chromium 项目此前一直使用 WebKit(WebCore) 作为渲染引擎，但出于某种原因，并没有将其多进程架构移植入Webkit。Google 决定从 WebKit 衍生出自己的 Blink 引擎（后由 Google 和 Opera Software 共同研发），将在 WebKit 代码的基础上研发更加快速和简约的渲染引擎，并逐步脱离 WebKit 的影响，创造一个完全独立的 Blink 引擎。
### 浏览器内核识别码
* `-webkit-`
代表safari, chrome的内核内核识别码
* `-moz-`:
代表火狐内核识别码
* `-ms-`:
代表IE内核识别码
* `-o-`:
代表opera内核识别码


### 典型的双核浏览器

* 搜狗2.0: Trident内核和webkit内核
* 傲游3.0Beta: Trident和WebKit内核
* QQ浏览器5: Trident内核和Webkit内核
* 360安全浏览器（1.0-5.0为Trident，6.0为Trident+Webkit，7.0为Trident+Blink）
* 360极速浏览器（7.5之前为Trident+Webkit，7.5为Trident+Blink）
* 使用双核浏览器时可以自动/手动来切换内核来浏览网页


## CSS position
参考资料
* [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
* [CSS之Position详解](http://www.cnblogs.com/Zigzag/archive/2009/02/19/position.html)
* [详解CSS position属性](http://luopq.com/2015/11/15/css-position/)


position属性最常用的有4个值
* relative
* absolute
* fixed
* static


```css
/* Keyword values */
position: static;  /*默认值*/
position: relative;
position: absolute;
position: fixed;
position: sticky;

/* Global values */
position: inherit;
position: initial;
position: unset;
```
###  static
position的默认值，一般不设置position属性时，会按照正常的文档流进行排列，块级元素和行内元素按照各自的特性进行显示。

### 相对定位 relative
`position:relative`表示进行相对定位。**相对定位指的是相对其本身的位置进行定位**。设置这个属性后，元素会根据top，left，bottom，right进行偏移，**并且其原本的空间仍然保留。**

```css
#sub1
{
    position: relative;
    padding: 5px;
    top: 5px;
    left: 5px;
}
```
```vbscript-html
<div id="parent">
     <div id="sub1">sub1</div>
     <div id="sub2">sub2</div>
</div>
```
如果不设置relative属性，sub1的位置**按照正常的文档流**，它应该处于某个位置。但当设置sub1为的position为relative后，**将根据top，right，bottom，left的值按照它理应所在的位置进行偏移**，relative的“相对的”意思也正体现于此。

### 绝对定位 absolute
* 误区
很容易误认为，当position属性设为absolute后，总是按照浏览器窗口来进行定位的。其实这是错误的。实际上，这是fixed属性的特点，而不是absolute的特点。

`position:absolute`情况下，元素会脱离文档流，**并且不占据原本的空间**，后面的元素会顶替上去。而且**不论元素是行内元素还是块级元素，都会生成一个块级框，例如，行内元素span设置了absolute后就可以设置height和width属性了**。
`position:absolute`情况下，进行绝对定位，其参考标准分2种情况

* 第1种情况

当sub1的父对象(或曾祖父，只要是父级对象)parent也设置了position属性，且position的属性值为`absolute`或者`relative`或者`fixed`时，也就是说，**不是默认值的情况，此时sub1按照这个parent来进行定位。**

注意，对象虽然确定好了，但有些细节需要您的注意，那就是我们到底以parent的哪个定位点来进行定位呢？如果parent设定了margin，border，padding等属性，那么这个定位点将忽略padding，将会**从padding开始的地方(即只从padding的左上角开始)进行定位**，这与我们会想当然的以为会以margin的左上端开始定位的想法是不同的。

接下来的问题是，sub2的位置到哪里去了呢？由于当position设置为absolute后，会导致sub1溢出正常的文档流，就像它不属于 parent一样，它漂浮了起来，在DreamWeaver中把它称为“层”，其实意思是一样的。此时sub2将获得sub1的位置，它的文档流不再基于sub1，而是直接从parent开始。

* 第2种情况

**如果sub1不存在一个有着position属性的父对象，那么那就会以body为定位对象，按照浏览器的窗口进行定位**，这个比较容易理解。

### 绝对定位 fixed
fixed是特殊的absolute，即fixed总是以body为定位对象的，即按照浏览器的窗口进行定位。


## CSS float
参考资料
* [MDN](https://developer.mozilla.org/zh-CN/docs/CSS/float)
* [详解CSS float属性](http://luopq.com/2015/11/08/CSS-float/)
* [CSS 定位详解](https://www.w3cplus.com/css/advanced-html-css-lesson2-detailed-css-positioning.html)

### 基本介绍
CSS的 float 属性可以使一个元素脱离正常的文档流，然后被安放到它所在容器的的左端或者右端，**并且其他的文本和行内元素环绕它。浮动元素会从普通文档流中脱离，但浮动元素影响的不仅是自己，它会影响周围的元素对齐进行环绕。**

浮动元素是 float 属性值不是 none 的元素。

```css
float: left | right | none | inline-start | inline-end
```

### 浮动元素的特征
* 特征1
浮动元素会从普通文档流中脱离，但浮动元素影响的不仅是自己，它会影响周围的元素对齐进行环绕。

```css
.box { background: #00ff90; padding: 10px; width: 500px; }
.float-ele { float: left; margin: 10px; padding: 10px; background: #ff6a00; width: 100px; text-align: center; }
```

```vbscript-html
<div class="box">
	<span class="float-ele">
	    浮动元素
    </span>
    普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流普通文档流
</div>
```
上述程序的运行效果如下图所示。

![CSS-float-Demo1.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo1.jpg)

由效果图可以看出，span元素周围的文字会围绕着span元素，而设置了float属性的span元素变成了一个块级元素，可以设置width和height属性。

* 特征2
不管一个元素是行内元素还是块级元素，如果被设置了浮动，**那浮动元素会生成一个块级框，可以设置它的width和height**，因此float常常用于制作横向配列的菜单，可以设置大小并且横向排列。

例如，上述程序示例中span被设置成浮动元素之后，可以设置其width和height属性值。

> 术语补充 
> **包含块**：浮动元素的包含块就是离浮动元素最近的块级祖先元素，前面叙述的例子中，div.box就是span元素的包含块。


### 浮动元素的展示规则
浮动元素的展示在不同情况下会有不同的规则，下面我们来一一说明这些规则。
#### 规则1
浮动元素在浮动的时候，其margin不会超过包含块的padding。**即浮动元素的浮动位置不能超过包含块的内边界。**

```vbscript-html
<div class="box">
	<span class="rule1">
	    浮动元素
    </span>
</div>
```

```css
.box { background: #00ff90; padding: 10px; width: 500px; height: 400px; }

.rule1 { float: left; margin: 10px; padding: 10px; background: #ff6a00; width: 100px; text-align: center; }
```

![CSS-float-Demo2.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo2.jpg)

上述例子中，box的padding是10px，浮动元素的margin是10px，合起来为20px，即浮动元素不会超过包含块的padding。**PS：如果想要元素超出，可以设置margin属性。**

####  规则2
如果有多个浮动元素，后面的浮动元素的margin不会超过前面浮动元素的margin。简单说就是如果有多个浮动元素，浮动元素会按顺序排下来而不会发生重叠的现象。

修改上面的示例程序为如下。

```vbscript-html
<div class="box">
	<span class="rule1">
	    浮动元素1
    </span>
    <span class="rule1">
	    浮动元素2
    </span>
    <span class="rule1">
       浮动元素3
    </span>
</div>
```

![CSS-float-Demo3.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo3.jpg)

如图所示，浮动元素会一个一个排序下来而不会发生重叠现象。


####  规则3
如果两个元素一个向左浮动，一个向右浮动，左浮动元素的marginRight不会和右浮动元素的marginLeft相邻。

该规则又分为2种情况。
* 情况1
包含块的宽度大于两个浮动元素的宽度总和

```vbscript-html
<div class="box">
	<span class="rule1">
	    浮动元素1
    </span>
    <span class="rule2">
        浮动元素2
    </span>
</div>
```

```css
.box { background: #00ff90; padding: 10px; width: 500px; height: 400px; }

.rule1 { float: left; margin: 10px; padding: 10px; background: #ff6a00; width: 100px; text-align: center; }

.rule2 { float: right; margin: 10px; padding: 10px; background: #ff6a00; width: 100px; text-align: center; }
```

![CSS-float-Demo4.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo4.jpg)

这种情况很简单，包含块元素的宽度足够大，两个元素一个向左浮动，一个向右浮动，井水不犯河水。

* 情况2
包含块的宽度小于两个浮动元素的宽度总和。


```css
.rule1 { float: left; margin: 10px; padding: 10px; background: #ff6a00; width: 300px; text-align: center; }

.rule2 { float: right; margin: 10px; padding: 10px; background: #ff6a00; width: 300px; text-align: center; }
```

![CSS-float-Demo5.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo5.jpg)

如图所示，如果包含块宽度不够，后面的浮动元素将会向下浮动，其顶端是前面浮动元素的底端。

#### 规则4
浮动元素顶端不会超过包含块的内边界底端，如果有多个浮动元素，下一个浮动元素的顶端不会超过上一个浮动元素的底端。

这条规则简单说就是如果有多个浮动元素，后面的元素高度不会超过前面的元素，并且不会超过包含块。

```html
<div class="box">
	<p>在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，</p>
    <p class="rule1">浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1浮动元素1
    </p>
    <p>在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后</p>
    <p class="rule1">浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2浮动元素2
    </p>
</div>
```

```css
.box { background: #00ff90; padding: 10px; width: 500px; height: 400px; }

.rule1 { float: left; margin: 10px; padding: 10px; background: #ff6a00; width: 250px; text-align: center; }

p { margin-top: 20px; margin-bottom: 20px; }
```

![CSS-float-Demo6.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo6.jpg)

如图所示，两个浮动元素，后面的浮动元素不会超过前面的浮动元素。

#### 规则5
如果有非浮动元素和浮动元素同时存在，并且非浮动元素在前，则浮动元素不会高于非浮动元素。这条规则也是显而易见的，在第4条规则中的例子，浮动元素前有一个非浮动元素p，而浮动元素没有超过它。

#### 规则6
浮动元素会尽可能地向顶端对齐，向左或向右对齐。

在满足其他规则下，浮动元素会尽量向顶端对齐，向左或向右对齐，在第4条规则中的例子，浮动元素会尽可能靠近不浮动的p元素，左侧对齐。


### float特殊情况

#### 浮动元素的延伸性
考虑如下例子，将span元素放在p元素内，并将其高度设置成高于p元素并且左浮动。这个例子的关键在浮动元素高度高于父元素。

```html
<p>
在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，
    <span class="high-float">
	    浮动元素比父级元素高
    </span>
</p>
<p>在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后在浮动元素之后
</p>
```

```css
p { margin-top: 20px; margin-bottom: 20px; background-color: #00ff21; width: 500px; }
.high-float { float: left; height: 80px; line-height: 80px; background-color: orangered; }
```

![CSS-float-Demo8.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo8.jpg)

在这个例子中，浮动元素高度高于父元素，可以看到浮动元素超出了父元素的底端。
这种情况要怎么解决呢，只要将父元素也设置成浮动即可，我们将第一个p元素设置成左浮动，效果如下

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo9.jpg)

将父元素p设置成`float:left`后，**浮动元素就会被包含到父元素里面，我们将这个特性成为浮动元素的延伸性**。

**可以将浮动元素的延伸性理解为元素被设置成浮动后，该元素会进行延伸进而包含其所有浮动的子元素。**

#### 浮动元素超出父级元素的padding
在前面提到的第一条规则：浮动元素的外边界不会超过父级元素的内边界。大部分情况下，我们见到的场景都是符合的。但是有一些特殊情况。
* 情况1： 负margin
如下，将浮动元素的margin-left设置成负数。

```html
<p>
在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，
	<span class="minus-margin">
        负margin-left
    </span>
</p>
```

```css
p { margin-top: 20px; margin-bottom: 20px; margin-left: 50px; background-color: #00ff21; width: 500px; }
.minus-margin { float: left; height: 80px; line-height: 80px; background-color: orangered; margin-left: -20px; }
```

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo10.jpg)

将margin-left设置成负数之后，浮动的子元素明显超出了父元素的内边界，这难道不是违反了第一条规则吗？

但仔细想想，这其实是合理的，因为默认情况下marign-left就是0，所以不会超出父元素的内边界，但是将其设置成负数之后，就相当于浮动元素覆盖了自己的内边界一样。

我们在从数学的角度来看看这个问题，这个例子中,

父元素的margin-left:50px，padding和border默认为0，即内边界所在距离浏览器左侧的位置为50px。

浮动的子元素默认情况下距离浏览器左侧的像素应该为50px，但是将其设置成margin-left:-20px后，浏览器会进行计算：

50px+（-20px）margin+0border+0padding=30px。

距离浏览器左侧更近，所以超出了父元素。

#### 浮动元素宽度大于父级元素宽度
如果我们将浮动元素的宽度设置大于父级元素，效果会如何呢？
元素左浮动，width大于父级元素

```html
<p>
在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，
    <span class="big-width">
        width大于父级元素
    </span>
</p>
```
```css
p { margin-top: 20px; margin-bottom: 20px; margin-left: 50px; background-color: #00ff21; width: 250px; }
.big-width { float: left; height: 80px; line-height: 80px; background-color: orangered; width: 300px; }
```

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo11.jpg)

将浮动元素左浮动，并且宽度超出父级元素时，由于浮动元素宽度较大，它会超过父级元素的右内边界。

如果将其设置成右浮动，情况又会怎么样呢？

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo12.jpg)

可以看到，设置成右浮动后，会超出父级元素的左内边界。

#### 重叠问题
重叠问题是指两个元素在同一个位置，会出现上下重叠的问题。浮动元素如果和普通文档流发生重叠会怎么样呢？

首先浮动元素要怎么样才会发生重叠呢，设置其margin-top为负数即可。我们看看例子

```html
<p>
	<span>
    在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前。
    </span>
    <span class="overlay">
	    浮动元素重叠
    </span>
</p>
```
```css
p { margin-top: 20px; margin-bottom: 20px; margin-left: 50px; background-color: #00ff21; width: 250px; }
.overlay { float: left; height: 80px; line-height: 80px; background-color: orangered; width: 300px; margin-top: -30px; }
```
![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo13.jpg)

如果浮动元素不设置负margin时，是这样的

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo14.jpg)

在这个例子中，可以看到p中正常流元素span的内容会显示在浮动元素上面。

设置span背景图片试试，效果如下

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo15.jpg)

**元素设置背景后，重叠的部分还是会显示背景**。


如果是span标签换成div标签会怎么样呢？

```html
<p>
	<div style="background-image:url(../images/banner1.jpg)">
    在浮动元素之前在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前，在浮动元素之前。
    </div>
    <span class="overlay">
         浮动元素重叠
     </span>
</p>
```

![CSS-float-Demo9.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo16.jpg)

这种情况下，重叠的部分不会显示背景图片。

**总结**一下这两种情况的区别
1.  行内元素与浮动元素发生重叠，其边框，背景和内容都会显示在浮动元素之上
2.  块级元素与浮动元素发生重叠时，边框和背景会显示在浮动元素之下，内容会显示在浮动元素之上

### clear属性
参考资料
* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)
* [详解CSS float属性](http://luopq.com/2015/11/08/CSS-float/)


clear CSS 属性指定一个元素是否可以在它之前的浮动元素旁边，或者必须向下移动(清除浮动) 在它的下面。clear 属性适用于浮动和非浮动元素。

clear属性可以指定当前元素的左右两侧是否会有浮动元素。


```html
<div class="box">
	<div class="float">浮动元素1</div>
    <div class="float">浮动元素2</div>
    <div class="float">浮动元素3</div>
</div>
```
```css
.float { float: left; width: 150px; background: #0094ff; border: 1px solid red; margin-left: 5px; }
.cl { clear: left; }
.cr { clear: right; }
.cb { clear: both; }
```

![CSS-float-Demo17.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo17.jpg)

如图所示，上述程序创建了3个浮动的div元素。

下面对程序进行修改，希望第二个浮动元素另起一行进行浮动，那该怎么做呢？

根据**clear属性的定义：确保当前元素的左右两侧不会有浮动元素。**我们对第一个浮动元素添加`clear:right`保证其右侧不会有浮动元素。修改HTML代码如下

```html
div class="box">
    <div class="float cr">浮动元素1</div>
    <div class="float">浮动元素2</div>
    <div class="float">浮动元素3</div>
</div>
```
修改之后，并没有任何效果，其效果图如下所示。**因为这种修改方法是错误的！！**

![CSS-float-Demo17.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo17.jpg)


正确的修改为给第二个元素添加`clear:left`，保证其左侧不会出现浮动元素。

```html
div class="box">
    <div class="float">浮动元素1</div>
    <div class="float cl">浮动元素2</div>
    <div class="float">浮动元素3</div>
</div>
```
效果如下图所示，达到了修改目的。

![CSS-float-Demo18.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo18.jpg)


对比上述2个修改示例，得出如下**结论**
* **clear只对元素本身的布局起作用。**
* 在上述例子中，第一个浮动元素添加了clear属性，它并不会影响到其他元素的布局，只会影响自己，所以第二个浮动元素并不会另起一行。
* 而第二个浮动元素设置了clear后，我们可以看到clear作用于自己，所以元素另起一行。

### 清除浮动

#### 为什么要清楚浮动——高度塌陷
一个块级元素如果没有设置height，其height是由子元素撑开的。**对子元素使用了浮动之后，子元素会脱离标准文档流**，也就是说，父级元素中没有内容可以撑开其高度，这样父级元素的height就会被忽略，这就是所谓的**高度塌陷**。

要解决这样的问题，就要使用清除浮动。


清除浮动有很多方法，下面我们一一说明每一种方法。

#### 方法1  IE浏览器
对于IE浏览器来说，要清除浮动带来的问题，只需要触发器`hasLayout`就可以，直接设置样式`zoom:1`就可以触发。

下面几种方法主要针对非IE浏览器中的清楚浮动。

#### 方法2  增加额外的div

这是最简单直接的方法，哪里有浮动元素，就在其父级元素的内容中增加一个div（作为最后一个子元素），这样就会清除浮动元素带来的问题。

```html
<div style="clear:both"></div>
```

* 优点
简单直接，初学者常常使用的方法，也易于理解
* 缺点
增加额外的无意义标签，不利于语义化，每次清除都要添加额外的空标签，造成浪费。

#### 方法3  父级元素添加overflow:hidden
这个方法的关键在于触发了BFC。
```css
.clearfix{overflow:hidden;zoom:1}
```
* 优点
代码量少，没有额外的标签。
* 缺点
如果子元素超出父元素的范围，会造成超出的部分被隐藏。


#### 方法4  after伪元素
after表示子元素的后面，通过它可以设置一个具有clear的元素，然后将其隐藏

```css
clearfix:{
    zoom:1
}
clearfix:after{
    display:block; 
    content:''; 
    clear:both; 
    height:0; 
    visibility:hidden;
}
```

这种方法的原理和第一个方法一样，就是生成一个元素来清除浮动，只是这个元素是“假的”。
* 优点
没有额外标签，综合起来算是比较好的方法
* 缺点：
稍显复杂，但是理解其原理后也挺简单的。

**推荐使用这种方法。**

### float的应用
#### 文字环绕效果
float最初的应用就是文字环绕效果，这对图文并茂的文章很有用。

```html
<div class="box">
	<img src="1.jpg" class="float" />
    我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字我是环绕的文字
</div>
```

```css
.box { background: #00ff90; padding: 10px; width: 500px; }  
.float {background: #0094ff none repeat scroll 0 0;border: 1px solid red;float: left;margin-left: 5px;width: 400px;}
```

![CSS-float-Demo19.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo19.jpg)


#### 横向菜单排列
使用`display:inline`可以实现横向排列菜单。使用float属性也可以实现横向排列的菜单。

```html
<ul class="menu clearfix">
	<li>首页</li>
    <li>政治</li>
    <li>娱乐</li>
    <li>体育</li>
    <li>游戏</li>
</ul>
```

```css
.clearfix: { zoom: 1; }
.clearfix:after { display: block; content: ''; clear: both; height: 0; visibility: hidden; }
.menu { background: #0094ff; width: 500px; }
.menu li { float: left; width: 100px; list-style-type: none; }
```

![CSS-float-Demo19.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo20.jpg)

这种方式可以很轻松的实现横向菜单效果，但需要注意的是要注意清除浮动，推荐使用display:inline-block来实现。

#### 布局
float最经常使用的场景就是布局。使用float可以很简单的实现布局，而且易于理解。下面来实现一个三栏两列的固定布局。

```html
<div class="header">
    我是头部
</div>
<div class="content clearfix">
    <div class="side">左侧</div>
    <div class="main">
        右侧
    </div>
</div>
<div class="footer">
    我是底部
</div>
```

```css
.clearfix: { zoom: 1; }
    .clearfix:after { display: block; content: ''; clear: both; height: 0; visibility: hidden; }
.header, .footer { height: 50px; background: #0094ff; text-align: center; }
.content { margin: 0 auto; width: 1000px; background: #000000; }
.side { float: left; height: 500px; width: 280px; background: #ff006e; }
.main { float: right; height: 500px; width: 700px; background: #fbcfcf; }
```
![CSS-float-Demo21.jpg](http://ol3kbaay9.bkt.clouddn.com/CSS-float-Demo21.jpg)


### 总结
float属性是一个频繁用到的属性，要用好它就要理解它的特性，否则容易云里雾里搞不清楚状况。关于float，最重要的是要理解下面几点
1. float会造成元素脱离文档流
2. float影响元素的几个规则
3. 浮动带来的问题以及如何清除浮动


## CSS display:flex
参考资料
* [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display)
* [CSS 弹性盒子盒子 MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)
* [Flex | 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
* [Flex布局实例](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
* [flexbox](http://zh.learnlayout.com/flexbox.html)
* [三分钟学会css3中的flexbox布局](http://www.webhek.com/post/css-flexbox-layout.html)


Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

CSS3 弹性盒子(Flexible Box 或 Flexbox)，是一种用于在页面上布置元素的布局模式，使得当页面布局必须适应不同的屏幕尺寸和不同的显示设备时，元素可预测地运行。对于许多应用程序，弹性盒子模型提供了对块模型的改进，因为它不使用浮动，flex容器的边缘也不会与其内容的边缘折叠。



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 