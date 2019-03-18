---
title: 前端面试复习-3
date: 2017-04-14 11:35:48
categories: Front-end
tags: [Front-end]
keywords: 前端面试复习
---




* 本文主要对前端面试中常考察的知识点进行记录和归纳。


<!--more-->

## Bootstrap
* Bootstrap的所有JavaScript插件都依赖于jQuery库，要确保在使用这些功能的时候引用相应的jQuery文件。


## HTTP状态码
参考资料
* [维基百科](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)
* [W3School](http://www.w3school.com.cn/tags/html_ref_httpmessages.asp)

HTTP状态码分为五大类 

### 1xx 消息

### 2xx 成功

* 200 OK
请求成功（其后是对GET和POST请求的应答文档）
* 204 No Content 
服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。

### 3xx 重定向

* 301 Moved Permanently
被请求的资源已**永久**移动到新位置

* 302 Found/Move temporarily
所请求的页面已经**临时**转移至新的url。

### 4xx  客户端错误

*  401 Unauthorized 
发送的请求需要有通过HTTP 认证（BASIC 认证、
DIGEST 认证）的认证信息。

* 403 Forbidden	
对被请求页面的访问被禁止。
* 404 Not Found	
服务器无法找到被请求的页面。

### 5xx  服务器错误

* 500 Internal Server Error
服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器端的源代码出现错误时出现。
* 502 Bad Gateway	
请求未完成。服务器从上游服务器收到一个无效的响应。
* 504 Gateway Timeout	
网关超时。

## call apply bind
参考资料
* [JavaScript中call,apply,bind方法的总结](http://www.cnblogs.com/pssp/p/5215621.html)
* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)


### call 

call()方法在使用一个指定的this值和若干个指定的参数值的前提下调用某个函数或方法。语法如下。

```javascript
fun.call(thisArg[, arg1[, arg2[, ...]]])
```
### apply

apply() 方法在指定 this 值和参数（参数以**数组**或类数组对象的形式存在）的情况下调用某个函数。

```javascript
fun.apply(thisArg[, argsArray])
```
apply中的第2个参数必须是一个**数组**。

### bind
bind()方法会创建一个**新函数**。当这个新函数被调用时，bind()的第一个参数将作为它运行时的 this, 之后的一序列参数将会在传递的实参前传入作为它的参数。

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```


### 结论

* call和apply都可以用于改变this的指向。但是call中参数直接单个传入，apply中第2个参数要以数组形式传入。
* **如果call和apply的第一个参数写的是null，那么this指向的是window对象。**

```javascript
var user = "window.user";
var a = {
    user:"Liu Baoshaui",
    fn:function(){
        console.log(this.user); 
    }
}
var b = a.fn;
b.apply(a);  // "Liu Baoshuai"
b.apply(null); // "window.user"
```

*  bind函数的返回值是由指定的this值和初始化参数改造的原函数拷贝。


```javascript
var a = {
    user:"Liu Baoshuai",
    fn:function(){
        console.log(this.user);
    }
}
var b = a.fn;
b.call(a); //"Liu Baoshaui"
b.bind(a);  //没有输出结果
//bind的返回值是由指定的this值和初始化参数改造的原函数拷贝
```

上述程序中，使用call或apply改变this对象后，可以执行该函数，并输出结果。但是，如果使用bind，则并不会输出结果，因为bind的返回值是由指定的this值和初始化参数改造的原函数拷贝。


对上述程序做如下修改。

```javascript
var a = {
    user:"Liu Baoshuai",
    fn:function(){
        console.log(this.user);
    }
}
var b = a.fn;
var c = b.bind(a);
c(); //"Liu Baoshuai"
console.log(c); //function() { [native code] }
```

* call, apply和bind都可以接收多个参数。但是，apply要以数组形式接收参数。call和bind函数中参数直接单个传入。 多个参数传入的时候，注意参数的顺序。

```javascript
var a = {
    user:"Liu Baoshuai",
    fn:function(e,d,f){
        console.log(this.user); //Liu Baoshaui
        console.log(e,d,f); //10 1 2
    }
}
var b = a.fn;
var c = b.bind(a,10);
c(1,2);  // 10 1 2
```

* call和apply都是改变上下文中的this，并**立即执行**这个函数。bind方法会返回由指定的this值和初始化参数改造的原函数拷贝，**并不会立即执行该函数。**


## AJAX
Ajax是异步JavaScript和XML，用于在Web页面中实现异步数据交互。

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

### 优点

* 可以使得页面不重载全部内容的情况下加载局部内容，降低数据传输量
* 避免用户不断刷新或者跳转页面，提高用户体验

### 缺点

* 对搜索引擎不友好
* 要实现Ajax下的前后退功能成本较大
* 可能造成请求数的增加
* 跨域问题限制

## JSON
JSON是一种轻量级的数据交换格式，ECMA的一个子集。
JSON是一种数据交换格式，它通过可以key/value或数组的形式保存数据。

JSON的结构包含2种，一种是key/value形式，另外一种是数组格式，后者用于处理复杂的JS对象。

ECMAScript 5中提供了2个函数用于JSON值和JavaScript值的转换
* JSON.stringify(): 将JS值转换为JSON值
* JSON.parse(): 将JSON值转换为JS值

###优点
* 轻量级、易于人的阅读和编写
* 便于机器（JavaScript）解析，
* 支持复合数据类型（数组、对象、字符串、数字）


## 如何发起跨域请求

常见的跨域请求方法有：CORS、JSONP、postMessage等。

### CORS
Cross-Origin Resource Sharing，跨源资源共享

IE8+ 中使用 XDominRequest 实现 CORS，使用方法和 XMLHttpRequest 类似，区别参见《JavaScript 高级程序设计》P582

```javascript
var xdr = new XDomainRequest(); // IE8+
xdr.onload = function(){
	alert(xdr.responseText);
};
xdr.open("get", "http://www.somewhere-else.com/page/");
xdr.send(null);
```

其他浏览器对CORS的实现如下所示。

要请求位于另一个域中的资源，使用标准的 XHR 对象并在 open()方法中传入绝对 URL

```javascript
xhr.open("GET", "https://api.github.com/user",true);
```

在 HTTP 请求头部加入额外的Origin信息

```javascript
Origin: http://sstruct.github.io/
```
此外，还需要设置服务器`Access-Control-Allow-Origin` 头部回发相同的源信息（如果是公共资源，可以回发”*”）。例如

```javascript
Access-Control-Allow-Origin: http://sstruct.github.io/
```

### JSONP
JSON with padding，填充式 JSON 或参数式 JSON

JSONP 和 JSON 差不多，只不过是被当作函数参数的 JSON

```javascript
function handleResponse(response){
	alert("You’re at IP address " + response.ip + ", which is in " +
		response.city + ", " + response.region_name);
}
var script = document.createElement("script");
script.src = "http://freegeoip.net/json/?callback=handleResponse";
document.body.insertBefore(script, document.body.firstChild);
```

参见：《JavaScript 高级程序设计》P587

JSONP 的缺点也要讲一下
* 首先，JSONP 是从其他域中加载代码执行。如果其他域不安全，很可能会在响应中夹带一些恶意代码。
* 其次，难以确定 JSONP 请求是否失败。

### postMessage
HTML5 新增方法(IE8+)，语法如下

```javascript
otherWindow.postMessage(message, targetOrigin);
```

## HTTP请求信息组成
HTTP请求信息由3部分组成
*  请求方法URI协议/版本
*  请求头(Request Header)
*   请求正文

## 闭包
闭包是有权访问另一个函数作用域中的变量的函数。闭包指那些能够访问独立(自由)变量的函数 (变量在本地使用，但定义在一个封闭的作用域中)。换句话说，这些函数可以“记忆”它被创建时候的环境。

创建闭包的常见方式，就是在一个函数内部创建另外一个函数。

**当一个函数返回它内部定义的一个函数时，就产生了一个闭包，闭包不但包括被返回的函数，还包括这个函数的定义环境。**

```javascript
var generateClosure = function(){
	var count = 0;
    var get = function(){
        count ++;
        return count;
    };
    return   get;
};

var counter = generateClosure();
console.log(counter());   //   输出   1
console.log(counter());   //   输出   2
console.log(counter());   //   输出   3
```

在generateClosure() 返回 get 函数时，私下将 get 可能引用到的 generateClosure() 函数的内部变量（也就是 count 变量）也返回了，并在内存中生成了一个副本。之后 generateClosure() 返回的函数的两个实例 counter1和 counter2 就是相互独立的了。


### 闭包的用途
#### 实现嵌套的回调函数
嵌套的回调函数中，由于闭包机制的存在，即使外层函数已经执行完毕，其作用域内申请的变量也不会释放，因为里层的函数还有可能引用到这些变量，这样就完美地实现了嵌套的异步回调。
#### 实现私有成员

```javascript
var generateClosure = function(){
	var count = 0;
    var get = function(){
        count ++;
        return count;
    };
    return   get;
};

var counter = generateClosure();
console.log(counter());   //   输出   1
console.log(counter());   //   输出   2
console.log(counter());   //   输出   3
```
只有调用 counter() 才能访问到闭包内的 count 变量，并按照规则对其增加1，除此之外决无可能用其他方式找到 count 变量。

受到这个简单例子的启发，我们可以把一个对象用闭包封装起来，只返回一个“访问器”的对象，即可实现对细节隐藏，从而实现私有成员。

## 浏览器跨域访问解决方案
### 跨域的概念
不同地址，不同端口，不同级别，不同协议都会构成跨域。例如：`about.haorooms.com`和`www.haorooms.com`都会构成跨域。总结起来**只要协议、域名、端口有任何一个不同，都被当作是不同的域。**

### 解决方案1  postMessage
参考[window.postMessage](http://www.haorooms.com/post/window_postMessage)。
#### 基本介绍
window.postMessage虽然说是html5的功能，但是支持IE8+，假如你的网站不需要支持IE6和IE7，那么可以使用window.postMessage。

window.postMessage是客户端和客户端直接的数据传递，既可以跨域传递，也可以同域传递。

#### 应用场景
假如你有一个页面，页面中拿到部分用户信息，点击进入另外一个页面，另外的页面默认是取不到用户信息的，你可以通过window.postMessage把部分用户信息传到这个页面中。（当然，你要考虑安全性等方面。）

### 解决方案2 CORS
CORS 跨域资源共享

CORS与JSONP相比，无疑更为先进、方便和可靠。
*  JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
*  使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
*  JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS。[低版本IE7以下不支持，要支持IE7还是要用jsonp方式]

### 解决方案3 JSONP


## Cookie-LocalStorage-SessionStorage
参考资料
* [详说 Cookie, LocalStorage 与 SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)


* Cookie大小限制为4KB，主要用来保存登录信息。
* localStorage是HTML5中加入的新技术，用于本地存储。localStorage除非被清除，否则将永久保存。大小限制一般为5M。
* sessionStorage保存数据的生命周期与localStorage不同。sessionStorage可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。当页面关闭后，sessionStorage中的数据就会被清空。sessionStorage大小限制一般为5M。
* cookie每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题。而localStorage和sessionStorage仅在客户端（浏览器）中保存，不参与和服务器的通信。



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 