---
title: JS经典面试题-2
date: 2017-05-22 22:45:26
categories: Front-end
tags: [Front-end,JavaScript]
keywords: JavaScript
toc: true
---







* 本文主要对JS经典面试题进行记录。对实例化对象和直接调用函数情况中this的指向进行分析。
* [JavaScript中new实例化对象与调用的this的区别](http://www.toutiao.com/i6420642535529513473/)

<!--more-->


## 知识点

**定义一个函数，并返回this。**
* **如果直接调用该函数，this指向的是window。**
* **如果通过new操作符实例化一个对象，则this指向的是对象本身。**


## 题目1

```javascript
function Fun(argument1,argument2){
	return this;
}

//直接调用
var f1 = Fun();   
console.log(f1);
//Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}

//实例化对象
var f2 = new Fun();  
console.log(f2);  //Fun {}
```

* **f2实例化了对象Fun，实例化对象后，this指向的是对象本身。**
* **f1仅仅是是调用Fun函数。this指向的是window。**

## 题目2

```javascript
function fun1(){
	console.log(this);
	function fun2(){
		console.log(this);
	}
	fun2();

	function fun3(){
		console.log(this);
	}

	var a = new fun3();
}

var f = fun1();
//outcome
//window{}
//window{}
//fun3{}
```

* f调用了fun1，输出的第一个this指向window。
* fun2也是调用，所以this也是指向window。
* 但是a就不一样了，它是实例化的对象，this指向是fun3实例化后的对象。因此输出`fun3{}`

## 题目3
对题目2作如下修改

```javascript
function fun1(){
	console.log(this);
	function fun2(){
		console.log(this);
	}
	fun2();

	function fun3(){
		console.log(this);
	}

	var a = new fun3();
}

var f = new fun1();
//outcome
//fun1{}
//window{}
//fun3{}
```

* fun1和fun3是被实例化，this指向对象本身
* fun2只是调用，this指向window
      
## 题目4

在题目3的基础上，对this属性进行赋值。


```javascript
this.alia = "window";
this.win1 = "window property";
function fun1(){
	this.alia = "fun1";
	this.ofun1 = "only fun1 have";
	console.log("fun1的alia:"+this.alia);  //"fun1的alia:fun1"
	console.log(this.win1);  //"window property"
	function fun2(){
		this.alia = "fun2";
		console.log("fun2的alia:"+this.alia);  
		//"fun2的alia:fun2"
		console.log(this.ofun1);  // "only fun1 have" 
		console.log(this.win1);  //"window property"
		this.ofun1 = "fun2 change"
	}
	fun2();
	console.log("this.ofun1:"+this.ofun1);  
	//"this.ofun1: fun2 change"
}
fun1();
//fun1的alia:fun1
//window property
//fun2的alia:fun2
//only fun1 have
//window property
//this.ofun1:fun2 change
```
   

**可以看到，JavaScript调用函数里面的this是指向window的，因此，对tgihs属性的赋值都是给window赋值的。**


## 题目5

在题目4的基础上，对this属性进行赋值。并将直接调用fun1改为new操作符实例化对象。

```javascript
this.alia = "window";
this.win1 = "window property";

var a = new fun1();
//fun1的alia:fun1
//undefined
//undefined
//fun2的alia:window
//window property
//this.ofun1:only fun1 have
//window.ofun1:change in fun2

function fun1(){
	//fun1内的this指向的a对象，是fun1()
	this.alia = "fun1";   
	this.ofun1 = "only fun1 have";
	console.log("fun1的alia:"+this.alia);  //"fun1的alia:fun1"
	console.log(this.win1);  //"undefined"  !!!
	function fun2(){
		// 直接调用  this指向的是window
		//window没有ofun1属性，所以输出undefined
		console.log(this.ofun1);  // "undefined"	
		//下面是window已有的属性
		console.log("fun2的alia:"+this.alia);  
		//"fun2的alia:window"
		console.log(this.win1);  //"window property"
		//给window添加属性
		this.ofun1 = "change in fun2";
	}
	fun2();
	//这时this指向的是a对象本身的fun1()
	console.log("this.ofun1:"+this.ofun1);  
	//"this.ofun1: only fun1 have"
	//window.ofun1刚才已经在fun2中赋值了，所以输出有值
	console.log("window.ofun1:"+window.ofun1);
	//"window.ofun1: change in fun2"  

}
```

**定义一个函数，并返回this。**
* **如果直接调用该函数，this指向的是window。**
* **如果通过new操作符实例化一个对象，则this指向的是对象本身。**

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 