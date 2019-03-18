---
title: JavaScript易错知识点整理 -1
date: 2017-03-06 15:46:26
categories: Front-end
tags: [Front-end,JavaScript]
keywords: JavaScript
toc: true
---


* 本文主要对JavaScript中的易错知识点进行整理。
* 本文对求职笔试面试中的一些常见考点进行了记录和总结。
* 易错知识点
	-  变量作用域
	- 类型比较
	- this指向
	- 函数参数
	- 闭包问题
	- 对象拷贝与赋值

<!--more-->





## 变量提升(Hoisting)
### 示例1

```javascript
function test(){
	console.log(a);  //undefined
	console.log(foo());  //"foo"
	console.log(boo());  //Uncaught TypeError: boo is not a function

	var a = 5;
	//函数声明
	function foo(){
		return "foo";
	}
	//函数表达式
	var boo = function(){
		return "boo";
	}
}
```
* 上述代码执行结果是`undefined`，`2` 和 `Uncaught TypeError: boo is not a function`。
* 变量和函数都被提升(`hoisted`) 到了函数体的顶部。对于变量，只会提升变量声明，不会提升变量赋值。对于函数，只有函数声明形式才能被提升，函数表达式不会被提升。因此，上述程序等价于


```javascript

function test(){
	//变量声明提升
	var a;
	//函数声明
	function foo(){
		return "foo";
	}
	//函数表达式仅仅提升变量声明
	var boo;
	console.log(a);  //undefined
	console.log(foo());  //"foo"
	console.log(boo());  //Uncaught TypeError: boo is not a function
	a = 5;	
	//函数表达式
	boo = function(){
		return "boo";
	}
}
```




## 事件循环

### 示例1


```javascript
function printing(){
	console.log(1);
	setTimeout(function(){console.log(2);},1000);
	setTimeout(function(){console.log(3);},0);
	console.log(4);
} 

printing();   //1 4 3 2
```

* 上述程序将依次输出

```
1 
4
3
2
```
* `setTimeout(function(){console.log(3);},0);`中延时虽然为0，但是该函数仍然会被放在任务队列中。当主线程执行完毕后，才会异步调用`console.log(3)`语句。
 
### 示例2

```javascript
setTimeout(function() {
  console.log('hello world');
}, 1000);
while(true) {};  //主线程一直执行
```

* 运行上述程序，并不会输入`hello world`。
* 一段js代码（里面可能包含一些setTimeout、鼠标点击、ajax等事件），从上到下开始执行，遇到setTimeout、鼠标点击等事件，异步执行它们，此时并不会影响代码主体继续往下执行（**当线程中没有执行任何同步代码的前提下才会执行异步代码**），一旦异步事件执行完，回调函数返回，将它们按次序加到执行队列中，这时要注意了，**如果主体代码没有执行完的话，是永远也不会触发callback的**，这也就是上面的一段代码导致浏览器假死的原因（主体代码中的while(true){}还没执行完）。

##  创建原生(native) 方法
### 示例1
* 在String对象上定义一个`repeatify` 函数。这个函数接受一个整数参数，来明确字符串需要重复几次。这个函数要求字符串重复指定的次数。例如

```javascript
console.log('Hello'.repeatify(3));
```
上述程序会输出`HelloHelloHello`。

* 解答

```javascript
String.prototype.repeatify = String.prototype.repeatify || function(times){
	var str = "";
	for(var i=0;i<times;i++){
		str += this;   //使用this参数
	}
	return str;
} 
```

上述方法中使用了`this`参数。需要注意的是，在定义自己的方法前，需要先检测该方法是否已经存在。即

```javascript
String.prototype.repeatify = String.prototype.repeatify || function(times){
//...
}
```
 
 
## this的用法

### 示例1

#### 题目

```javascript
var fullname = 'John Doe';
var obj = {
	fullname:'Colin Ihring',
	prop:{
		fullname:'Aurelio De Rosa',
		getFullname:function(){
			return this.fullname;
		}
	}
};

console.log(obj.prop.getFullname());  //Aurelio De Rosa
var test = obj.prop.getFullname;
console.log(test());  //John Doe
```

(1) 分析上述程序的运行结果，结合`this`相关知识点，对程序进行分析。
(2) 修改上述程序，让最后一个`console.log()`打印输出`Aurelio De Rosa`。


#### 解答

* 上述代码输出结果为`Aurelio De Rosa` 和 `John Doe` 。
* **JavaScript中关键字this所引用的是函数上下文，取决于函数是如何调用的，而不是怎么被定义的。**
* 在第一个`console.log()`，`getFullname()`是作为`obj.prop`对象的函数被调用。因此，当前的上下文指代后者，并且函数返回这个对象的fullname属性。
* 相反，当`getFullname()`被赋值给`test`变量时，**当前的上下文是全局对象window**，这是因为`test`被隐式地作为全局对象的属性。基于这一点，函数返回window的fullname，在本例中即为第一行代码设置的。
* 针对第2问，可以使用`call()`或者`apply()`方法强制转换上下文环境。

```javascript
var test = obj.prop.getFullname;
console.log(test());  //John Doe
//修改
console.log(test.call(obj.prop));  //Aurelio De Rosa
console.log(test.apply(obj.prop));  //Aurelio De Rosa
```

## 闭包 Closures
### 示例1
#### 题目

```javascript
//HTML
//<button>Click me 1</button>
//<button>Click me 2</button>
//<button>Click me 3</button>
//<button>Click me 4</button>
//<button>Click me 5</button>

//JavaScript
var nodes = document.getElementsByTagName("button");
for(var i = 0;i<nodes.length;i++){
	nodes[i].addEventListener('click',function(){
		console.log('You clicked element #'+i);
	});
}
```

(1) 请问，如果用户点击第一个和第四个按钮的时候，控制台分别打印的结果是什么？为什么？
(2) 修复(1)中的问题，使得点击第一个按钮时输出0，点击第二个按钮时输出1，依此类推。

#### 解答
* 代码打印两次`You clicked element #NODES_LENGTH`，其中`NODES_LENGTH`是nodes的结点个数（本例中为5）。（闭包知识分析）
* 在for循环完成后，变量`i`的值等于节点列表的长度。此外，因为`i`在代码添加处理程序的作用域中，该变量属于处理程序的闭包。闭包中的变量的值不是静态的，因此`i`的值不是添加处理程序时的值（对于列表来说，第一个按钮为0，对于第二个按钮为1，依此类推）。在处理程序将被执行的时候，在控制台上将打印变量`i`的当前值，等于节点列表的长度。
* 对于本题第(2)问，有多种办法可以解决这个问题。下面主要使用两种方法解决这个问题。
	- 方法1：使用立即执行函数表达式IIFE，再创建一个闭包，从而得到所期望的i的值。
	- 方法2：不使用IIFE，而是将函数移到循环的外面

```javascript
//方法1：使用IIFE
for(var i = 0;i<nodes.length;i++){
	nodes[i].addEventListener('click',(function(i){
		return function(){
			console.log('You clicked element #'+i);
		}
	})(i));
}


//方法2：不使用IIFE，将函数移动到循环的外面
function handlerWrapper(i){
	return function(){
		console.log('You clicked element #'+i);
	}
}
var nodes = document.getElementsByTagName("button");
for(var i = 0;i<nodes.length;i++){
	nodes[i].addEventListener('click',handlerWrapper(i));
}
```


## 数据类型
### 示例1

```javascript
console.log(typeof null);  //object
console.log(typeof {});  //object
console.log(typeof undefined); //undefined
console.log(typeof []);//object
var myArray = [1,2,3];
if(myArray instanceof Array){  //条件成立
	console.log('true');     //输出true
}
```
* Null类型只有一个值，就是`null`。从逻辑角度来看，null值表示一个空对象指针，因此`typeof null`会返回`Object`。而`typeof undefined`返回值是`undefined`。



## JS算法

### 质数判断

> 写一个`isPrime()`函数，当其为质数时返回`true`，否则返回`false`。

书写过程中，注意算法的优化。
* 参考[LeetCode - 204. Count Primes  | myBlog](http://liubaoshuai.com/2017/02/24/leetcode-notes-3.html)中的分析和[Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)文章。
* 注意对所给参数的类型检测。

```javascript
//类型检测
//If your browser doesn't support the method Number.isInteger 
//of ECMAScript 6, you can implement your own pretty easily
if(typeof number !== "number" || !Number.isInteger(number)){
	return false;
}
```

`Number.isInteger()`的函数实现为

```javascript
Number.isInteger = Number.isInteger || function(value) {
    return typeof value === "number" && 
           isFinite(value) && 
           Math.floor(value) === value;
};
```


* 负数不是质数，0和1也不是质数。判断时可以先将其排除。
* **2是质数中唯一的偶数，其余质数都是奇数**。
* `for(var i=3;(i*i)<number;i+=2)`循环遍历时候，从`i=3`开始循环，并且每次`i`值可以增加2，即`i+=2`，跳过偶数。循环终止条件采用`(i*i)<number`判断，可以防止开放带来的计算误差。

* 方法1

```javascript
function isPrime(number){
	//类型检测
	//If your browser doesn't support the method Number.isInteger 
	//of ECMAScript 6, you can implement your own pretty easily
	if(typeof number !== "number" || !Number.isInteger(number)){
		return false;
	}
	//负数不是质数，0和1都不是质数
	if(number < 2){
		return false;
	}
	if(number === 2){
		return true;
	}
	//排除偶数
	else if(number % 2 === 0){
		return false;
	}
	for(var i=3;(i*i)<number;i+=2){  //排除偶数  i+=2
		if(number % i === 0){
			return false;
		}
	}
	return true;
}
```

## 作用域(Scope)

### 示例1

```javascript
(function(){
	var a = b =5;
})();
console.log(a);  //Uncaught ReferenceError: a is not defined
console.log(b);  //5
```
* 在立即执行函数表达式`IIFE`中，有两个赋值。但是，其中变量`a`使用关键词`var`来声明，这就意味着`a`是这个函数的局部变量。与此相反，`b`没有使用`var`声明，被分配给了全局作用域（非严格模式下），是全局变量。
* 上述程序中，`console.log(a);`会输出`Uncaught ReferenceError: a is not defined`；`console.log(b);`会输出`5`。
* 如果上述程序中使用严格模式(`"use strict;"`)，那么`console.log(b);`语句就会输出`Uncaught ReferenceError: a is not defined`错误。这是因为严格模式下必须显示地引用全局作用域。

```javascript
(function(){
	var a = b =5;
})();
console.log(a);  //Uncaught ReferenceError: a is not defined
console.log(b);  //Uncaught ReferenceError: a is not defined
```

* 使用`window.b`，则可以直接引用全局变量。此时，严格模式下访问`b`，会正常输出`5`。
 
```
(function(){
	"use strict";
	var a = window.b =5;
})();
console.log(a);  //Uncaught ReferenceError: a is not defined
console.log(b);   // 5
```


### 示例2

```javascript
var a = 1;
function test1(){
	var a =2;  //local
	console.log(a);  
}
test1();  //2
console.log(a); //1

var b = 1;
function test2(){
	b =2;   //global
	console.log(b);  
}
test2();  //2
console.log(b); //2
```

## Array.prototype.reverse()
> The reverse() method reverses an **array** in place. The first array element becomes the last, and the last array element becomes the first.  --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
>


* 调用`Array.prototype.reverse()`函数时，会改变传入的Array参数。

```javascript
var a = ['one', 'two', 'three'];
var reversed = a.reverse(); 

console.log(a);        // ['three', 'two', 'one']
console.log(reversed); // ['three', 'two', 'one']
```

* `Array.prototype.reverse()`函数的传入参数是Array数组类型。因此，若要对字符串String进行反转操作，需要先将其转换为数组Array类型。利用`split()`和`join()`函数可以实现该操作。

```javascript
var str = "LiuBaoshuai";
var newStr = str.split("").reverse().join("");
console.log(str);  // LiuBaoshuai
console.log(newStr);  // iauhsoaBuiL
```

> The `split()` method splits a **String** object into an **array** of strings by separating the string into substrings.
> The `join()` method joins all elements of an **array** (or an array-like object) into a **string**.


## Array.prototype.slice()

> The slice() method returns **a shallow copy** of a portion of an array into a new array object selected from begin to end (end not included). The original array will not be modified.   --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

* `slice()` does not alter the original array . It returns **a shallow copy** of elements from the original array.  Elements of the original array are copied into the returned array as follows
	- For object references (and not the actual object), `slice` copies object references into the new array. Both the original and new array refer to the same object. **If a referenced object changes, the changes are visible to both the new and original arrays.**
	- For strings, numbers and booleans (not String, Number and Boolean objects), `slice` copies the values into the new array. **Changes to the string, number or boolean in one array does not affect the other array.**
* **If a new element is added to either array, the other array is not affected.**

```javascript
// Using slice, create newCar from myCar.
var myHonda = { color: 'red', wheels: 4, engine: { cylinders: 4, size: 2.2 } };
var myCar = [myHonda, 2, 'cherry condition', 'purchased 1997'];
var newCar = myCar.slice(0, 2);

// Display the values of myCar, newCar, and the color of myHonda
//  referenced from both arrays.
console.log('myCar = ' + JSON.stringify(myCar));
//myCar = [{"color":"red","wheels":4,"engine":{"cylinders":4,"size":2.2}},2,"cherry condition","purchased 1997"]
console.log('newCar = ' + JSON.stringify(newCar));
//newCar = [{"color":"red","wheels":4,"engine":{"cylinders":4,"size":2.2}},2]
console.log('myCar[0].color = ' + myCar[0].color);
//myCar[0].color = red
console.log('newCar[0].color = ' + newCar[0].color);
//newCar[0].color = red

// Change the color of myHonda. (Reference value)
myHonda.color = 'purple';
console.log('The new color of my Honda is ' + myHonda.color);
//The new color of my Honda is purple

// Display the color of myHonda referenced from both arrays.
console.log('myCar[0].color = ' + myCar[0].color);
//myCar[0].color = purple
console.log('newCar[0].color = ' + newCar[0].color);
//newCar[0].color = purple

//change 2nd item of array (Primitive value)
newCar[1] = 10;
console.log('The new 2nd item of newCar is ' + newCar[1]); 
//The new 2nd item of newCar is 10

// Display the 2nd item from both arrays
console.log('myCar[1] = ' + myCar[1]);  //myCar[1] = 2 
console.log('newCar[1] = ' + newCar[1]);  //newCar[1] = 10

//Add elements to newCar
newCar[newCar.length] = "addElement";
console.log('myCar = ' + myCar); 
//myCar = [object Object],2,cherry condition,purchased 1997
console.log('newCar = ' + newCar); 
//newCar = [object Object],10,addElement
```
* 改变复制的目标数组中的引用类型的值，会将源数组和复制的目标数组一起改变。
* 改变复制的目标数组中的基本类型的值，只会改变复制的目标数组，不会影响源数组。
* 向数组中添加元素，只影响其本身，其余数组不受影响。

## Array.prototype.concat()
* `Array.prototype.concat()`在深/浅复制方面的作用机制和`Array.prototype.slice()`相同。
* The concat method does not alter this or any of the arrays provided as arguments but instead returns **a shallow copy** that contains copies of the same elements combined from the original arrays. Elements of the original arrays are copied into the new array as follows:
	- Object references (and not the actual object): concat copies object references into the new array. Both the original and new array refer to the same object. That is, if a referenced object is modified, the changes are visible to both the new and original arrays. This includes elements of array arguments that are also arrays.
	- Strings, numbers and booleans (not String, Number, and Boolean objects): concat copies the values of strings and numbers into the new array.


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
