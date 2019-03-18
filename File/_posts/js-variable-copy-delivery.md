---
title: JavaScript变量复制和参数传递
date: 2017-02-26 16:35:26
categories: Front-end
tags: [Front-end,JavaScript]
keywords: Front-end,参数传递,JavaScript
toc: true
---


* 本文主要介绍JavaScript中基本类型和引用类型，并对变量复制和参数传递方式进行介绍和总结。
* 本文主要参考[JavaScript高级程序设计（第3版）](https://book.douban.com/subject/10546125/)


<!--more-->

## 基本类型和引用类型
ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。
* 基本类型值（也称为原始数据类型，primitive  type）
包括 `Undefined`，`Null`，`Boolean`，`Number`和`String`。基本类型值是按值访问的，因为可以操作保存在变量中实际的值。
* 引用类型值
如`Object`等。引用类型的值是保存在内存中的对象。与其他语言不同，**JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间**。在操作对象时，实际上是在操作对象的引用而不是实际的对象。为此，引用类型的值是按照引用访问的。（注：这种说法不严格。当复制保存着对象的某个变量时，操作的是对象的引用。但在为对象添加属性时，操作的是实际的对象。—— 图灵社区“壮壮的前端之路”注） 


### 变量的内存分配

* 基本类型值：存储在栈（stack）中的简单数据段
基本类型的值直接存储在变量访问的位置。这是因为这些原始类型占据的空间是固定的，所以可将它们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。* 

* 引用值：存储在堆（heap）中的对象
存储在变量处的值是一个指针（point），指向存储对象的内存地址。这是因为：引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。相反，放在变量的栈空间中的值是该对象存储在堆中的地址。地址的大小是固定的，所以把它存储在栈中对变量性能无任何负面影响。



![js-stack-heap.PNG](http://ol3kbaay9.bkt.clouddn.com/js-stack-heap.PNG)

不同的内存分配机制也带来了不同的访问机制。**在javascript中是不允许直接访问保存在堆内存中的对象的，所以在访问一个对象时，首先得到的是这个对象在堆内存中的地址，然后再按照这个地址去获得这个对象中的值**，这也被称为按引用访问。。而原始类型的值则是可以直接访问到的。


### 动态的属性
* 对于引用类型的值，可以为其添加/改变/删除属性和方法。
* 但是对于基本类型的值，并不能为其添加/改变/删除属性和方法，尽管这样并不会导致任何错误。

```javascript
var person = new Object();
person.name = "LiuBaoshuai";
alert(person.name);  // "LiuBaoshuai"

var name = "LiuBaoshaui";
name.age = 27;
alert(name.age);  // undefined
```

### 复制变量值  
 * 对于基本类型的值，复制变量时，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。

```javascript
var num1 = 5;
var num2 = num1;
num2 += 3;   //num2 = 8
num1;        //num1 = 5  
```
上述程序中，`num1`和`num2`中的值是相互独立的（`num2`只是`num1`的一个副本），这两个变量可以参与任何操作而不会互相影响。下图形象地展示了复制基本类型值的过程。
![js-varibale-1.PNG](http://ol3kbaay9.bkt.clouddn.com/js-varibale-1.PNG)

* 对于引用类型的值，复制变量时，同样也会将存储在变量对象中的值复制一份放到为新变量分配的空间中。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。复制操作结束后，两个变量实际上将引用同一个对象。因此，改变其中一个变量，就会影响另一个变量。

```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "LiuBaoshaui";
alert(obj2.name);   // "LiuBaoshuai"
```
`obj1`和`obj2`都指向同一个对象。这样，当为obj1添加name属性后，可以通过obj2来访问这个属性，因为这两个变量引用的是同一个属性。下图展示了保存在变量对象中的变量和保存在堆中对象之间的这种关系。

![](http://ol3kbaay9.bkt.clouddn.com/js-varibale-2.PNG)


## 传递参数
* **ECMAScript 中所有函数的参数都是按值传递的。** 也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。基本类型值的传递如同基本类型变量的复制一样。引用类型值的传递如图引用类型变量的复制一样。
* **访问变量有按值和按引用两种方式，而参数只能按值传递。**
* 在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数，或者用ECMAScript 的概念来说，就是 `arguments` 对象中的一个元素）。

```javascript
function addTen(num){    
    num +=10;    
    return num;
}
var count = 20;
var result = addTen(count);
// count和函数内部的num是两个变量
console.log(count);   //20  没有变化！！ 
alert(result);  //30
```

* 在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。

```javascript
function setName(obj){
    obj.name = "LiuBaoshuai";
}
var person = new Object();
// person和函数内部的obj引用的是同一个对象
setName(person);
console.log(person.name);  //"LiuBaoshaui"
```

* 有很多开发人员错误地认为：在局部作用域中修改的对象会在全局作用域中反映出来，就说明参数是按引用传递的。为了证明对象是按值传递的，请参见如下代码

```javascript
function setName(obj){
    obj.name = "LiuBaoshuai";
    //重写obj，此时obj变量引用的是一个局部对象，不再和person引用同一个对象
    obj = new Object();
    obj.name = "lbs0912";
}
var person = new Object();
setName(person);
console.log(person.name);  //"LiuBaoshaui"
```

上述程序中，在函数内部重写了`obj`对象，将一个新的变量赋值给变量obj。如果person是按引用传递的，那么person就会自动被修改为指向其name属性值为`lbs0912`的新对象。但是，在接下来访问`person.name`时，显示的仍然是`"LiuBaoshuai"`。这说明即使在函数内部修改了参数的值，但原始的引用仍然保持不变。

实际上，当在函数内部重写obj时，这个变量引用的就是一个局部对象了，而这个局部对象会在函数执行完毕后立即被销毁。

```javascript
function setName(obj){
    //重写obj，此时obj变量引用的是一个局部对象，不再和person引用同一个对象
    var obj = new Object();
    obj.name = "lbs0912";
}
var person = new Object();
setName(person);
console.log(person.name);  //undefined
```

上述程序中在重写`obj`对象之后（`obj`成为局部对象），`obj`对象和`person`不再引用同一个对象。因此，访问`person.name`时，将输出`undefined`。
 


 对于下面的程序， `obj1`仍然指向原来的对象，之所以`value`改变了，是因`changeStuff`函数中第一条语句` obj.value = '333';`。该语句将`obj`指向了`obj1`，改变了obj的value值，同时也会改变obj1的value值，因为二者引用了同一个对象。

此后，obj对象指向了新的对象obj2，断开了和obj1的关系，此时obj1的value值不再受后续操作的影响，仍然保持` obj1.value = '333';`。
 

如果是按引用传递的话，这个时候`obj1.value`应该是等于'222'的。因此，该程序也能证明**ECMAScript 中所有函数的参数都是按值传递的。**

> 按值传递(call by value)时，函数的形参是被调用时所传实参的副本。修改形参的值并不会影响实参。
> 
> 按引用传递(call by reference)时，函数的形参接收实参的隐式引用，而不再是副本。这意味着函数形参的值如果被修改，实参也会被修改。同时两者指向相同的值。
> 
> 按引用传递会使函数调用的追踪更加困难，有时也会引起一些微妙的BUG。按值传递由于每次都需要克隆副本，对一些复杂类型，性能较低。因此，两种传值方式都有各自的问题。

```javascript
var obj1 = {
  value:'111'
};
var obj2 = {
  value:'222'
};
function changeStuff(obj){
  obj.value = '333';
  obj = obj2;
  return obj.value;
}
var foo = changeStuff(obj1);
console.log(foo);    // '222' 参数obj指向了新的对象obj2
console.log(obj1.value);//'333'
```


 
*参考阅读*
[1] [ECMAScript 原始值和引用值](http://www.w3school.com.cn/js/pro_js_value.asp)
[2] [JS是按值传递还是按引用传递?](http://bosn.me/js/js-call-by-sharing/)
[3] [StackOverflow](http://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language?lq=1)
 
## 变量比较

* 基本类型值的比较： 比较变量实际的值

```javascript
var a = "apple";
var b = "apple";
a == b;     // true
```

* 引用类型值的比较：比较变量的地址值（而不是引用对象的值）

数组`a`和数组`b`的内容是一样的，但是其地址值是不同的。比较引用类型的值时，比较的是其地址值，它们在内存中是两个Object，地址不一样，所以比较的结果是`false`。

将数组`a`复制给数组`c`，此时二者引用的是同一个对象，地址指向相同，因此比较结果为`true`。
 
 
```javascript
var a = ["apple"];
var b = ["apple"];
var c = a;
// Array也是一种Object，属于引用类型值
console.log(a == b);    // false
console.log(a == c);    // true
console.log(a == a);    // true
```

### `NaN == NaN` returns false
* Reference [StackOverflow](http://stackoverflow.com/questions/19955898/why-is-nan-nan-false)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
