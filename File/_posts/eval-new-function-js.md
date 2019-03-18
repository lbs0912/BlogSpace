---
title: 通过eval()和new Function()来动态执行JavaScript代码
date: 2017-02-12 11:35:48
categories: Front-end
tags: [Front-end,动态执行代码]
keywords: front-end,动态执行代码

---

本文主要介绍如何通过eval()和new Function()来动态执行JavaScript代码，并对两种方法中的作用域和优缺点进行了对比。

<!--more-->

# 使用eval()动态执行代码

## 语法

其调用格式为`eval(str)`。
> The eval() function evaluates JavaScript code represented as a string.  --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)

```javascript
var a = 12;
eval( 'a+5' );  //17

//注意语句上下文 eval()的解析：
eval( '{ foo: 123 }' ); //123
eval( '({ foo: 123 })' );    // { foo: 123 }
```

> eval()在语句的上下文中解析它的参数。如果希望eval返回一个对象，需要用小括号将对象字面量括起来。

## 在严格模式中使用eval()
应该在严格模式下使用`eval()`。在非严格模式下，`eval()`所执行的代码会在当前的作用域中创建局部变量（本地变量）。

```javascript
function sloppyFunc(){
    eval( 'var foo = 123' );  //add to the scope of sloppyFunc
    console.log( foo ); // 123
}
```
而在严格模式下，`eval()`所执行的代码并不会在当前的作用域中创建局部变量（本地变量）。

```javascript
function sloppyFunc(){
	'use strict';
    eval( 'var foo = 123' );  
    console.log( foo ); // ReferenceError: foo is not defined
```
然而在严格模式下，`eval()`所执行的代码仍然可以读写当前上下文中的变量。如果要防止这样的读写，需要间接地调用`eval()`。
```javascript
function sloppyFunc(){
	'use strict';
	var x = 10;
    eval( x+20 );  // 30
```
  
## 在全局作用域中间接调用eval()

执行eval() 的方式有2种：

* 直接调用：直接调用eval函数。此时`eval(str)`是在当前作用域（本地作用域）中执行代码的。
* 间接调用：通过将`eval()`存储在另一个名称下并通过`call()`方法来调用或者通过改变eval函数的名称来调用或者通过将eval改变成一个方法的方式来调用，call是window对象的方法。此时`eval(str)`是在全局作用域中执行代码的。

```javascript
// method 1: 直接调用
var x = 'global';
function directEval(){
    'use strict';
    var x = 'local';
    console.log( eval('x') ); // local
}

// method 2: 间接调用
var x = 'global';

function indirectEval(){
    'use strict';
    var x = 'local';

    // 不同方式调用 call  间接调用eval
    console.log( eval.call(null, 'x') ); // global
    console.log( window.eval('x') ); // global
    console.log( (1,eval)('x') ); // global (1)
	
	// change the name of eval
    var xeval = eval;
    console.log( xeval('x') ); // global
	
	//turn eval into a method
    var obj = { eval: eval }
    console.log( obj.eval('x') ); // global
}
```
下面对 (1)处进行说明。当你通过变量的名字来调用它，这个初始值叫做引用。引用是一个具有2个主字段的数据结构。

* base：指向当前的环境，即当前变量值所存储的数据结构。
* referencedName：变量的名称。

当调用eval()时，函数的执行运算符（即括号）将引用传入eval执行，并决定要调用哪个函数。如此便直接调用了eval()。然而你也可以不传递给括号运算符引用来强制地间接调用eval()。它的实现是在调用括号运算符之前先存下引用的值。在（1）这行的都好运算符就做了这件事。逗号运算符执行了第一个运算值，并将第二个运算值进行返回。而这个执行过程中总是会返回值。这意味着引用已经被解析，而函数名将不再有效。

间接调用方式始终是非严格模式的。这就造成了代码与其当前上下文环境的隔离。

```javascript
function strictFunc(){
    'use strict';
    var code = '(function(){ return this; }())';
    var result = eval.call( null, code );
    console.log( result !== undefined ); // true sloppy mode
}
```
# 通过new Function()执行代码

## 语法
`Function()`构造器的形式如下
```javascript
new Function(param1,...,paramN,funcBody)
``` 
构造器会生成一个函数，创建出来的函数如下所示。
```javascript
function (param1,...,paramN){
	funcBody;
}
```

## new Function()会创建全局作用域的函数

与eval()间接调用方式类似，new Function()会创建全局作用域的函数。

```javascript
var x = 'global';

function strictFunc(){
    'use strict';

    var x = 'local';
    var f = new Function('return x');
    console.log( f() ); //global
}
```
 默认情况下，这些函数也是处于非严格模式下的。
 
```javascript
function strictFunc(){
    'use strict';

    var sl = new Function( 'return this' );
    console.log( sl() !== undefined ); // true, sloppy mode

    var st = new Function( '"use strict"; return this;' );
    console.log( st() !== undefined ); // true, strict mode
}
```


# eval()与new Function()比较
通常来说，尽可能地使用new Function()来代替eval执行代码。相比之下，前者的参数更为清晰，你不必使用间接的eval()调用来确保所执行的代码除了其自己的作用域只能访问全局的变量。

# 最佳实践
总的来说，最好避免使用`eval()`和`new Fuction()` 。动态执行代码相对会比较慢并且存在安全隐患。另外对于一些工具（如IDE）也不能很好地进行代码的额静态分析。

我们有更好的替代方案。例如，Brendan Eich提出的[反模式](https://www.kancloud.cn/kancloud/learn-js-design-patterns/56473)：开发者通过`propName`来存储一个属性。

```javascript
var value = eval('obj.'+propName);
```
这个问题的意义在于，**点运算符只支持固定传入属性**。这意味着这种请款下，属性名在运行时必须确定。这就是为什么上面的例子要使用eval()来配合访问属性。幸好，JavaScript还有一个括号运算符，它可以动态接受属性值。因此，**括号运算符用来动态访问属性名更好**。

```javascript
var value = obj[propName];
```

另外，不要使用eval或者new Function来解析JSON数据。因为这是不安全的。你可以使用ECMASCript 5内置的JSON方法或是借助库来解析JSON数据。

## 合理的使用场景
也有一些对于eval()和new Function()的合理的使用场景，尽管这些场景偏高级。例如，通过函数（JSON数据不允许），模板库，编译器，命令行以及模块系统配置数据。

# 总结
动态执行代码在JavaScript是一个相对高级的话题。如果想深入了解这块，可以参考如下链接。

* [Kangax-Global eval. What are the options?](http://perfectionkills.com/global-eval-what-are-the-options/)
* [Kangax GitHub](http://kangax.github.io/)
* [MDN-eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 