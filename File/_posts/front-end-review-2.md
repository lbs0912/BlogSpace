---
title: 前端面试复习-2
date: 2017-04-14 11:35:48
categories: Front-end
tags: [Front-end]
keywords: 前端面试复习
---


* 本文主要对前端面试中常考察的知识点进行记录和归纳。


<!--more-->

## forEach
参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)了解更多。

forEach 方法为数组中含有效值的每一项执行一次callback 函数，那些已删除（使用delete方法等情况）或者未初始化的项将被跳过（但不包括那些值为 undefined 的项）（例如在稀疏数组上）。

**forEach循环中，无法使用break命令或return命令跳出循环。**

callback 函数会被依次传入三个参数
1. 数组当前项的值 element
2. 数组当前项的索引 index
3. 数组对象本身 array

```javascript
//syntax
array.forEach(callback(currentValue, index, array){
    //do something
}, this)

array.forEach(callback[, thisArg])
```

示例程序如下，求数组的和。

```javascript
var arr = [1,4,2,3];
function sum(arr) {
	var s = 0;
	arr.forEach(function(val, idx, arr) {
		s += val;
	});
	return s;
};
console.log(sum(arr));
```

## JS数组基础操作
### 数组求和

* 方法1： 常规for循环

* 方法2： forEach遍历

```
var arr = [1,4,2,3];
function sum(arr) {
	var s = 0;
	arr.forEach(function(val, idx, arr) {
		s += val;
	});
	return s;
};
console.log(sum(arr));
```



* 方法3：reduce归并

```javascript
function sum(arr) {
	return arr.reduce(function(prev, curr, idx, arr){
		return prev + curr;
	});
}
```

* 方法4：eval()与join()

```javascript
function sum(arr) {
	return eval(arr.join("+"));
};
```
### 移除数组中所有与item相等的元素 

* 方法1：不改变原数组

常规方法中，采用for循环实现。

```javascript
//创建新数组，push()
function remove(arr, item) {
	var result = [];
	for(var i = 0; i < arr.length; i++){
		if(arr[i] != item){
			result.push(arr[i]);
		}
	}
	return result;
}
```

也可以借助`filter`实现。

> The `filter()` method **creates a new array** with all elements that pass the test implemented by the provided function.    --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)


```javascript
function remove(arr, item) {
	return arr.filter(function(element){
		return element != item;
	});
}
```

* 方法2：直接修改原数组，借助splice实现

```javascript
function removeWithoutCopy(arr, item) {
	for(var i = 0; i < arr.length; i++){
		if(arr[i] == item){
			arr.splice(i,1);
			i--;
		}
	}
	return arr;
}
```

### 在数组指定位置添加元素(不改变原数组)
#### 在arr头部添加item
* 方法1: concat直接拼接

concat() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```javascript
function prepend(arr, item) {
	return [item].concat(arr);
}
```

* 方法2： 利用`push.apply()`实现

push() 方法将一个或多个**元素**添加到数组的末尾，并返回数组的新长度。注意，直接调用push方法时，参数是元素，而不能是数组。如果传入数组，则会将数组本身作为一个整体添加到新的数组末尾。

如下示例程序中，`[10].push(arr)`会将arr数组作为一个整体插入新的数组中，返回结果是`[10,[1,4,2,3]]`，是一个二维数组。

```javascript
var arr = [1,4,2,3];
function prepend(arr, item) {
	var newArr=[item];
	newArr.push(arr);
	return newArr;
}
var newArr = prepend(arr,10);
console.log(newArr);   //[10,[1,4,2,3]]
```

因此，当合并两个数组时，不能直接调用push方法，而应该同时使用`push()`和`apply()`实现。示例程序如下。参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)。

```javascript
var vegetables = ['parsnip', 'potato'];
var moreVegs = ['celery', 'beetroot'];

// 将第二个数组融合进第一个数组
// 相当于 vegetables.push('celery', 'beetroot');
Array.prototype.push.apply(vegetables, moreVegs);

console.log(vegetables); 
// ['parsnip', 'potato', 'celery', 'beetroot']
```

针对本题情况，实现代码如下。

```javascript
function prepend(arr, item) {
	var newArr=[item];
	Array.prototype.push.apply(newArr,arr);
	//或者
	//[].push.apply(newArr, arr);
	return newArr;
}
```

* 方法3： 利用`unshift()`或`slice()`或`splice()`实现

unshift() 方法将一个或多个元素添加到数组的开头，并返回新数组的长度。

```javascript
function prepend(arr, item) {
	var result = arr.slice(0);
	//or
	//var result = arr.concat();

	result.splice(0,0,item);
	//or
	//result.unshift(item); 
	
	return result;
}
```

#### 在arr尾部添加item

* 方法1： 普通迭代拷贝

```javascript
var append = function(arr, item) {
	var newArr = [];
	for (var i = 0; i < arr.length; i++) {
		newArr.push(arr[i]);
	}
	newArr.push(item);
	return newArr;
};
```

* 方法2： 利用`slice+push`实现

```javascript
// 使用slice浅拷贝+push组合
var append2 = function(arr, item) {
	var newArr = arr.slice(0); // slice(start, end)浅拷贝数组
	newArr.push(item);
	return newArr;
};
```

* 方法3： 利用`concat()`实现

```javascript
var append3 = function(arr, item) {
	return arr.concat(item);
};
```



#### 在arr指定index处添加item
* 方法1： 利用`splice(index,0,item)`实现

```javascript
function insert(arr, item, index) {
	var newArr=arr.concat(); 
	//var newArr=arr.slice(0);
    //var newArr=[]; [].push.apply(newArr, arr);
	
	newArr.splice(index,0,item);
	return newArr;
}
```

* 方法2： 利用`slice+concat`实现

```javascript
//利用slice+concat
function insert(arr, item, index) {
	return arr.slice(0,index).concat(item,arr.slice(index));
}

```


### 在数组指定位置删除元素(不改变原数组)
#### 在arr开头删除元素
* 方法1：使用slice

```javascript
//利用slice
function curtail(arr) {
	return arr.slice(1);
}
```

* 方法2：使用filter

```javascript
//利用filter
function curtail(arr) {
	return arr.filter(function(ele,index,arr) {
		return i!==0;
	});
}
```

* 方法3：迭代复制

shift() 方法从数组中删除第一个元素，**并返回该元素的值**。此方法更改数组的长度。
unshift() 方法将一个或多个元素添加到数组的开头，**并返回新数组的长度**。

```javascript
function curtail(arr) {
	var newArr = arr.concat(); 
	//var newArr = arr.slice(0);
	//var newArr=[]; [].push.apply(newArr, arr);
	
	newArr.shift();
	return newArr;
}
```

#### 在arr末尾删除元素
使用slice实现

```javascript
function truncate(arr) {
	return arr.slice(0,arr.length-1);
}
```


### 合并数组(不改变原数组)

* 方法1：利用concat

```javascript
function concat(arr1, arr2) {
	return arr1.concat(arr2);
}
```

* 方法2：利用push.apply()

```javascript
function concat(arr1, arr2) {
	var newArr=arr1.slice(0);
	[].push.apply(newArr, arr2);
	return newArr;
}
```

### 统计数组中元素出现次数
* 方法1： 常规循环
 

```javascript
function count(arr, item) {
	var sum = 0;
	for(var i = 0; i < arr.length; i++){
		if( arr[i] == item){
			sum ++;
		}
	}
	return sum;
}
```
* 方法2：利用filter()

```javascript
function count(arr, item) {
	return arr.filter(function(ele){
		return (ele==item);
	}).length;
}
```

* 方法3：利用forEach()

```javascript
function count(arr, item) {
	var count = 0;
	arr.forEach(function(ele) {
		ele === item ? count++ : 0;
	});
	return count;
}
```
* 方法3：利用reduce()

```javascript
function count(arr, item) {
	var count = arr.reduce(function(prev, curr) {
		return curr === item ? prev+1 : prev;
	}, 0);
	return count;
}
```


### 输出数组中重复出现的元素
* 方法1：双循环

```javascript
function duplicates(arr) {
	var res = [];
	var flag;
	for(var i=0;i<arr.length;i++){
		flag = false;
		for(var j=i+1;j<arr.length;j++){
			if(arr[i]==arr[j]){
				arr.splice(j,1);
				j=j-1;
				flag = true;
			}
		}
		if(flag == true){
			res.push(arr[i]);
		}
	}
	return res;
}
```

* 方法2：先排序，再循环

```javascript
function duplicates(arr) {
	var copy = arr.sort(),
	result = [];
	for(var i in copy){
		if(copy[i]==copy[i-1] && 
		     result.indexOf(copy[i])==-1){              
		          result.push(copy[i]);
		}
    }
    return result;
}
```

* 方法3：利用forEach()

**查询元素elem，若正向查找索引值和反向查找索引值不相等，则说明该元素重复出现了。**

```javascript
function duplicates(arr) {
	var result = [];
	arr.forEach(function(elem){
	if(arr.indexOf(elem) != arr.lastIndexOf(elem) &&  
		result.indexOf(elem) == -1){
			result.push(elem);
		}
	});
	return result;
}
```

### 数组去重

注意判断数组中的NaN元素。因为`NaN==NaN`为false。

```javascript
Array.prototype.uniq = function () {
	var flag = false;
	for(var i = 0; i < this.length; i++){
		//判断是否为NaN
		if(this[i] !== this[i]){
			//NaN == NaN 为false
			flag = true;
		}
		for(var j = i+1; j < this.length;){
			//数字重复或者出现NaN
			if(this[i] === this[j] ||(flag && this[j] !== this[j])){
				this.splice(j,1);
			}
			else{
				j++;
			}
		}
	}
	return this;
}
```

## Array.prototype.reduce()
参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)了解更多。

```javascript
arr.reduce(callback,[initialValue])
```
**注意: 不提供 initialValue ，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。提供 initialValue ，从索引0开始。**

数组求和示例。

```javascript
let sum = [0, 1, 2, 3].reduce(function(acc, val) {
  return acc + val;
}, 0);

console.log(sum);
// 6
```


数组扁平化示例。

```javascript
let list1 = [[0, 1], [2, 3], [4, 5]];

let list2 = [0, [1, [2, [3, [4, [5, [6]]]]]]];

const flatten = (arr) => {
    return arr.reduce(
        (acc, val) => {
            return acc.concat(Array.isArray(val) ? flatten(val) : val)
        }, []
    );
};

flatten(list1); 
// [0, 1, 2, 3, 4, 5]

flatten(list2); 
// [ 0, 1, 2, 3, 4, 5, 6 ]
```

计算数组中各个值出现次数示例。

```javascript
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function(allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});

// countedNames is 
//{ 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

## on事件 V.S. addEvent事件

参考资料
* [javascript 同一事件多次绑定问题详解](http://blog.csdn.net/wyqlxy/article/details/8985871)
* [on事件和addevent事件的区别](http://www.jianshu.com/p/4deaa313178d)


### on事件的覆盖

使用on事件，给同一个标签添加多个事件的时候，存在事件覆盖问题。

#### HTML标签中绑定on事件

在HTML标签中直接绑定on事件，前面的on事件会覆盖后面的on事件，最终效果为，点击元素，只会执行最前面的on事件。

```vbscript-html
<a id="a1"  onclick="alert('a1')" onclick="alert('a2')" >
	test4
</a>
```
后面的on事件被最前面的on事件覆盖。点击a元素，只会输出`a1`。

####  JS中绑定on事件
在JS中绑定on事件，后面的on事件会覆盖前面的on事件，最终效果为，点击元素，只会执行最后面的on事件。

```vbscript-html
<button id="click">Click me</button>
<p id="demo">Demo</p>
```

```javascript
function myFunction1() {
	document.getElementById("demo").innerHTML = "Hello World";
}
function myFunction2() {
    document.getElementById("demo").innerHTML = "Liu Baoshuai";
}
var btn = document.getElementById("click");
btn.onclick = myFunction1;
btn.onclick = myFunction2;
```

点击button按钮，只会执行最后绑定的on事件函数，即myFunction2函数，p标签的内容被替换为Liu Baoshuai。

注意，函数名后面带括号表示立即执行，如下程序中，页面加载后，onclick事件会立即执行，p标签的内容被替换为Liu Baoshuai。

```javascript
btn.onclick = myFunction1()
btn.onclick = myFunction2();
```

再如下述程序，点击页面，只会执行`alert(1)`。
```javascript
function fn1(){alert(1)};
function fn2(){alert(2)};
document.onclick=fn1();  //函数立即执行
document.onclick=fn2();  //函数立即执行
```

如果将上述程序修改为如下（函数名后加上括号表示立即执行），则在页面加载时，会执行`alert(1)`和`alert(2)`。但是这2个事件并不是onclick触发的，只是函数立即执行的效果。

```javascript
function fn1(){alert(1)};
function fn2(){alert(2)};
document.onclick=fn1;
document.onclick=fn2;
```

#### jQuery中绑定click事件
jQuery中使用bind绑定click事件或者直接绑定click事件，每个click事件都会执行，类似于c#的多播处理，不存在事件覆盖问题。

```javascript
$("#a2").click(function(){
	alert('a2');
});

$("#a2").click(function(){
	alert('b2');
});

$("#a3").bind("click",function(){
	alert('a3');
});

$("#a3").bind("click",function(){
	alert('b3');
});
```
### addEvent事件
> addEventListener()和attachEvent()效果类似。但是，attachEvent是仅在IE下是可行的，addEventListener是在其它遵循W3C标准的浏览器中可行的。在IE9和之后的版本中也可以使用addEventListener。

addEvent事件监听可以给一个标签添加多个事件，并且**事件之间不存在覆盖问题。每个事件都会被执行。**


```javascript
function myFunction1() {
	alert("myFunction1");
	document.getElementById("demo").innerHTML = "Hello World";
}
function myFunction2() {
	alert("myFunction2");
    document.getElementById("demo").innerHTML = "Liu Baoshuai";
}
var btn = document.getElementById("click");
btn.addEventListener("click",myFunction1);
btn.addEventListener("click",myFunction2);
```
上述程序中，点击按钮，先执行myFunction1事件，再执行myFunction2事件。


```javascript
element.addEventListener(event, function, useCapture)
```
addEventListener()可以接收3个参数
* 第1个参数：event
事件类型
* 第2个参数：function
回调函数
* 第3个参数：useCapture，bool类型
默认值为false。若该参数为false，事件的触发机制就会按照冒泡(从下往上)。若该参数是true，就会按照事件捕获，从上往下。

attachEvent事件绑定和addEventListener事件绑定不带第三个参数的情形下，都是冒泡型事件处理方式

关于事件冒泡和事件捕获，请参考[JS中事件冒泡与捕获](https://segmentfault.com/a/1190000005654451)。


**当事件捕获和事件冒泡一起存在的情况，事件触发顺序如下**
* 第1步：document 往 target节点，**捕获前进，遇到注册的捕获事件立即触发执行**
* 第2步：到达target节点，触发事件。对于target节点上，是先捕获还是先冒泡，则由捕获事件和冒泡事件的注册顺序决定，先注册先执行。
* 第3步：target节点往 document 方向，**冒泡前进，遇到注册的冒泡事件立即触发**

**总结**上述步骤为
* 对于非target节点则**先执行捕获在执行冒泡**
* 对于target节点则是先执行先注册的事件，无论冒泡还是捕获

```html
<style>
.div1{
	width: 300px;
	height: 300px;
	background: red;
	margin: 100px auto;  
}
.div2{
	width: 200px;
	height: 200px;
	background: blue;
}
.div3{
	width: 100px;
	height: 100px;
	background: green;
}
</style>

<div class="div1">
	<div class="div2">
		<div class="div3"></div>
    </div>
</div>

<script>
window.onload=function(){
	var div1 = document.getElementsByClassName("div1")[0];
	var div2 = document.getElementsByClassName("div2")[0];
	var div3 = document.getElementsByClassName("div3")[0];
	
	div1.addEventListener("click",function(){
         alert(1);
    } ,false); //事件冒泡
    div1.addEventListener("click",function(){
         alert(2)
    } ,true); //事件捕获
    div3.addEventListener("click",function(){
         alert(3)
    } ,false);//事件冒泡

}
</script>
```

上述程序中，给div1绑定2个点击事件。给div3绑定1个点击事件。
* 点击div1元素，会依次弹出 1 2
* 点击div2元素，会依次弹出 2 1
* 点击div3元素，会依次弹出 2 3 1

点击div1元素，从document到div1，没有触发捕获事件。到达div1元素后，按照事件注册的先后顺序触发，因此，会依次弹出1 2。再从div1到document，没有触发冒泡事件。

点击div2元素，从document到div1，没有触发捕获事件。到达div1元素后，按照事件注册的先后顺序触发，因此，会依次弹出1 2。再从div1到document，没有触发冒泡事件。

点击div1元素，从document到div2，经过div1时，触发捕获事件，输出2。到达div2元素后，由于div2没有绑定事件，则再从div2到document，经过div1时，触发冒泡事件，输出1。

点击div3元素，从document到div3，经过div1时，触发捕获事件，输出2。到达div3元素后，触发绑定的事件，输出3。再从div3到document，经过div1时，触发冒泡事件，输出1。因此，最终输出为2 3 1。

## void(0)用法
参考资料
* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/void)
* [js中javascript:void(0)用法详解](http://m.jb51.net/article/70916.htm)
* [菜鸟笔记](http://www.runoob.com/js/js-void.html)


`void`运算符对给定的表达式进行求值，然后返回 `undefined`。void 运算符通常只用于获取 undefined 的原始值，一般使用 `void(0)`（等同于 `void 0`）。其语法格式如下

```javascript
void expression
```

### 用法1 - 立即调用的函数表达式
在使用立即执行的函数表达式时，可以利用 void 运算符让 JavaScript 引擎把一个函数识别成函数表达式而不是函数声明（语句）。

```javascript
void function iife() {
    var bar = function () {};
    var baz = function () {};
    var foo = function () {
        bar();
        baz();
     };
    var biz = function () {};
    foo();
    biz();
}();
```

### 用法2 - JavaScript URIs
当用户点击一个以 `javascript: URI` 时，浏览器会对冒号后面的代码进行求值，然后把求值的结果显示在页面上，这时页面基本上是一大片空白，这通常不是我们想要的。只有当这段代码的求值结果是 `undefined` 的时候，浏览器才不会去做这件傻事。所以我们经常会用 void 运算符来实现这个需求。

`javascript: void(0)`不会整体刷新页面。

```javascript
<a href="javascript:void(0);">
  这个链接点击之后不会做任何事情，如果去掉 void()，点击之后整个页面会被替换成一个字符0。
</a>

<a href="javascript:void(document.body.style.backgroundColor='green');">
  点击这个链接会让页面背景变成绿色。
</a>
```

###  href="#  V.S.  a href="javascript:void(0)"
`#`包含了一个位置信息，为网页地址添加了锚点。默认的锚是`#top`， 也就是网页的上端。`href="#"`操作，**会整体刷新页面，并回到页面顶部**。

`javascript:void(0)`仅仅表示一个死链接，**不会整体刷新页面**。当用户链接时，void(0) 计算为 0，但 Javascript 上没有任何效果。


## Web前端性能优化方法之延迟加载
参考[Web前端性能优化方法之延迟加载](http://www.toutiao.com/i6403312352967524865/?group_id=6403442413059162369&group_flags=0)解更多。

### 延迟加载
一个页面的展示如果每次都要等到所有内容都加载完毕，页面的加载速度势必会受到很大影响，这个时候延迟加载的优势就体现出来了。

**默认情况下，在页面加载期间，HTML代码的解析将暂停，直到脚本停止执行**。这意味着，如果服务器速度较慢或者脚本特别沉重，则会导致网页延迟。

**延迟加载是保证页面初次加载时，所需要的最小内容集，其他的内容在需要的时候再进行加载，这可以保证页面只需加载最少的资源，加快响应速度。**

### JS延迟加载
* 方法1：引入的JS文件放置于body结束标签之前

在引入外部js文件时，我们将js文件放在`<body>`的结束标签之前，这样可以让js文件在最后引入，从而可以加快页面加载速度。

* 方法2：使用setTimeout方法

使用setTimeout方法，动态的添加js脚本到head中。

```javascript
setTimeout(function(){
	//reference to <head>
	var head = document.getElementsByTagName("head")[0];
	//a new JS file
	var js  = document.createElement("script");
	js.type = "text/javascript";
	js.src = "newFile.js";
	//preload JS
	head.appendChild(js); 
},1000);
```

* 方法3：使用lazyload.js插件

lazyload.js是一款jQuery插件，可以延迟加载JS文件。

* 方法4：async和defer使用

参考[Script标签和脚本执行顺序](http://pij.robinqu.me/Browser_Scripting/Document_Loading/ScriptTag.html)了解跟多。

script标签（不带defer或async属性）的会阻止文档渲染。相关脚本会立即下载并执行。

async表示该script标签并不阻塞，也不同步执行。浏览器只需要在脚本下载完毕后再执行即可——不必阻塞页面渲染，等待该脚本的下载和执行。

带有defer属性的脚本，同样会推迟脚本的执行，并且不会阻止文档解析。就如同这个脚本，放置到了文档的末尾

### 结论
* 仅有async属性，脚本会异步执行
* 仅有defer属性，脚本会在文档解析完毕后执行
* 两个属性都没有，脚本会被同步下载并执行，期间会阻塞文档解析


### 图片延迟加载
* 方法1：使用lazyload.js插件

```vbscript-html
<script src-"jquery.min.js"></script>
<script src="jquery.lazyload.js"></script>
<script type="text/javascript">
	$(function(){
		$("img").lazyload({
			placeholder:"img/loading.gif",
			effect:"fadeIn"
		});
	});
</script>
```

* 方法2：原生JS代码图片滚动加载

```javascript
var temp = -1;
window.onscroll = function(){
	var imgElements = document.getElementsByTagName("img");
	var lazyImgArr = new Array();
	var j = 0;
	for(var i=0;i<imgElements.length;i++){
		if(imgElements[i].className == "lazy"){
			lazyImgArr[j++] = imgElements[i];
		}
	}
	//滚动的高度
	var scroHeight = document.body.scrollTop;
	//body(页面)可见区域的总高度
	var bodyHeight = document.body.offsetHeight;
	if(temp < scrollHeight){
		for(var k=0;k<lazyImgArr.length;k++){
			//图片纵坐标
			var imgTop = lazyImgArr[k].offsetTop;
			if((imgTop - scrollHeight) <= bodyHeight){
				lazyImgArr[k].src = lazyImgArr[k].alt;
				lazyImgArr[k].className = "notlazy";
			}
		}
		temp = scrollHeight;
	}
};
```

### CSS文件延迟加载
* 方法1：使用lazyload.js插件

```html
<script src-"jquery.min.js"></script>
<script src="jquery.lazyload.js"></script>
<script type="text/javascript">
LazyLoad.css("http://www.asp.net/ajaxLibrary/Themes/AjaxSiteStyles.css",function(arg){
	alert(arg);
},"AjaxSiteStyles.css has been loaded");
</script>
```

* 方法2：使用setTimeout方法实现

```javascript
setTimeout(function(){
	var elem  = document.createElement("link");
	elem.rel = "stylesheet";
	elm.type = "text/css";
	elem.href = "http://v3.bootcss.com/bootstrap.min.css";
	document.body.appendChild(elem); 
},1000);
```



## decodeURI() VS encodeURI()
* encodeURI() 用于将URI转换为十六进制编码。
* decodeURI() 可对encodeURI() 函数编码过的URI进行解码。


参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)和[W3 School](http://www.w3school.com.cn/jsref/jsref_encodeuri.asp)了解更多。

`encodeURI()` 函数可把字符串作为 URI 进行编码，**输出字符对应的ASCII码的16进制数**。encodeURI 会替换所有的字符，但不包括以下字符，即使它们具有适当的UTF-8转义序列

![encodeURI-1.JPG](http://ojx8u3g1z.bkt.clouddn.com/encodeURI-1.JPG)

**如果 URI 组件中含有分隔符，比如 `?` 和 `#`，则应当使用 `encodeURIComponent()` 方法分别对各组件进行编码。**

```javascript
document.write(encodeURI("http://www.w3school.com.cn")+ "<br />")
document.write(encodeURI("http://www.w3school.com.cn/My first/"))
document.write(encodeURI(",/?:@&=+$#"))
```

上述程序输出为

```javascript
http://www.w3school.com.cn
http://www.w3school.com.cn/My%20first/
,/?:@&=+$#
```

## 为什么不建议在JS中使用innerHTML
* 设置innerHTML值后，页面会进行刷新，耗时较多，造成性能损耗。
*  设置innerHTML值过程中，没有验证的余地。因此，更容易在文档中插入错误代码，从而使网页不稳定。

## COMMONJS AMD CMD区别
参考资料
* [COMMONJS, AMD, CMD区别](http://hao.jser.com/archive/7865/)
* [区别比较](http://www.cnblogs.com/omelette/p/6652472.html)

commonjs是用在服务器端的，同步的，如nodejs。

amd, cmd是用在浏览器端的，异步的，如requirejs和seajs

其中，amd先提出，cmd是在commonjs和amd基础上提出的。

### commonjs
commonjs的目标是制定一个js模块化的标准，它的目标制定一个可以同时在客服端和服务端运行的模块。

这些模块拥有自己独立的作用域，也可以向顶层曝露出自己的api，也就是module.exports。在ES6中，common被制定为标准。但是在ES6还未被浏览器完美支持的情况下，commonjs规范之能在服务端发挥它的作用。比如在nodejs和webpack等中。

而我们服务端的异步加载模块主要有2种加载方案：CMD和AMD。这两种规范的典型是seajs和rjs(requirejs)。这两种方案虽然都是加载模块的解决方案，但是还是有一些的差别。

### amd
AMD是为了弥补commonjs规范在浏览器中目前无法支持ES6的一种解决方案。异步模块定义规范（AMD）制定了定义模块的规则，这样模块和模块的依赖可以被异步加载。这和浏览器的异步加载模块的环境刚好适应（浏览器同步加载模块会导致性能、可用性、调试和跨域访问等问题）。

requirejs是AMD规范的实践者，RequireJS 是一个JavaScript模块加载器。它非常适合在浏览器中使用，但它也可以用在其他脚本环境，就像 Rhino and Node。使用RequireJS加载模块化脚本将提高代码的加载速度和质量。

### cmd
cmd是commonjs另外的一种模块加载方案，这个规范本身偏向于commonjs的规范。他的一个文件就是一个模块，和ES6中标准的commonjs规范类似。

它定义了一个`define(factory)`函数。define 接受 factory 参数，factory 可以是一个函数，也可以是一个对象或字符串。

CMD规范的实践者seaJS。

## 获取数组中的最大值和最小值

```javascript
var max = Math.max.apply(Math, array);
var min = Math.min.apply(Math, array);
```
## jQuery选择器
* 基本选择器
* 层次选择器
* 过滤选择器
* 表单选择器

## Ajax
### 基本介绍
AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。

AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

AJAX 是在不重新加载整个页面的情况下与服务器交换数据并更新部分网页的艺术。

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。

有很多使用 AJAX 的应用程序案例：新浪微博、Google 地图、开心网等等。

### jQuery中onload

在传统的JavaScript中，使用XMLHttpRequest对象异步加载数据。而在jQuery中，**使用onload方法可以轻松实现获取异步数据的功能**。语法格式如下。

```javascript
load(url,[data],[callback]);
```

其中，参数url为被加载页面的地址，可选项[data]参数表示发送到服务器的数据，**其格式为key/value**。另一个可选项[callback]参数表示加载成功后，返回值加载页面的回调函数。

## post和get区别
参考资料
* [浅谈HTTP中Get与Post的区别](http://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html)
* [W3School](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)
* [form表单中method的get和post区别](http://blog.sina.com.cn/s/blog_7ffb8dd501013kdm.html)


Http定义了与服务器交互的不同方法，最基本的方法有4种，分别是GET, POST, PUT, DELETE。

URL全称是资源描述符。可以这样认为：一个URL地址，它用于描述一个网络上的资源。而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的**查，改，增，删**4个操作。

**GET一般用于获取/查询资源信息，而POST一般用于更新资源信息。**

### GET
根据HTTP规范，GET用于信息获取，而且应该是安全和幂等的。GET不会修改或改变原信息。

(1) 所谓安全，意味着该操作用于获取信息（获取/查询）而非修改信息。换句话说，GET 请求一般不应产生副作用，它仅仅是获取资源信息，就像数据库查询一样，不会修改，不会增加数据，不会影响资源的状态。

注意，这里安全的含义仅仅是指是不修改信息。

(2) 幂等意味着对同一URL的多个请求应该返回同样的结果。

> 幂等( idempotent, idempotence)是一个数学或计算机学概念，常见于抽象代数中。
> 
> 幂等有一下几种定义
>
>对于单目运算，如果一个运算对于在范围内的所有的一个数多次进行该运算所得的结果和进行一次该运算所得的结果是一样的，那么我们就称该运算是幂等的。比如绝对值运算就是一个例子，在实数集中，有abs(a)=abs(abs(a))。
>
>对于双目运算，则要求当参与运算的两个值是等值的情况下，如果满足运算结果与参与运算的两个值相等，则称该运算幂等，如求两个数的最大值的函数，有在在实数集中幂等，即max(x,x) = x。

但在实际应用中，以上2条规定并没有这么严格。比如，新闻站点的头版不断更新。虽然第二次请求会返回不同的一批新闻，该操作仍然被认为是安全的和幂等的，因为它总是返回当前的新闻。从根本上说，如果目标是当用户打开一个链接时，他可以确信从自身的角度来看没有改变资源即可。

### POST
根据HTTP规范，POST表示可能**修改**变服务器上的资源的请求。

继续引用上面的例子，还是新闻以网站为例，读者对新闻发表自己的评论应该通过POST实现，因为在评论提交后站点的资源已经不同了，或者说资源被修改了。


### GET和POST区别

关于GET请求，有如下注意事项
* GET请求可被缓存
* GET请求保留在浏览器历史记录中
* GET请求可被收藏为书签
* GET请求不应在处理敏感数据时使用
* GET请求有长度限制
* GET请求只应当用于取回数据，而不能修改原数据

关于POST请求，有如下注意事项
* POST请求不会被缓存
* POST请求不会保留在浏览器历史记录中
* POST不能被收藏为书签
* POST请求对数据长度没有要求


* 区别1

GET不会修改或改变原信息。而POST可能会改变原信息。

* 区别2

**GET请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，参数之间以&相连**。例如，`login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0%E5%A5%BD`。

POST把提交的数据则放置在是HTTP包的**包体**中。

* 区别3

GET方式可提交的数据要少于POST方式可提交的数据。POST多用于传输较大量的数据。

GET是通过URL提交参数的，而实际上，**URL不存在参数上限的问题，HTTP协议规范没有对URL长度进行限制。**但是由于特点的**浏览器和服务器对URL参数的限制**，导致GET方式可提交的数据要少于POST方式可提交的数据。例如，IE对URL长度的限制是2083字节。对于其他浏览器，如Netscape，Firefox，Chrome等，理论上没有长度限制，**其限制取决于操作系统的支持**。

>注意，这个限制长度是整个URL的长度，而不仅仅是参数数据的长度。

HTTP协议规范中，对POST参数长度也没有限制。限制POST参数长度的，是服务器的处理程序的能力。

* 区别4

**POST的安全性要比GET的安全性高**。注意，这里所说的安全性和上面GET提到的“安全”不是同个概念。上面“安全”的含义仅仅是不作数据修改，而这里安全的含义是真正的Security的含义。

比如，**通过GET提交数据，用户名和密码将明文出现在URL上**，因为
1. 登录页面有可能被浏览器缓存
2. 其他人查看浏览器的历史纪录，那么其他人就可以拿到你的账号和密码

除此之外，使用GET提交数据还可能会造成Cross-site request forgery攻击。

* 区别5
GET请求只能进行URL编码，而POST支持多种编码方式。

* 区别6
对参数的数据类型，GET只接受ASCII字符，而POST没有限制。

### PUT
PUT用于向指定的URI传送**更新**资源，是幂等的。而POST则不是幂等的。


### 总结
Get是向服务器发送取数据的一种请求，而Post是向服务器提交数据的一种请求。在FORM（表单）中，Method默认为"GET"，实质上，GET和POST只是发送机制不同，并不是一个取一个发！


### FORM表单-method
参考[form表单中method的get和post区别](http://blog.sina.com.cn/s/blog_7ffb8dd501013kdm.html)了解跟多。

#### 问题提出

```html
<form action="getPostServlet/getPost.do?param4=param4" method="get">
	<input type="hidden" name="param1" value="param1">
    <input type="hidden" name="param2" value="param2">
    <input type="text" name="param3" value="param3" readonly>
    <input type="submit" name="button1" value="submit">
</form>
```

表单中action中带有参数param4。

如果用get方法提交，后台无法接收到参数param4。

如果用post方法提交，后台可以接收到参数param4。

#### 问题分析
用get方法提交的url显示
```
http://localhost/mywebapp/getPostServlet/getPost.do?pram1=param1&pram2=param2&pram3=param3&button1=submit
```
**method=get时，action自己后边带的参数列表会被忽视，后台无法接收到这个参数，只能得到表单中的参数。**

用post方法提交的url显示如下
```
http://localhost/mywebapp/getPostServlet/getPost.do?param4=param4
```
method=post方式提交表单，参数分为两部分：一部分是action中的参数放在地址栏；另一部分是表单中的参数放在请求的头中；所以所有的数据后台全部能获得。

对于get方式，服务器端用Request.QueryString获取变量的值。对于post方式，服务器端用Request.Form获取提交的数据。

## JSON
JSON是一种数据交换格式，它通过可以key/value或数组的形式保存数据。

JSON的结构包含2种，一种是key/value形式，另外一种是数组格式，后者用于处理复杂的JS对象。

ECMAScript 5中提供了2个函数用于JSON值和JavaScript值的转换
* JSON.stringify(): 将JS值转换为JSON值
* JSON.parse(): 将JSON值转换为JS值

## HTTP和HTTPS区别
参考资料
* [Http和Https的区别 | 简书](http://www.jianshu.com/p/37654eb66b58)
* [HTTP和HTTPS的区别](https://code-ken.github.io/2016/01/05/diff-http-https/)

* https协议需要ca申请证书，一般免费证书很少，需要交费。
* http是超文本传输协议，信息是明文传输，https 则是具有安全性的**ssl加密传输协议**。
* http和https使用的是完全不同的连接方式，用的端口也不一样。HTTP默认端口是80，HTTPS默认端口是443。

### HTTP
HTTP，即超文本传输协议，它完成客户端到服务端等一系列运作流程。

与HTTP有关的协议包括IP，TCP和DNS。

IP协议工作在网络层。IP地址指明了节点被分配到的地址，MAC地址是网卡所指定的固定地址。IP地址和MAC地址进行配对。IP地址可以更换，但是MAC地址基本不会更换（和网卡硬件有关）。

#### HTTP缺点
* 通信使用明文(不加密)，内容可能会被窃听

![http-diff-1.png](http://ol3kbaay9.bkt.clouddn.com/http-diff-1.png)
* 不验证通信方的身份, 因此有可能遭遇伪装

![http-diff-2.png](http://ol3kbaay9.bkt.clouddn.com/http-diff-2.png)
* 无法证明报文的完整性, 所以有可能已遭篡改

![http-diff-3.png](http://ol3kbaay9.bkt.clouddn.com/http-diff-3.png)


### HTTPS
**HTTP + 加密 + 认证 + 完整性保护 = HTTPS**

![https-1.png](http://ol3kbaay9.bkt.clouddn.com/https-1.png)

#### HTTPS是身披SSL外壳的HTTP

通常情况下HTTP是直接和TCP层进行通信的。当使用**SSL(安全套阶字)**时，则演变成HTTP先和SSL通信，SSL再和TCP通信的了。

![https-2.png](http://ol3kbaay9.bkt.clouddn.com/https-2.png)

#### 加密技术

讲解SSL前，科普一下加密方法。SSL采用的是一种叫做**公开密钥加密**的加密处理方式。
##### 对称加密
加密和解密用的一个密钥的方式称为对称加密，也叫做共享密钥加密。
![https-3.png](http://ol3kbaay9.bkt.clouddn.com/https-3.png)

对称加密在发送加密信息时也需要将密钥发送给对方，但这样可以被攻击者截取，就不安全啦。


![https-4.png](http://ol3kbaay9.bkt.clouddn.com/https-4.png)

##### 非对称加密
非对称加密又称作公开密钥加密。它很好的解决了对称加密密钥被截取的问题。

非对称加密采用一对非对称的密钥，一把叫做私有密钥，一把叫做共有密钥。

使用非对称加密，发送密文一方使用对方的共有密钥进行加密处理，对方收到加密信息后，再使用自己的私有密钥进行解密。

![https-5.png](http://ol3kbaay9.bkt.clouddn.com/https-5.png)


#### HTTPS采用混合加密机制
HTTPS采用对称加密和非对称加密所混合的加密机制。

若密钥不能安全交换，那么有可能仅考虑非对称加密。

但是非对称加密与对称加密相比，处理速度相对较慢。

![https-6.png](http://ol3kbaay9.bkt.clouddn.com/https-6.png)

#### 公开密钥的认证
使用数字证书认证机构和其颁布的公开密钥证书进行认证。即让第三方独立机构进行验证。（需要一定的费用）

注意，私有密钥是保存在服务器端的。

![https-7.png](http://ol3kbaay9.bkt.clouddn.com/https-7.png)

#### HTTPS安全通信机制
![https-7.png](http://ol3kbaay9.bkt.clouddn.com/https-8.png)

下图是完整的HTTPS的通信过程
![https-7.png](http://ol3kbaay9.bkt.clouddn.com/https-9.png)

### 为什么HTTPS不是那么普及
* 加密通信与纯文本通信相比，消耗更多的CPU和内存资源
* 购买证书ca是要钱的
* 少许对客户端有要求的情况下，会要求客户端也必须有一个证书。
这里客户端证书，其实就类似表示个人信息的时候，除了用户名/密码，还有一个CA 认证过的身份。目前少数个人银行的专业版是这种做法,具体证书可能是拿U盘作为一个备份的载体。

### HTTPS 一定是繁琐的
* 本来简单的http协议，一个get一个response。由于https要还密钥和确认加密算法的需要，单握手就需要6/7 个往返。任何应用中，过多的round，trip，肯定影响性能。
* 接下来才是具体的http协议。每一次响应或者请求， 都要求客户端和服务端对会话的内容做加密/解密。尽管对称加密/解密效率比较高，可是仍然要消耗过多的CPU，为此有专门的SSL芯片。如果CPU性能比较低的话，肯定会降低性能，从而不能serve 更多的请求。




## 前端图片格式
参考资料
* [web前端图片极限优化策略](http://jixianqianduan.com/frontend-weboptimize/2015/11/17/front-end-image-optmize.html)

### 图片格式

![image-format.JPG](http://ol3kbaay9.bkt.clouddn.com/image-format.JPG)

### 图片格式选择

颜色丰富的照片，JPG是通用的选择
* 人眼的结构很适合查看JPG压缩后的照片，可以充分的忽略并在脑中补齐细节
* JPG在压缩率不高时保留的细节还是不错的
* WebP能够比JPG减少30%的体积，但目前兼容性较差

如果需要较通用的动画，GIF是唯一可用的选择
* GIF支持的颜色范围为256色，而且仅支持完全透明/完全不透明
* GIF在显示颜色丰富的动画时可能出现颜色不全、边缘锯齿等问题

如果图片由标准的几何图形组成，或需要使用程序动态控制其显示特效，可以考虑SVG格式
* SVG是使用XML定义的矢量图形，生成的图片在各种分辨率下均可自由放缩
* SVG中可以通过JavaScript等接口自由变换图片特效，可以完成其中部分元素的自由旋转、移动、变换颜色等

如果需要清晰的显示颜色丰富的图片，PNG比较好
* PNG-8能够显示256种颜色，但能够同时支持256阶透明，因此颜色数较少但需要半透明的情景（如微信动画大表情）可以考虑PNG-8
* PNG-24可以显示真彩色，但不支持透明，颜色丰富的图片推荐使用（如屏幕截图、界面设计图）
* PNG-32可以显示真彩色，同时支持256阶透明，效果最好但尺寸也最大

## 前端图片优化方案
* **使用base64编码代替图片场景**

适用于图片大小小于2KB，页面上引用图片总数不多的情况。

原理：将图片转换为base64编码字符串，inline到页面或css中。

优势：**减少http的请求次数**，并可以放到后台数据库中，只传输字符串，有较多的构建工具可以直接实现 

劣势：这种方法仅限于图片总数较少，而且图片大小小于2KB的情况。否则图片字符串会变得很长很长。

* **合并图片sprite(雪碧图)**

场景：任何用到页面图片的场景 

原理：将多个页面上用到的背景图片合并成一个大的图片在页面中引用。

优势：**可以有效的减少HTTP请求个数**，而且不影响开发体验，使用构建插件可以做到对开发者透明。适用于页面图片多且丰富的场景。 

劣势：生成的图片体积较大，减少请求个数同时也增加了图片大小，不合理拆分将不利于并行加载。

* **使用css, svg, canvas或iconfont代替图片**

**css代替图片** 

场景：适用于移动端或较高级的浏览器，而且绘制的图案较为简单。 

原理：css方式可以用来绘制相对简单的团来代替图片，一般使用before或者after伪元素来丰富图案的复杂度。 

优势：具有实现简单，图片体积小的特点，可以实现简单的动态效果 劣势：也受限于css的兼容性特点，绘制复杂图案困难。

**svg代替图片**

svg 是一种矢量图片，支持透明，缩放，动画，除了android 2.3的手机，其它场景都支持，是一种比较好的图片代替方案。 

优势：SVG是矢量图形，不受像素影响——SVG的这个特性使得它在不同的平台或者媒体下表现良好，无论屏幕分辨率如何。SVG对动画的支持较好；其DOM结构可以被其特定语法或者Javascript控制，从而轻松的实现动画。Javascript可以完全控制SVG Dom 元素。SVG的结构是 XML，其可访问性（盲文、声音朗读等）、可操作性、可编程性、可被CSS样式化完胜Canvas。另外，其支持 ARIA 属性，使其如虎添翼。

劣势：DOM比正常的图形慢，而且如果其结点多而杂，就更慢了。 不适合网页游戏等。

**canvas代替图片**

场景：需要高性能的图片或动画 

原理：适用html5的canvas元素绘制创建图片 

优势：整个就是画2D图形时，页面渲染性能比较高，页面渲染性能受图形复杂度影响小，性能只受图形的分辨率的影响，画出来的图形可以直接保存为 .png 或者 .jpg的图形，适合于画光栅图像或者不规则图形 

劣势：没有dom操作，必须依赖定时器，文字渲染性能差，不能添加描述(title属性等)，兼容性限制

**iconfont**是一种web字体，来代替图片的解决方案

场景：代替页面上色彩单一的图片 

优势：兼容性好，应用广，目前使用也很广泛 

劣势：但是由于字体的颜色设置单一，只能用于代替颜色单一的图片，对于色彩复杂的图片，iconfont处理起来比较困难

* **响应式图片**

场景：不同终端对同一个图片需求不一样，可以根据终端加载不同的图片来节省没必要的流量 

原理：通过picture元素，picturefill或平台判断来为不同终端平台输出不同的图片 

优势：减少没必要的图片加载，灵活控制，慢速用户加载小图片不至于加载失败，移动端没必要加载大尺寸图片等，可以通过不同方式兼容所有浏览器 

劣势：无法避免图片的加载过程，图片本身没优化

* **图片压缩**

场景：在不得不加载图片的前提下，要进一步提升优化效果，只能通过有损或无损压缩来减少图片的大小。 

原理：对图片进行无损、有损压缩，转为压缩后图片来实现 

优势：减少图片加载流量，效果比较明显 劣势：服务器和浏览器压力增大，而且服务器需要额外的服务支持

* **更好的图片格式** 

场景：之前说到webp、bpg、sharpP等新图片格式具有更好的压缩比，可以使用这类新型的图片来代替原始图片 

原理：对图片格式转换，在画质可以接受的情况下达到更好的压缩比效果 

优势：减少图片加载流量，效果比较明显 

劣势：服务器和浏览器压力增大，而且服务器需要额外的服务支持，格式转换要考虑浏览器的兼容性


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 