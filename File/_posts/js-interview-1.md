---
title: JS经典面试题-1
date: 2017-05-22 22:35:26
categories: Front-end
tags: [Front-end,JavaScript]
keywords: JavaScript
toc: true
---






* 本文主要对JS经典面试题进行记录。
* [前端程序员经常忽视的一个JavaScript面试题](https://github.com/Wscats/Good-text-Share/issues/85)

<!--more-->


## 题目

```java
function Foo() {
    getName = function () { 
	    alert (1); 
	};
    return this;
}

Foo.getName = function () { 
	alert (2);
};

Foo.prototype.getName = function () { 		
	alert (3);
};

var getName = function () { 
	alert (4);
};

function getName() { 
	alert (5);
}
 
//请写出以下输出结果
Foo.getName(); //2
getName();  //4
Foo().getName(); //1
getName(); //1
new Foo.getName();//2
new Foo().getName();//3
new new Foo().getName();//3
```
## 分析
这道题的经典之处在于它综合考察了面试者的JavaScript的综合能力，包含了变量定义提升、this指针指向、运算符优先级、原型、继承、全局变量污染、对象属性及原型属性优先级等知识。


**题目中，首先定义了一个叫Foo的函数，之后为Foo创建了一个叫getName的静态属性存储了一个匿名函数，之后为Foo的原型对象新创建了一个叫getName的匿名函数。之后又通过函数变量表达式创建了一个getName的函数，最后再声明一个叫getName函数。**



## 求解
### 第1问

```javascript
Foo.getName(); //2
```

Foo.getName自然是访问Foo函数上存储的静态属性，答案自然是2，这里就不需要解释太多的。一般来说第一问对于稍微懂JS基础的同学来说应该是没问题的，当然我们可以用下面的代码来回顾一下基础，先加深一下了解。

```javascript
function User(name) {
	var name = name; //私有属性
	this.name = name; //公有属性
	function getName() { //私有方法
		return name;
	}
}

User.prototype.getName = function() { //公有方法
	return this.name;
}

User.name = 'Wscats'; //静态属性
User.getName = function() { 
	//静态方法
	return this.name;
}

var Wscat = new User('Wscats'); //实例化
```

		
注意下面这几点
* 调用公有方法，公有属性，我们必需先实例化对象。也就是用new操作符实化对象，就可构造函数实例化对象的方法和属性，并且公有方法是不能调用私有方法和静态方法的
* 静态方法和静态属性就是我们无需实例化就可以调用
* 而对象的私有方法和属性，外部是不可以访问的

### 第2问

```javascript
getName();  //4
```

直接调用getName函数，访问当前上文作用域内的叫getName的函数，所以这里应该直接把关注点放在4和5上，跟1 2 3都没什么关系。

此处其实有两个坑，一是变量声明提升，二是函数表达式和函数声明的区别。

参考资料
* [关于Javascript的函数声明和函数表达式](https://github.com/Wscats/Good-text-Share/issues/73)
* [关于JavaScript的变量提升](https://github.com/Wscats/Good-text-Share/issues/86)


在Javascript中，定义函数有两种类型
*  函数声明

```javascript
// 函数声明
function wscat(type){
	return type==="wscat";
}
```
* 函数表达式

```javascript
// 函数表达式
var oaoafly = function(type){
	return type==="oaoafly";
 }
```

给出如下示例程序，在一个程序里面同时用函数声明和函数表达式定义一个名为getName的函数

```javascript
getName()//oaoafly
var getName = function() {
	console.log('wscat')
}

getName()//wscat
function getName() {
	console.log('oaoafly')
}

getName()//wscat
```

JavaScript 解释器中存在一种变量声明被提升的机制，也就是说函数声明会被提升到作用域的最前面，即使写代码的时候是写在最后面，也还是会被提升至最前面。

而用函数表达式创建的函数是在运行时进行赋值，且要等到表达式赋值完成后才能调用


```javascript
 var getName //变量被提升，此时为undefined
                
getName()//oaoafly 
//函数被提升 这里受函数声明的影响，虽然函数声明在最后可以被提升到最前面了

var getName = function() {
	console.log('wscat')
};
//函数表达式此时才开始覆盖函数声明的定义

getName()//wscat  函数表达式

function getName() {
	console.log('oaoafly')
}

getName()//wscat 这里就执行了函数表达式的值
```
               

Javascript中函数声明和函数表达式是存在区别的，函数声明在JS解析时进行函数提升，因此在同一个作用域内，不管函数声明在哪里定义，该函数都可以进行调用。而函数表达式的值是在JS运行时确定，并且在表达式赋值完成后，该函数才能调用。

所以，第2问的答案就是4，5的函数声明被4的函数表达式覆盖了。

```javascript
var getName = function () { alert (4);};
function getName() { alert (5);}
getName();  //4

//上述程序等价于

var getName;
function getName() { alert (5);}
getName = function () { alert (4);};

getName();  //4
```

### 第3问


```javascript
Foo().getName(); //1
```
`Foo().getName()`先执行了`Foo`函数，然后调用Foo函数的返回值对象的getName属性函数。因此，语句输出结果为1。

```javascript
function Foo() {
    getName = function () { alert (1); };
    return this;
}
var getName = function () { alert (4);};
function getName() { alert (5);}
```
Foo函数的第一句`getName = function () { alert (1); };`是一句函数赋值语句，**注意它没有var声明**（因此为全局变量），所以先向当前Foo函数作用域内寻找getName变量，没有。再向当前函数作用域上层，即外层作用域内寻找是否含有getName变量，找到了，也就是第二问中的alert(4)函数，将此变量的值赋值为function(){alert(1)}。

**此处实际上是将外层作用域内的getName函数修改了**。

注意，此处若依然没有找到会一直向上查找到window对象。若window对象中也没有getName属性，就在window对象中创建一个getName变量。

之后Foo函数的返回值是this，而JS的this问题已经有非常多的文章介绍，这里不再多说。简单的讲，**this的指向是由所在函数的调用方式决定的**。而此处的直接调用方式，this指向window对象。

于是Foo函数返回的是window对象，相当于执行window.getName()，**而window中的getName已经被修改为alert(1)**，所以最终会输出1。

此处考察了两个知识点，一个是变量作用域问题，一个是this指向问题。我们可以利用下面代码来回顾下这两个知识点

```javascript
var name = "Wscats";//全局变量
window.name = "Wscats";//全局变量
function getName() {
	name = "Oaoafly"; //去掉var变成了全局变量
	var privateName = "Stacsw";
	return function() {
		console.log(this);//window
		return privateName
	}
}

var getPrivate = getName("Hello"); 
//当然传参是局部变量，但函数里没有接受这个参数

console.log(name) //Oaoafly
console.log(getPrivate()) 
//Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}
//Stacsw
```

               
因为JS没有块级作用域，但是函数是能产生一个作用域的，函数内部不同定义值的方法会直接或者间接影响到全局或者局部变量，函数内部的私有变量可以用闭包获取。

**而关于this，this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象。**

所以，第3问中实际上就是window在调用**Foo()**函数，所以this的指向是window。

```javascript
Foo().getName(); //1
//等价于
window.Foo().getName();  //1
```


### 第4问

```javascript
getName(); //1
```
直接调用getName函数，相当于window.getName()，因为这个变量已经被Foo函数执行时修改了，遂结果与第三问相同，为1，也就是说Foo执行后把全局的getName函数给重写了一次，所以结果就是Foo()执行重写的那个getName函数。

### 第5问
```javascript
new Foo.getName();  //2
```
此处考察的是JS的运算符优先级问题，也是难度比较大的一题。

下面是JS运算符的优先级表格，从高到低排列。可参考[MDN运算符优先级](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)。

![js-operator-priority.JPG](http://ol3kbaay9.bkt.clouddn.com/js-operator-priority-1.JPG)
![js-operator-priority.JPG](http://ol3kbaay9.bkt.clouddn.com/js-operator-priority-2.JPG)
![js-operator-priority.JPG](http://ol3kbaay9.bkt.clouddn.com/js-operator-priority-3.JPG)
![js-operator-priority.JPG](http://ol3kbaay9.bkt.clouddn.com/js-operator-priority-4.JPG)


查看表中优先级的第18和第17级，都出现关于new的优先级，**new (带参数列表)比new (无参数列表)高比函数调用高，跟成员访问同级。**

因此，第5问中优先级为

```javascript
new Foo.getName();  //2

//等价于
new (Foo.getName)();  //2
```

* 点的优先级(18)比new无参数列表(17)优先级高。当点运算完后，又因为有个括号()，此时就是变成new有参数列表(18)，所以直接执行new。
* 当然也可能有朋友会有疑问为什么遇到()，不是先执行函数调用再new呢？那是因为函数调用(17)比new有参数列表(18)优先级低。
* .成员访问(18)->new有参数列表(18)。所以这里实际上将getName函数作为了构造函数来执行，遂弹出2。


### 第6问

```javascript
new Foo().getName();//3

//等价于
(new Foo()).getName();  //3
```
这一题比上一题的唯一区别就是在Foo那里多出了一个括号。这个有括号跟没括号，会造成优先级的区别。

* 首先new有参数列表(18)跟点的优先级(18)是同级
* 同级的话按照从左向右的执行顺序，所以先执行new有参数列表(18)再执行点的优先级(18)
* 最后再函数调用(17)
* new有参数列表(18)->.成员访问(18)->()函数调用(17)

这里还有一个小知识点，Foo作为构造函数有返回值，所以这里需要说明下**JS中的构造函数返回值问题。**

#### 构造函数的返回值

**在传统语言中，构造函数不应该有返回值，实际执行的返回值就是此构造函数的实例化对象。而在JS中构造函数可以有返回值也可以没有。**

* JS中当构造函数没有返回值时，则按照其他语言一样返回实例化对象。

```javascript
function Foo(name){
	this.name = name
}
console.log(new Foo('wscats'));  
//Foo {name: "wscats"}
```
* JS中当构造函数有返回值时，则检查其返回值是否为引用类型。如果是非引用类型，如基本类型（String,Number,Boolean,Null,Undefined），则与无返回值相同，实际返回其实例化对象。

```javascript
 function Foo(name){
	this.name = name
	return 520;
}
console.log(new Foo('wscats'));
//Foo {name: "wscats"}
```
* JS中当构造函数有返回值时，则检查其返回值是否为引用类型。若返回值是引用类型，则实际返回值为这个引用类型。


```javascript
function Foo(name){
	this.name = name
	return {
		age:16
	}
}
console.log(new Foo('wscats'));
//Object {age: 16}
```

原题中，由于返回的是this，而**this在构造函数中本来就代表当前实例化对象，最终Foo函数返回实例化对象。**

之后调用实例化对象的getName函数，因为在Foo构造函数中没有为实例化对象添加任何属性，当前对象的原型对象(prototype)中寻找getName函数。

当然这里再拓展个题外话，如果构造函数和原型链都有相同的方法，如下面的代码，那么**默认会调用构造函数的公有方法而不是原型链**，这个知识点在原题中没有表现出来，在后面的改进版习题上添加了对该知识点的考察。


```javascript
function Foo(name) {
	this.name = name;
	this.getName = function() {
		return this.name;
	}
}

Foo.prototype.name = 'Oaoafly';
Foo.prototype.getName = function() {
	return 'Oaoafly';
}

console.log((new Foo('Wscats')).name)//Wscats
console.log((new Foo('Wscats')).getName())//Wscats
```

### 第7问

```javascript
new new Foo().getName();   // 3
//等价于
new ((new Foo()).getName)();
```

`new new Foo().getName();`同样是运算符优先级问题。
* new有参数列表(18)->new有参数列表(18)
* 先初始化Foo的实例化对象，然后将其原型上的getName函数作为构造函数再次new
* 所以最终结果为3

## 答案
```javascript
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}


Foo.getName();//2
getName();//4
Foo().getName();//1
getName();//1
new Foo.getName();//2
new Foo().getName();//3
new new Foo().getName();//3
```

## 后续
对上述试题难度再次增大，在Foo函数里面加多一个公有方法getName。

```javascript
function Foo() {
	this.getName = function() {
		console.log(3);
		return {
			getName: getName//这个就是第六问中涉及的构造函数的返回值问题
		}
	};//这个就是第六问中涉及到的，JS构造函数公有方法和原型链方法的优先级

	getName = function() {
		console.log(1);
	};
	return this
}

Foo.getName = function() {
	console.log(2);
};

Foo.prototype.getName = function() {
	console.log(6);
};

var getName = function() {
	console.log(4);
};

function getName() {
	console.log(5);
} 


Foo.getName(); //2
getName(); //4

console.log(Foo());
//Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}

Foo().getName(); //1
getName(); //1
new Foo.getName(); //2
new Foo().getName(); //3
//添加的语句
new Foo().getName().getName(); //3 1
new new Foo().getName(); //3
```

               
## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 