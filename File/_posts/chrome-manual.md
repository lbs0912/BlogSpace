---
title: Chrome控制台实用指南
date: 2017-02-26 16:35:26
categories: Front-end
tags: [Front-end,Chrome]
keywords: Front-end,Chrome
toc: true
---


* 本文主要记录Chrome控制台使用的一些技巧。
* [ Chrome - Console API Reference](https://developers.google.com/web/tools/chrome-devtools/console/console-reference?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3#consolelogobject-object)
* [ Chrome - Command Line API Reference](https://developers.google.com/web/tools/chrome-devtools/console/command-line-reference?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3#debugfunction)
* [15 Must-Know Chrome DevTools Tips and Tricks](http://tutorialzine.com/2015/03/15-must-know-chrome-devtools-tips-tricks/)

<!--more-->

##  清空控制台

* `console.log()`: 清空控制台



##  信息输出分类


* `console.log()`: 控制台输出一般性信息。
* `console.info()`: 控制台输出提示信息。
* `console.error()`: 控制台输出错误信息。
* `console.warn()`: 控制台输出警告信息。

``` javascript
console.log("Normal Information");
console.info("Tip Information");
console.error("Error Information");
console.warn("Warn Information");
```

`console.log()`,`console.info()`,`console.error()`和`console.warn()`可以对控制台输出的信息进行分类，提高程序调试效率。运行上述程序，在Chrome控制台中输出的信息如下图所示。

![enter image description here](http://oda53d5ps.bkt.clouddn.com/Chrome-Console.PNG)


##  信息输出分组

* `console.group("groupName")` :  创建输出信息分组
* `console.groupEnd()`:    结束信息分组
* Example



``` javascript
console.group("Group 1");
	console.log("Normal Information from Group 1");
	console.info("Tip Information from Group 1");
console.groupEnd();

console.group("Group 2");
	console.error("Error Information from Group 2");
	console.warn("Warn Information from Group 2");
console.groupEnd();

```



运行上述程序，在Chrome控制台中输出的信息如下图所示。

![Chrome-Console-2.PNG](http://oda53d5ps.bkt.clouddn.com/Chrome-Console-2.PNG)



##  修饰输出信息的样式

`console.log()`,`console.info()`,`console.error()`和`console.warn()`这4个函数都能接收多个参数，参数之间以逗号隔开。控制台输出的信息将以空格隔开。

``` javascript
console.log(element1,element2,element3,element4);
console.log("%c element1","style setting",element3,element4);
```

其中，第一个参数可以添加格式化指令`%c`，其后的参数表示参数1的样式设置。格式化指令仅对参数1起作用，其余参数添加格式化指令后不会起到样式修改的作用。

``` javascript
console.log("%chello world", "background-image:-webkit-gradient( linear, left top, right top, color-stop(0, #f22), color-stop(0.15, #f2f), color-stop(0.3, #22f), color-stop(0.45, #2ff), color-stop(0.6, #2f2),color-stop(0.75, #2f2), color-stop(0.9, #ff2), color-stop(1, #f22) );color:transparent;-webkit-background-clip: text;font-size:5em;");
console.info("%cLiu Baoshuai","color:blue;font-size:23px;");
console.warn("%cChange Style","color:red;","%cwarnInformation","font-size:30px;");
```

运行上述程序，在Chrome控制台中输出的信息如下图所
示。

![enter image description here](http://oda53d5ps.bkt.clouddn.com/Chrome-Console-4.PNG)

## 常用的Chrome开发技巧
参考[15个关于Chrome的开发必备小技巧](http://www.toutiao.com/i6342610600517960193/)和[15 Must-Know Chrome DevTools Tips and Tricks](http://tutorialzine.com/2015/03/15-must-know-chrome-devtools-tips-tricks/)了解更多。
 
### 快读查找文件
* Windows： `Ctrl`+`P`
* Mac OS： `Cmd`+`P`

### 在源代码中搜索
在整个工程下搜索一段代码 
* Windows： `Ctrl`+`Shift`+`F`
* Mac OS： `Cmd`+`Opt`+`F`
* 该搜索支持正则表达式

### 跳到指定行
在chome调试器的sources栏目中，跳转到指定行
* Windows： `Ctrl`+`G`
* Mac OS： `Cmd`+`L`
* 输入指定行号即可（ `Ctrl`+`G`命令会自动输出`：·`，开发者只需输如行号即可）
* 也可以使用`Ctrl`+`O`，但是需要手动输入`：·`和指定的行号

### 获取DOM元素
Chrome控制台，提供了方法和变量来快速获取页面中的DOM元素
* `$()`
 `$()`是`document.querySelector()`原生方法的映射，可以获取并返回第一个与填写的属性匹配的DOM元素，如`$('div')`就会返回第一个出现在页面中的div元素。
* `$$()`
`$$()`是`document.querySelectorAll()`原生方法的映射，可以获取并返回一个数组，数组中包含了所有与你填写的属性匹配的DOM元素。
* `$0--$4`
代表在Chrome调试器中操作不同DOM元素的历史记录，且最多记录5次。`$0`代表最近一次，依次类推。

### 多光标
对js脚本文件或其他文件进行多处操作
* Windows： `Ctrl`，然后鼠标点击即，即可出现多处光标
* Mac OS： `Cmd`，然后鼠标点击即，即可出现多处光标

### 保存日志
* 在console面板上勾选`Preserve log`选项，则不会在每次加载页面时，清空日志
* 若想要调试页面关闭前的bugs时，可以勾选该选项

![chrome-preserve-log.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-preserve-log.gif)

### 格式化代码
Chrome通过其内置的优化器，帮助开发者提高文件内容的可读性。对于压缩过或者杂乱的代码，尤为适用。
* F12打开Chrome开发工具
* 选择想要阅读的文件
* 点击文件下方的`{}`状按钮即可

![chrome-format.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-format.gif)


### 设备模拟器
Chome中可以模拟移动设备端，例如，可以通过Chrome模拟人为触摸屏幕和晃动设备，甚至可以通过它改变地理位置。
* F12打开Chrome调试器
* 在调试器底部选中Emulation选项
* 在Emulation面板中，左侧选中Sensors即可

![chrome-emulation.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-emulation.gif)

### 颜色选择器
调用Chrome的颜色选择器后，可以通过鼠标，悬浮在网页中的任意处，获取颜色。并且颜色选择器会十分精准地将其转换成对应的编码格式。
* F12打开Chrome调试器
* 选中目标元素
* 在样式编辑器中，点击颜色预览，就会出现这个颜色选择器

![chrome-colorpicker.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-colorpicker.gif)

### 强制改变元素状态
Chrome开发工具有一个强制改变元素CSS状态的功能，如:hover和:focus。对于CSSer比较方便。

![chrome-csschange.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-csschange.gif)

### 可视化"隐藏的DOM"
Web浏览器在构建例如textbox，button以及input这些元素时，通常会隐藏在其之下的展现层元素。但是，可以通过Setting a General，在General面板中选中"Show user agent shadow DOM"这一选项，来展示这些被隐藏掉的基础元素，甚至可以单独地去设置它们的样式。

![chrome-virtual-dom.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-virtual-dom.gif)


### 选中下一个匹配项
当选中一个匹配项后，利用`Ctrl+D`（Mac上`Cmd+D`），就会将下一个相同的匹配项也选中，该功能可以帮助开发者同时编辑它们。

![chrome-multi-select.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-multi-select.gif)


### 改变颜色格式
在颜色预览中，通过`Shift` + 鼠标点击，就可以在`rgba`，`hsl`和`hexadecimal`三种格式中，来回切换。

![chrome-color-change.gif](http://ol3kbaay9.bkt.clouddn.com/chrome-color-change.gif)

### 利用Chrome工作空间编辑本地文件
利用Chrome的工作空间，可以直接编辑和保存本地文件，无需额外的操作，例如复制、粘贴。
* F12打开Chrome调试器
* 找到Sources栏，出现在左侧的控制面板，点击鼠标右键，选择`Add Folder To Workspace`。或者，直接将整个工程文件夹，拖拽到调试器中
* 这样操作后，不管打开哪个页面，都会出现刚才操作的文件
* 可以将页面中用到的文件映射到相应的文件夹，允许在线编辑和简单的保存

## Reference
[1] [Chrome控制台实用指南-知乎](https://zhuanlan.zhihu.com/p/24187505)
[2] [15个关于Chrome的开发必备小技巧](http://www.toutiao.com/i6342610600517960193/)
[3] [15 Must-Know Chrome DevTools Tips and Tricks](http://tutorialzine.com/2015/03/15-must-know-chrome-devtools-tips-tricks/)


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
