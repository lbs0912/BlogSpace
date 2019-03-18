---
title: JS类型转换
date: 2017-05-22 14:35:26
categories: Front-end
tags: [Front-end,JavaScript]
keywords: JavaScript
toc: true
---





* 本文主要对JS中类型转换知识点进行梳理和归纳总结。

<!--more-->




## valueOf  V.S. toString
参考[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)，二者有如下区别

* The `valueOf()` method returns the **primitive value** of the specified object.
* The `toString()` method returns a string representing the object.
* `valueOf`方法返回对象的原始值（Number，String，Boolean，Undefined，Null）。在需要时候，该方法一般由JS自动调用。当然，也可以手动显示调用。
*  `toString()`方法返回一个表示该对象的字符串。在需要时候，该方法一般由JS自动调用。当然，也可以手动显示调用。

## String类型转换
在某个操作或者运算需要字符串而该对象又不是字符串的时候，会触发该对象的 **String转换**，会将非字符串的类型尝试自动转为 String 类型。

在`valueOf`和`toString`方法都未被重写的情况下，系统内部会**首先自动**调用 `toString` 函数。

```
var obj = {name: 'Coco'};
var str = '123' + obj;
console.log(str);   //先调用toString方法
// 123[object Object]
```

### String类型转换规则
* 规则1：2种方法均未被重写

若`toString`和`valueOf`方法均未被重写， 则优先执行`toString`方法。

* 规则2：优先执行被重写的方法

若未重写`valueOf`但是重写了`toString`方法， 则优先执行`toString`方法。若`toString`方法返回值不是原始类型数据，则执行`valueOf`方法。

若未重写`toString`但是重写了`valueOf`方法， 则优先执行`valueOf`方法。若`valueOf`方法返回值不是原始类型数据，则执行`toString`方法。

* 规则3：2种方法均被重写

**若`toString`和`valueOf`方法均被重写， 则优先执行`valueOf`方法** 。

若`valueOf`方法返回值不是原始类型数据，则执行`toString`方法。若二者返回的均不是原始类型数据，则**抛出错误**。


下面针对上述规则，给出具体例子。

```javascript
/** 规则1 **/
// 2种方法均未被重写，优先调用toString方法
var obj = {name: 'Coco'};
var str = '123' + obj;
// 等价于 
//var str = '123' + obj.toString();
console.log(str);  
// 123[object Object]

/** 规则2 **/
// 优先执行被重写的方法
var obj = {
    toString: function() {
        console.log('调用了 obj.toString');
        return '1234';
    }
}

console.log(obj + '1');
// 调用了 obj.toString
// 12341


var obj = {
    valueOf: function() {
        console.log('调用了 obj.valueOf');
        return '8765';
    }
}

console.log(obj + '1');
// 调用了 obj.valueOf
// 8765


/** 规则3 **/
//2种方法均被重写，优先调用valueOf方法
var obj = {
	toString: function() {
        console.log('调用了 obj.toString');
        return '8765';
    },
    valueOf: function() {
        console.log('调用了 obj.valueOf');
        return '8765';
    },
}

console.log(obj + '1');
// 调用了 obj.valueOf
// 8765


/** 规则3 **/
//2种方法均被重写，但是均为返回原始类型值，抛出错误
var obj = {
    toString: function() {
        console.log('调用了 obj.toString');
        return {};
    },
    valueOf: function() {
        console.log('调用了 obj.valueOf')
        return {};
    }
}

console.log(obj+"1");
// 调用了 obj.toString
// 调用了 obj.valueOf
// Uncaught TypeError: Cannot convert object to primitive value
```

> 根据ES5规范11.6.1节，如果某个操作数是字符串或者能通过以下步骤转换为字符串的话，`+`将进行拼接操作。如果其中一操作数是对象（包括数组），则首先对其调用`ToPrimitive`抽象操作（规范9.1节），该抽象操作再调用`[[DefaultValue]]`（规范8.12:8节），以**数字**作为上下文。    ——《你不知道的JS（中卷）》P69



### 注意
在使用上述的3条String转换规则时，如果遇到`alert(obj)`操作，则**在`alert(obj)`操作中不管有无重写`valueOf`方法，都是执行`toString`方法。**

```
/**规则3**/
var a = {
	num:30,
    valueOf:function(){
    	return this.num;
    },
    toString:function(){
    	return 0;
    },
};

console.log(a > 1)  //true valueOf
alert(a > 1);  // false toString  

//不管有无重写valueOf方法, 都会执行toString
alert(a)  //0  toString  !!! 
console.log(a);  //Object {num: 30, valueOf: function, toString: function}

alert(a > 1)   //true  valueOf
alert(a+"abc")  //30abc  valueOf
alert(a == 30)  //  true  valueOf

```

```javascript

/** 规则2 **/
var a = {
    num:30,
    toString:function(){
    	return 0;
    }
};

console.log(a > 1)   //false  toString
alert(a > 1)   //false  toString

console.log(a);    //Object {num: 30, toString: function} 
alert(a)        //0    toString

alert(a+"abc")  // 0abc  toString
alert(a == 30)  //  false  toString

```

```javascript
/** 规则2 **/
var a = {
    num:30,
    valueOf:function(){
        return this.num;
    },
};

alert(a > 1);  //true    valueOf

console.log(a);  // Object {num: 30, valueOf: function} 
alert(a);  //   [object object] 

alert(a+"abc");  //  30abc  valueOf
alert(a == 30);  //  true   valueOf
```






## Number类型转换

当遇到一些对象需要转换成 Number 类型时，会发生Number类型转换。常见的Number类型转换情况包括
* 调用 Number() 函数，强制进行 Number 类型转换
* 调用 Math.sqrt() 这类参数需要 Number 类型的方法
* obj == 1 ，进行对比的时候
* obj + 1 , 进行运算的时候


### Number类型转换规则
* 规则1：2种方法均未被重写

若`toString`和`valueOf`方法均未被重写， 则优先执行`valueOf`方法。

Number类型转换的规则1和String类型转换的规则1恰好相反。

* 规则2：优先执行被重写的方法

若未重写`valueOf`但是重写了`toString`方法， 则优先执行`toString`方法。若`toString`方法返回值不是原始类型数据，则执行`valueOf`方法。

若未重写`toString`但是重写了`valueOf`方法， 则优先执行`valueOf`方法。若`valueOf`方法返回值不是原始类型数据，则执行`toString`方法。

* 规则3：2种方法均被重写

**若`toString`和`valueOf`方法均被重写， 则优先执行`valueOf`方法** 。

若`valueOf`方法返回值不是原始类型数据，则执行`toString`方法。若二者返回的均不是原始类型数据，则**抛出错误**。


下面针对上述规则，给出具体例子。

```javascript
/* 规则1 */
var obj = {
	value:10
};

console.log(obj + 1); 
//[object Object]1

/* 规则2 */
var obj = {
    valueOf: function() {
        console.log('调用 valueOf');
        return 5;
    }
}

console.log(obj + 1); 
// 调用 valueOf
// 6


/* 规则3 */
var obj = {
    valueOf: function() {
        console.log('调用 valueOf');
        return {};
    },
    toString: function() {
        console.log('调用 toString');
        return 10;
    }
}
console.log(obj + 1); 
// 调用 valueOf  (未返回原始类型值，则继续调用toString方法)
// 调用 toString
// 11

/* 规则3 */
var obj = {
    valueOf: function() {
        console.log('调用 valueOf');
        return {};
    },
    toString: function() {
        console.log('调用 toString');
        return {};
    }
}
console.log(obj + 1); 
// 调用 valueOf
// 调用 toString
// Uncaught TypeError: Cannot convert object to primitive value
```


### 示例

```javascript
var obj2 = {
	x: 10,
    valueOf : function(){
    	console.log("调用了valueOf");
        return this.x + 30;
    },
    toString : function(){
    	console.log("调用了toString");
        return this.valueOf() + 10;
    }
};
console.log(obj2);  
//Object {x: 10, valueOf: function, toString: function}

alert(obj2);
//alert(obj2)只会调用toString方法，而在toString方法中又调用了valueOf方法
//语句输出信息如下
//调用了toString
//调用了valueOf
//50

console.log(+obj2);  //40  valueOf
//语句输出信息如下
//调用了valueOf
//40
```





### 示例


```javascript
var a = {
    valueOf:function(){
        return 42;
    },
    toString:function(){
        return 4;
    }
};

//隐式转换
console.log(a+"");  //"42"  调用valueOf方法。
//显式转换  显式指定调用toString方法
console.log(String(a)); //"4"  
```

`String()`方法会对参数进行显式转换，并指定调用`toString`方法。

基于同样的原理，有下述示例。

```javascript
var obj = {
    valueOf:function(){
        return 3;
    },
    toString:function(){
        return 'b';
    }
};
//隐式转换  调用valueOf方法
console.log('a'+obj);  //'a3'   
//显示转换 指定调用toString方法
console.log('a'+String(obj));  //'ab'  
```


### 示例

```javascript
var a = {
	i: 10,
	valueOf: function() { return this.i+30; },
	toString: function() { return this.valueOf()+10; }
}
alert(a > 20); //  true  valueOf
alert(+aaa); // 40  valueOf
alert(aaa); // 50  toString
```
## Boolean类型转换

下述情况中会发生Boolean类型转换
* 布尔比较时
* if(obj) , while(obj) 等判断时

**简单来说，除了下述 6 个值转换结果为 false，其他全部为 true**
* undefined
* null
* -0
* 0或+0
* NaN
* ""   (空字符串)

```javascript
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false
Boolean("" // false
```

## Function转换

```javascript
function test() {
    var a = 1;
    return a;
}

console.log(test());   // 1
console.log(test);   //打印出函数定义
//function test() {
//    var a = 1;
//    return a;
//}
```
从上述程序可以看出，直接调用函数名test，会把test函数的定义重新打印一次。**其实，这里是JS自动调用了函数的valueOf方法。**

```
//两句话等价
console.log(test);   //打印出函数定义
console.log(test.valueOf());   //打印出函数定义
```
下面，对Function的valueOf函数进行重写。

![js-function-type-casting.JPG](http://ol3kbaay9.bkt.clouddn.com/js-function-type-casting.JPG)


Function转换和Number转换类似，**如果函数的valueOf方法返回的不是一个原始类型，会继续调用它的toString方法。**

```javascript
test.valueOf = function() {
    console.log('调用 valueOf 方法');
    return {};
}

test.toString= function() {
    console.log('调用 toString 方法');
    return 3;
}

test;
// 输出如下：
// 调用 valueOf 方法
// 调用 toString 方法
// 3
```

## 应用：参数柯里化
### 题目描述
> 在计算机科学中，柯里化（英语：Currying），又译为卡瑞化或加里化，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。 ——[维基百科](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)

```javascript
var foo = function(a) {
	return function(b) {
	    return a * a + b * b;
  }
}
(foo(3))(4);  //25
foo(3)(4); //25
```

编写add函数，实现下述功能。

```javascript
add(1)(2) // 3
add(1, 2, 3)(10) // 16
add(1)(2)(3)(4)(5) // 15
```

### 求解
编写add函数如下。运行可以发现，在 `add()()` 情形下是正确的，而当链式操作的参数多于两个的时候，无法返回结果。

```javascript
function add() {
    var args = Array.prototype.slice.call(arguments);

    return function() {
        var arg2 = Array.prototype.slice.call(arguments);
        return args.concat(arg2).reduce(function(a, b){
            return a + b;
        });
    }
}

console.log(add(1)(2)); // 3
console.log(add(1, 2)(3)); // 6
console.log(add(1)(2)(3)); 
// Uncaught TypeError: add(...)(...) is not a function(…)
```

本题的难点是，在add()的时候，如何既返回一个值又返回一个函数以供后续继续调用？

查阅资料得知，通过重写函数的 `valueOf` 方法或者 `toString` 方法，可以得到正确结果。

```javascript
function add () {
	var args = Array.prototype.slice.call(arguments);

	var fn = function () {
		var arg_fn = Array.prototype.slice.call(arguments);
		return add.apply(null, args.concat(arg_fn));
	}

	fn.valueOf = function () {
		return args.reduce(function(a, b) {
			return a + b;
		})
	}
	return fn;
}

console.log(add(1)(2)); // function 3
console.log(add(1, 2)(3)); // function 6
console.log(add(1)(2)(3));  //function 6
```

根据上述Function转换可知，**在调用函数名的时候，JS会自动调用函数的valueOf方法**。在上述程序中，重写了valueOf方法，供函数调用。

**当调用一次 add 的时候，实际是是返回 fn 这个 function，实际是也就是返回 fn.valueOf()**

```javascript
add(1);
// 输出如下：
// 进入add
// 调用valueOf
// 1
```
上述过程相当于

```javascript
[1].reduce(function(a, b) {
    return a + b;
})
// 1
```

当链式调用两次的时候

```javascript
add(1)(2);
// 输出如下：
// 进入add
// 调用fn
// 进入add
// 调用valueOf
// 3
```

当链式调用三次的时候

```javascript
add(1)(2)(3);
// 输出如下：
// 进入add
// 调用fn
// 进入add
// 调用fn
// 进入add
// 调用valueOf
// 6
```

可以看到，这里其实有一种循环。只有最后一次调用才真正调用到 valueOf，而之前的操作都是合并参数，递归调用本身，由于最后一次调用返回的是一个 fn 函数，所以最终调用了函数的 fn.valueOf，并且利用了 reduce 方法对所有参数求和。

除了改写 valueOf 方法，也可以改写 toString 方法。

```javascript
function add () {
	var args = Array.prototype.slice.call(arguments);

	var fn = function () {
		var arg_fn = Array.prototype.slice.call(arguments);
		return add.apply(null, args.concat(arg_fn));
	}

	fn.toString = function() {
		return args.reduce(function(a, b) {
			return a + b;
		})
	}

	return fn;
}
```

**如果只改写 valueOf() 或是 toString() 其中一个，会优先调用被改写了的方法，而如果两个同时改写，则会像 String 转换规则一样，优先查询 valueOf() 方法，在 valueOf() 方法返回的是非原始类型的情况下再查询 toString() 方法。**


## 应用：使用arguments求和
### 题目描述
参考[牛客网](https://www.nowcoder.com/practice/df84fa320cbe49d3b4a17516974b1136?tpId=6&tqId=10974&tPage=2&rp=2&ru=/ta/js-assessment&qru=/ta/js-assessment/question-ranking)了解更多。

函数 useArguments 可以接收 1 个及以上的参数。请实现函数 useArguments，返回所有调用参数相加后的结果。本题的测试参数全部为 Number 类型，不需考虑参数转换。 

```javascript
useArguments(1, 2, 3, 4);     //16
```

### 求解
```javascript
//方法1
function useArguments() {
    var arr=Array.prototype.slice.call(arguments)
    //把arguments类数组转化为数组
    return eval(arr.join("+"));//求和
}

//方法2
function useArguments() {
	var args = Array.prototype.slice.apply(arguments);
    return args.reduce(function(acc,item){
        return acc+item;
    });
}
```



## 应用：柯里化
### 题目描述
参考[牛客网](https://www.nowcoder.com/practice/bb78d69986794470969674a8b504ac00?tpId=6&tqId=10977&tPage=2&rp=2&ru=/ta/js-assessment&qru=/ta/js-assessment/question-ranking)了解更多。

已知 fn 为一个预定义函数，实现函数 curryIt，调用之后满足如下条件：
1、返回一个函数 a，a 的 length 属性值为 1（即显式声明 a 接收一个参数）
2、调用 a 之后，返回一个函数 b, b 的 length 属性值为 1
3、调用 b 之后，返回一个函数 c, c 的 length 属性值为 1
4、调用 c 之后，返回的结果与调用 fn 的返回值一致
5、fn 的参数依次为函数 a, b, c 的调用参数 

```javascript
var fn = function (a, b, c) {
	return a + b + c
}; 

curryIt(fn)(1)(2)(3);   //6
```
### 求解
柯里化是把接受多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数且返回结果的新函数的技术。

简单理解题目意思，就是指，我们将预定义的函数的参数逐一传入到curryIt中，当参数全部传入之后，就执行预定义函数。

于是，我们首先要获得预定义函数的参数个数fn.length，然后声明一个空数组去存放这些参数。返回一个匿名函数接收参数并执行，当参数个数小于fn.length，则再次返回该匿名函数，继续接收参数并执行，直至参数个数等于fn.length。最后，调用apply执行预定义函数。

```javascript
function curryIt(fn) {
     //获取fn参数的数量
     var n = fn.length;
     //声明一个数组args
     var args = [];
     //返回一个匿名函数
     return function(arg){
         //将curryIt后面括号中的参数放入数组
         args.push(arg);
         //如果args中的参数个数小于fn函数的参数个数，
         //则执行arguments.callee（其作用是引用当前正在执行的函数，这里是返回的当前匿名函数）。
         //否则，返回fn的调用结果
         if(args.length < n){
            return arguments.callee;
         }else return fn.apply("",args);
     }
 }

var fn = function (a, b, c) {
	return a + b + c
}; 

console.log(curryIt(fn)(1)(2)(3));   //6
```

也可以不用 arguments.callee 的方法，用变量名代替，和上面的方法大同小异。

```javascript
function curryIt(fn) {
    var length = fn.length,
        args = [];
    var result =  function (arg){
        args.push(arg);
        length --;
        if(length <= 0 ){
            return fn.apply(this, args);
        } else {
            return result;
        }
    }
     
    return result;
}
```

## 参考资料
* [js的 valueOf & toString](http://blog.csdn.net/gjc9620/article/details/46708415)
* [一道面试题引发的对javascript类型转换的思考](https://github.com/chokcoco/cnblogsArticle/issues/15)
* [ECMAScript 6 规范 | 中文镜像](http://yanhaijing.com/es5/#101)


 

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 