---
title: JavaScript中apply(), call()和bind()比较
date: 2017-03-14 23:21:00
tags: [Front-end,JavaScript]
categories: Front-end
---





* 本文主要对 JavaScript中apply(), call()和bind()进行整理和记录。

<!--more-->

## Function.prototype.call()
> The `call()` method calls a function with a given this value and arguments **provided individually**. --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
>


### Syntax

```javascript
function.call(thisArg, arg1, arg2, ...)
```


## Function.prototype.apply()
> The `apply()` method calls a function with a given this value and arguments **provided as an array (or an array-like object)**. --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
>


### Syntax

```javascript
fun.apply(thisArg, [argsArray])
```



## Function.prototype.bind()
> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called. --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
>


### Syntax

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

## apply()和call()比较
* 相同点：二者都用来改变函数运行时的上下文，即改变函数体内部的this指向。

```javascript
function fruits() {}
 
fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
 
var apple = new fruits;
apple.say();    //My color is red
var banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
```

* 不同点：二者接受参数的方式不一样。`call()`需要将参数按顺序传入，`apply()`则是将参数一数组的形式传入。

```javascript
var func = function(arg1, arg2) {
};
func.call(this, arg1, arg2);
func.apply(this, [arg1, arg2])
```

## apply()和call()的应用实例和笔试题
### 数组之间追加


```javascript
var array1 = [12 , "foo" , {name "Joe"} , -2458]; 
var array2 = ["Doe" , 555 , 100]; 
Array.prototype.push.apply(array1, array2); 
console.log(array1);
/* array1 值为  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```
### 获取数组中的最大值和最小值

```javascript
var numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
var maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```
number 本身没有 max 方法，但是 Math 有。因此，就可以借助 call 或者 apply 使用其方法。

### 验证是否是数组
（前提是`toString()`方法没有被重写过）

```javascript
functionisArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```

### 类（伪）数组使用数组方法

```javascript
var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
```
* Javascript中存在一种名为伪数组的对象结构。比较特别的是 `arguments` 对象，还有像调用 `getElementsByTagName` , `document.childNodes` 之类的。它们返回`NodeList`对象都属于伪数组，不能应用 Array下的 `push` , `pop` 等方法。
* 但是能通过 `Array.prototype.slice.call` 可以将其转换为具有 `length` 属性的对象（length属性是真正的数组才具有的属性），这样 domNodes 就可以应用 Array 下的所有方法了。

### 笔试题1
定义一个 log 方法，让它可以代理 `console.log` 方法。并且为每一个 log 消息添加一个”(app)”的前辍。

```javascript
log("hello world");    //(app)hello world
```
#### 求解
常规求解如下代码所示，可以解决最基本的需求，但是当传入参数的个数是不确定的时候，下面的方法就失效了。
 
```javascript
function log(msg)　{
  console.log(msg);
}
log(1);    //1
log(1,2);    //1
```

这个时候，就可以考虑使用 apply 或者 call。注意，这里传入的参数个数是不确定的，所以使用apply是最好的，方法如下。

```javascript
function log(){  //此处不要写arguments ！！因为调用的是arguments伪数组
  console.log.apply(console, arguments);
};
log(1);    //1
log(1,2);    //1 2
```
需要注意的是，`function log()`中括号中参数需要为空，因为函数体内调用的是`arguments`伪数组参数，而并不是log()函数传入的参数。
 
下面考虑log信息前app前缀的实现。这个时候，需要考虑到`arguments`参数是个伪数组，通过 Array.prototype.slice.call 转化为标准数组，再使用数组方法`unshift`，就可以实现该功能。因此，该笔试题最终代码为

```javascript
function log(){
	var args = Array.prototype.slice.call(arguments);
	//或者使用apply
    var args = Array.prototype.slice.apply(arguments);
	args.unshift('(app)');
    console.log.apply(console, args);
};
```
 
## Function.prototype.bind()用法
> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called. --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
>


`bind()`方法会创建一个新函数，称为绑定函数。当这个新函数（绑定函数）被调用时，`bind()`的第一个参数将作为它运行时的`this`， 之后的一序列参数将会在传递的实参前传入作为它的参数。

### Syntax

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

### 使用bind()保存上下文
在常见的单体模式中，通常会使用 `_this` , `that` , `self` 等保存 `this`，这样就可以在改变了上下文之后继续引用到它。如下程序所示。

```javascript
var foo = {
    bar : 1,
    eventBind: function(){
        var _this = this;
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(_this.bar);     //1
        });
    }
}
```
由于 Javascript 特有的机制，上下文环境在 `eventBind:function(){ }` 过渡到 `$(‘.someClass’).on(‘click’,function(event) { })` 发生了改变，上述使用变量保存 `this` 这些方式都是有用的，也没有什么问题。当然使用 `bind()` 可以更加优雅的解决这个问题。

```javascript
var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(this.bar);      //1
        }.bind(this));
    }
}
```
在上述代码里，`bind()` 创建了一个函数，当这个click事件绑定在被调用的时候，它的 this 关键词会被设置成被传入的值（这里指调用bind()时传入的参数）。因此，这里我们传入想要的上下文 this(其实就是 foo )，到 bind() 函数中。然后，当回调函数被执行的时候， this 便指向 foo 对象。


### 多次调用bind()
* 示例1

```javascript
var x = 6;
var bar = function(){
    console.log(this.x);
}
var foo = {
    x: 3
} 
bar(); //6  this指向全局作用域
var func = bar.bind(foo);
func(); // 3   this指向foo对象
```

这里我们创建了一个新的函数（绑定函数）func。当使用 bind() 创建一个绑定函数之后，它被执行的时候，它的 this 会被设置成 foo ， 而不是像我们调用 bar() 时的全局作用域。

* 示例2

```javascript
var bar = function(){
    console.log(this.x);
}
var foo = {
    x:3
}
var sed = {
    x:4
}
var func = bar.bind(foo).bind(sed);
func(); // 3
 
var fiv = {
    x:5
}
var func = bar.bind(foo).bind(sed).bind(fiv);
func(); //3
```
上述程序中多次使用了`bind()`。程序输出结果两次都是3，而非期待中的4和5。
 
 原因是，**在Javascript中，多次 bind() 是无效的。**
 
> 更深层次的原因， bind() 的实现，相当于使用函数在内部包了一个 `call / apply`，第二次 bind() 相当于再包住第一次 bind()，故第二次以后的 bind 是无法生效的。 —— [深入浅出妙用 Javascript 中 apply、call、binds](http://web.jobbole.com/83642/)
>


## apply，call和bind比较

```javascript
var obj = {
    x: 81,
};
var foo = {
    getX: function() {
        return this.x;
    }
}
//bind创建的是一个绑定函数，因此最后需要加上(),表示立即执行。
//如果不加上(),则只会创建一个绑定函数，而不会立即执行。
console.log(foo.getX.bind(obj)());  //81 
var bindFunc = foo.getX.bind(obj);
console.log(bindFunc());   //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```
上述程序中，使用 bind() 方法时，后面多了对括号，表示立即执行。（bind创建了一个绑定函数）。如果不需要立即执行，可以去掉括号。

因此，有如下结论：
**当希望改变上下文环境之后并非立即执行，而是回调执行的时候，使用 bind() 方法。而 apply/call 则会立即执行函数。**

## 总结
 * apply，call，bind 三者都是用来改变函数的this对象的指向的。
 * apply，call，bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文。
 * apply，call，bind 三者都可以利用后续参数传参。
 * bind 是返回对应函数，便于稍后调用；apply和call 则是立即调用 。

## 参考资料
[1] [深入浅出妙用 Javascript 中 apply、call、binds](http://web.jobbole.com/83642/)
[2] [call()、apply()、bind()的用法](http://wwsun.github.io/posts/javascript-functions.html)




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 