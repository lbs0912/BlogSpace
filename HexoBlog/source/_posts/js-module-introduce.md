---
title: JS模块化——CommonJS AMD CMD ES6 Module
date: 2019-03-28 10:35:26
categories: Front-End Develop
tags: [Front-End Developer,JS]
keywords: JS,Module,AMD,CMD,CommonJS
toc: true
password:
comments: true
top:
---


* 对 JS 常见的模块化方案进行介绍和比较——`CommonJS` `AMD` `CMD` `ES6 Module`
* 对 `ES6 Module` 和 `CommonJS` 的差异进行对比，介绍循环依赖和动态 `import()`

<!--more-->



## 更新日志
* 2018/05/08，撰写
* 2019/03/27，内容整理
* 2019/04/01，动态 `import()` 和博文发表


## 参考资料
* [AMD, CMD, CommonJS和UMD | Segmentfault](https://segmentfault.com/a/1190000004873947)
* [JS模块化加载之CommonJS、AMD、CMD、ES6](https://zhuanlan.zhihu.com/p/41231046)
* [ES6 module的加载和实现 | 阮一峰](http://es6.ruanyifeng.com/#docs/module-loader)
* [前端模块化开发方案小对比](https://div.io/topic/1078)

## 模块化开发

### 优点

*  模块化开发中，通常一个文件就是一个模块，有自己的作用域，只向外暴露特定的变量和函数，并且可以按需加载。
*  依赖自动加载，按需加载。
*  提高代码复用率，方便进行代码的管理，使得代码管理更加清晰、规范。
*  减少了命名冲突，消除全局变量。
*  目前流行的js模块化规范有CommonJS、AMD、CMD以及ES6的模块系统


### 常见模块化规范
* CommonJs (Node.js)
* AMD (RequireJS)
* CMD (SeaJS)



## CommonJS(Node.js)

**CommonJS是服务器模块的规范**，Node.js采用了这个规范。

根据 `CommonJS` 规范，一个单独的文件就是一个模块，每一个模块都是一个单独的作用域，在一个文件定义的变量（还包括函数和类），**都是私有的**，对其他文件是不可见的。

**CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。** 

`CommonJS` 中，加载模块使用 `require` 方法。该方法读取一个文件并执行，最后返回文件内部的 `exports` 对象。


> `Node.js` 主要用于服务器编程，加载的模块文件一般都已经存在本地硬盘，加载起来较快，不用考虑异步加载的方式，所以 `CommonJS` 的同步加载模块规范是比较适用的。
> 
> 但如果是浏览器环境，要从服务器加载模块，这是就必须采用异步模式。所以就有了 `AMD`，`CMD` 等解决方案。


```
var x = 5;
var addX = function(value) {
  return value + x;
};

module.exports.x = x;
module.exports.addX = addX;


// 也可以改写为如下
module.exports = {
  x: x,
  addX: addX,
};
```

```
let math = require('./math.js');
console.log('math.x',math.x);
console.log('math.addX', math.addX(4));
```

## AMD (RequireJS) 异步模块定义

* `AMD`  = `Asynchronous Module Definition`，即 *异步模块定义*。
* **`AMD` 规范加载模块是异步的，并允许函数回调，不必等到所有模块都加载完成，后续操作可以正常执行。**
* `AMD` 中，使用 `require` 获取依赖模块，使用 `exports` 导出 `API`。



```
//规范 API
define(id?, dependencies?, factory);
define.amd = {};


// 定义无依赖的模块
define({
    add: function(x,y){
        return x + y;
    }
});


// 定义有依赖的模块
define(["alpha"], function(alpha){
    return {
        verb: function(){
            return alpha.verb() + 1;
        }
    }
});
```


### 异步加载和回调

> **require([module], callback)** 中 `callback` 为模块加载完成后的回调函数

```
//加载 math模块，完成之后执行回调函数
require(['math'], function(math) {
　math.add(2, 3);
});
```

### RequireJS


`RequireJS` 是一个前端模块化管理的工具库，遵循 `AMD` 规范，`RequireJS` 是对 `AMD` 规范的阐述。

`RequireJS` 基本思想为，通过一个函数来将所有所需的或者所依赖的模块装载进来，然后返回一个新的函数（模块）。后续所有的关于新模块的业务代码都在这个函数内部操作。




`RequireJS` 要求每个模块均放在独立的文件之中，并使用 `define` 定义模块，使用 `require` 方法调用模块。

按照是否有依赖其他模块情况，可以分为 *独立模块* 和 *非独立模块*。

* 独立模块，不依赖其他模块，直接定义

```
define({
    method1: function(){},
    method2: function(){}
});

//等价于
define(function() {
    return {
        method1: function(){},
        method2: function(){}
    }
});
```

* 非独立模块，依赖其他模块


```
define([ 'module1', 'module2' ], function(m1, m2) {
    ...
});

//等价于
define(function(require) {
    var m1 = require('module1');
    var m2 = require('module2');
    ...
});
```


* `require` 方法调用模块

```
require(['foo', 'bar'], function(foo, bar) {
    foo.func();
    bar.func();
});
```


## CMD (SeaJS) 

`CMD`  = `Common Module Definition`，即 *通用模块定义*。`CMD` 是 `SeaJS` 在推广过程中对模块定义的规范化产出。

CMD规范和AMD类似，都主要运行于浏览器端，写法上看起来也很类似。主要是区别在于 **模块初始化时机**

* **AMD中只要模块作为依赖时，就会加载并初始化**
* **CMD中，模块作为依赖且被引用时才会初始化，否则只会加载。**
* `CMD` 推崇依赖就近，`AMD` 推崇依赖前置。
* `AMD` 的 `API` 默认是一个当多个用，`CMD` 严格的区分推崇职责单一。例如，`AMD` 里 `require` 分全局的和局部的。CMD里面没有全局的 `require`，提供 `seajs.use()` 来实现模块系统的加载启动。`CMD` 里每个 `API` 都简单纯粹。

```
//AMD
define(['./a','./b'], function (a, b) {
 
    //依赖一开始就写好
    a.test();
    b.test();
});
 
//CMD
define(function (requie, exports, module) {
     
    //依赖可以就近书写
    var a = require('./a');
    a.test();
     
    ...
    //软依赖
    if (status) {
        var b = requie('./b');
        b.test();
    }
});
```








### Sea.js
* [Sea.js Github Page](https://github.com/seajs/seajs)
* [SeaJS与RequireJS最大的区别](https://www.douban.com/note/283566440/)

使用Sea.js，在书写文件时，需要遵守CMD（Common Module Definition）模块定义规范。一个文件就是一个模块。


#### 用法

* 通过 `exports` 暴露接口。这意味着不需要命名空间了，更不需要全局变量。这是一种彻底的命名冲突解决方案。
* 通过 `require` 引入依赖。这可以让依赖内置，开发者只需关心当前模块的依赖，其他事情 `Sea.js` 都会自动处理好。对模块开发者来说，这是一种很好的 关注度分离，能让程序员更多地享受编码的乐趣。
* 通过 `define` 定义模块，更多详情参考[SeasJS | 极客学院](http://wiki.jikexueyuan.com/project/hello-seajs/usage-guide.html)。

#### 示例

例如，对于下述`util.js`代码

```
var org = {};
org.CoolSite = {};
org.CoolSite.Utils = {};

org.CoolSite.Utils.each = function (arr) {
  // 实现代码
};

org.CoolSite.Utils.log = function (str) {
  // 实现代码
};
```

可以采用SeaJS重写为

```

define(function(require, exports) {
  exports.each = function (arr) {
    // 实现代码
  };

  exports.log = function (str) {
    // 实现代码
  };
});
```

通过 `exports` 就可以向外提供接口。通过 `require('./util.js')` 就可以拿到 `util.js` 中通过 `exports` 暴露的接口。这里的 `require`  可以认为是 `Sea.js` 给 JavaScript 语言增加的一个语法关键字，**通过 `require` 可以获取其他模块提供的接口。**

```
define(function(require, exports) {
  var util = require('./util.js');
  exports.init = function() {
    // 实现代码
  };
});
```

## SeaJS与RequireJS区别
二者区别主要表现在**模块初始化时机**

* **AMD（RequireJS）中只要模块作为依赖时，就会加载并初始化。即尽早地执行（依赖）模块。相当于所有的require都被提前了，而且模块执行的顺序也不一定100%就是require书写顺序。**
* **CMD（SeaJS）中，模块作为依赖且被引用时才会初始化，否则只会加载。即只会在模块真正需要使用的时候才初始化。模块加载的顺序是严格按照require书写的顺序。**

![amd-cmd-nodek](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/front-end-2019/amd-cmd-nodek.png)

从规范上来说，AMD 更加简单且严谨，适用性更广，而在RequireJS强力的推动下，在国外几乎成了事实上的异步模块标准，各大类库也相继支持AMD规范。

但从SeaJS与CMD来说，也做了很多不错东西：1、相对自然的依赖声明风格 2、小而美的内部实现 3、贴心的外围功能设计 4、更好的中文社区支持。


## UMD

* `UMD` = `Universal Module Definition`，即通用模块定义。`UMD` 是`AMD` 和 `CommonJS`的糅合。

> `AMD` 模块以浏览器第一的原则发展，异步加载模块。
> `CommonJS` 模块以服务器第一原则发展，选择同步加载。它的模块无需包装(unwrapped modules)。
> 这迫使人们又想出另一个更通用的模式 `UMD`（Universal Module Definition)，实现跨平台的解决方案。

* `UMD` 先判断是否支持 `Node.js` 的模块（`exports`）是否存在，存在则使用 `Node.js` 模块模式。再判断是否支持 `AMD`（`define` 是否存在），存在则使用 `AMD` 方式加载模块。

```
(function (window, factory) {
    if (typeof exports === 'object') {
     
        module.exports = factory();
    } else if (typeof define === 'function' && define.amd) {
     
        define(factory);
    } else {
     
        window.eventUtil = factory();
    }
})(this, function () {
    //module ...
});
```


## ES6 模块

### ES6模块和CommonJS区别

* **ES6 模块输出的是值的引用，输出接口动态绑定，而 `CommonJS` 输出的是值的拷贝。**
* **`CommonJS` 模块是运行时加载，ES6 模块是编译时输出接口。**

#### CommonJS 输出值的拷贝

**CommonJS 模块输出的是值的拷贝（类比于基本类型和引用类型的赋值操作）。对于基本类型，一旦输出，模块内部的变化影响不到这个值。对于引用类型，效果同引用类型的赋值操作。**

```
// lib.js
var counter = 3;
var obj = {
    name: 'David'
};

function changeValue() {
    counter++;
    obj.name = 'Peter';
};

module.exports = {
    counter: counter,
    obj: obj,
    changeValue: changeValue,
};
```

```
// main.js
var mod = require('./lib');

console.log(mod.counter);  // 3
console.log(mod.obj.name);  //  'David'
mod.changeValue();
console.log(mod.counter);  // 3
console.log(mod.obj.name);  //  'Peter'

// Or
console.log(require('./lib').counter);  // 3
console.log(require('./lib').obj.name);  //  'Peter'
```

* `counter` 是基本类型值，模块内部值的变化不影响输出的值变化。
* `obj` 是引用类型值，模块内部值的变化影响输出的值变化。
* 上述两点区别，类比于基本类型和引用类型的赋值操作。


也可以借助取值函数（`getter`），将 `counter` 转为引用类型值，效果如下。

> 在类的内部，可以使用 `get` 和 `set` 关键字，对某个属性设置存执函数和取值函数，拦截该属性的存取行为。 —— [class | 阮一峰](http://es6.ruanyifeng.com/#docs/class)

```
// lib.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
  get counter() {
    return counter
  },
  incCounter: incCounter,
};
```

```
// main.js
var mod = require('./lib');

console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 4
```




#### ES6 输出值的引用

ES6 模块是动态关联模块中的值，输出的是值得引用。**原始值变了，`import` 加载的值也会跟着变。**

> `ES6` 模块的运行机制与 `CommonJS` 不一样。JS 引擎对脚本静态分析时，遇到模块加载命令 `import`，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。**ES6 模块中，原始值变了，`import` 加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值**。  —— [ES6 Module 的加载实现 | 阮一峰](http://es6.ruanyifeng.com/#docs/module-loader)

```
// lib.js
export let counter = 3;
export function incCounter() {
  counter++;
}

// main.js
import { counter, incCounter } from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```


#### CommonJS 运行时加载 ES6静态编译 
`CommonJS` 模块是运行时加载，ES6 模块是编译时输出接口。

这是因为，**`CommonJS` 加载的是一个对象**（即 `module.exports` 属性），该对象只有在脚本运行完才会生成。而 **`ES6` 模块不是对象**，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

ES6 模块是编译时输出接口，因此有如下2个特点
* `import` 命令会被 JS 引擎静态分析，优先于模块内的其他内容执行
* `export` 命令会有变量声明提升的效果

##### import 优先执行

在文件中的任何位置引入 `import` 模块都会被提前到文件顶部

```
// a.js
console.log('a.js')
import { foo } from './b';

// b.js
export let foo = 1;
console.log('b.js 先执行');

// 执行结果:
// b.js 先执行
// a.js
```

虽然 `a` 模块中 `import` 引入晚于 `console.log('a')`，但是它被 JS 引擎通过静态分析，提到模块执行的最前面，优于模块中的其他部分的执行。

##### export 命令变量提升效果


由于 `import` 和 `export` 是静态执行，所以 `import` 和 `export` 具有变量提升效果。即 `import` 和 `export` 命令在模块中的位置并不影响程序的输出。

```
// a.js
import { foo } from './b';
console.log('a.js');
export const bar = 1;
export const bar2 = () => {
  console.log('bar2');
}
export function bar3() {
  console.log('bar3');
}

// b.js
export let foo = 1;
import * as a from './a';
console.log(a);

// 执行结果:
// { bar: undefined, bar2: undefined, bar3: [Function: bar3] }
// a.js
```

`a` 模块引用了 `b` 模块，`b` 模块也引用了 `a` 模块，`export` 声明的变量也是优于模块其它内容的执行的。但具体对变量赋值需要等到执行到相应代码的时候。



### ES6模块和CommonJS相同点


#### 模块不会重复执行

重复引入某个相同的模块时，模块只会执行一次。


###  循环依赖

#### CommonJS 模块循环依赖

CommonJS 模块的重要特性是加载时执行，即脚本代码在 `require` 的时候，就会全部执行。一旦出现某个模块被“循环加载”，**就只输出已经执行的部分，还未执行的部分不会输出。**

##### Demo 1

```
//a.js
exports.done = false;
var b = require('./b.js');
console.log('在 a.js 之中，b.done = %j', b.done);
exports.done = true;
console.log('a.js 执行完毕');
```

上面代码之中，`a.js` 脚本先输出一个 `done` 变量，然后加载另一个脚本文件 `b.js`。注意，此时 `a.js` 代码就停在这里，等待 `b.js` 执行完毕，再往下执行。

再看 `b.js` 的代码。

```
//b.js
exports.done = false;
var a = require('./a.js');
console.log('在 b.js 之中，a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完毕');
```

上面代码之中，`b.js` 执行到第二行，就会去加载 `a.js`，这时，就发生了“循环加载”。系统会 `a.js` 模块对应对象的 `exports` 属性取值，可是因为 `a.js` 还没有执行完，从 `exports` 属性只能取回已经执行的部分，而不是最后的值。

`a.js` 已经执行的部分，只有一行。

```
exports.done = false;
```

因此，对于 `b.js`来说，它从 `a.js` 只输入一个变量 `done`，值为 `false`。

然后，**`b.js` 接着往下执行，等到全部执行完毕，再把执行权交还给 `a.js`**。于是，`a.js` 接着往下执行，直到执行完毕。我们写一个脚本 `main.js`，验证这个过程。

```
var a = require('./a.js');
var b = require('./b.js');
console.log('在 main.js 之中, a.done=%j, b.done=%j', a.done, b.done);
```

执行 `main.js`，运行结果如下。

```
$ node main.js

在 b.js 之中，a.done = false
b.js 执行完毕
在 a.js 之中，b.done = true
a.js 执行完毕
在 main.js 之中, a.done=true, b.done=true
```

上面的代码证明了2点
* 在 `b.js` 之中，`a.js` 没有执行完毕，只执行了第一行
* `main.js` 执行到第二行时，不会再次执行 `b.js`，而是输出缓存的 `b.js` 的执行结果，即它的第四行。

```
exports.done = true;
```

总之，**CommonJS 输入的是被输出值的拷贝，不是引用。**


另外，由于 CommonJS 模块遇到循环加载时，返回的是当前已经执行的部分的值，而不是代码全部执行后的值，两者可能会有差异。所以，输入变量的时候，必须非常小心。

```
var a = require('a'); // 安全的写法 导入整体，保证module已经执行完成
var foo = require('a').foo; // 危险的写法

exports.good = function (arg) {
  return a.foo('good', arg); // 使用的是 a.foo 的最新值
};

exports.bad = function (arg) {
  return foo('bad', arg); // 使用的是一个部分加载时的值
};
```

上面代码中，如果发生循环加载，`require('a').foo` 的值很可能后面会被改写，改用 `require('a')` 会更保险一点。




##### Demo 2


```
// a.js
console.log('a starting');
exports.done = false;
const b = require('./b');
console.log('in a, b.done =', b.done);
exports.done = true;
console.log('a done');

// b.js
console.log('b starting');
exports.done = false;
const a = require('./a');
console.log('in b, a.done =', a.done);
exports.done = true;
console.log('b done');

// node a.js
// 执行结果：
// a starting
// b starting
// in b, a.done = false
// b done
// in a, b.done = true
// a done
```



从上面的执行过程中，可以看到，在 CommonJS 规范中，当遇到 `require()` 语句时，会执行 `require` 模块中的代码，**并缓存执行的结果，当下次再次加载时不会重复执行，而是直接取缓存的结果。正因为此，出现循环依赖时才不会出现无限循环调用的情况。**



#### ES6 模块循环依赖

**跟 CommonJS 模块一样，ES6 不会再去执行重复加载的模块，又由于 ES6 动态输出绑定的特性，能保证 ES6 在任何时候都能获取其它模块当前的最新值。**



### 动态 import()

ES6 模块在编译时就会静态分析，**优先于模块内的其他内容执行**，所以导致了我们无法写出像下面这样的代码


```
if(some condition) {
  import a from './a';
}else {
  import b from './b';
}

// or 
import a from (str + 'b');
```

因为编译时静态分析，导致了我们无法在条件语句或者拼接字符串模块，因为这些都是需要在运行时才能确定的结果在 ES6 模块是不被允许的，所以 动态引入`import()` 应运而生。



`import()` 允许你在运行时动态地引入 ES6 模块，想到这，你可能也想起了 `require.ensure` 这个语法，但是它们的用途却截然不同的。

`require.ensure` 的出现是 `webpack` 的产物，它是因为浏览器需要一种异步的机制可以用来异步加载模块，从而减少初始的加载文件的体积，所以如果在服务端的话， `require.ensure` 就无用武之地了，因为服务端不存在异步加载模块的情况，模块同步进行加载就可以满足使用场景了。 CommonJS 模块可以在运行时确认模块加载。

而 `import()` 则不同，它主要是为了解决 ES6 模块无法在运行时确定模块的引用关系，所以需要引入 `import()`。

先来看下它的用法

* 动态的 `import()` 提供一个基于 `Promise` 的 `API`
* 动态的 `import()` 可以在脚本的任何地方使用 `import()` 接受字符串文字，可以根据需要构造说明符


```
// a.js
const str = './b';
const flag = true;
if(flag) {
  import('./b').then(({foo}) => {
    console.log(foo);
  })
}
import(str).then(({foo}) => {
  console.log(foo);
})

// b.js
export const foo = 'foo';

// babel-node a.js
// 执行结果
// foo
// foo
```

当然，如果在浏览器端的 `import()` 的用途就会变得更广泛，比如 按需异步加载模块，那么就和 `require.ensure` 功能类似了。



因为是基于 `Promise` 的，所以如果你想要同时加载多个模块的话，可以是 `Promise.all` 进行并行异步加载。

```
Promise.all([
  import('./a.js'),
  import('./b.js'),
  import('./c.js'),
]).then(([a, {default: b}, {c}]) => {
    console.log('a.js is loaded dynamically');
    console.log('b.js is loaded dynamically');
    console.log('c.js is loaded dynamically');
});
```

还有 `Promise.race` 方法，它检查哪个 `Promise` 被首先 `resolved` 或 `reject`。我们可以使用 `import()` 来检查哪个 `CDN` 速度更快：

```
const CDNs = [
  {
    name: 'jQuery.com',
    url: 'https://code.jquery.com/jquery-3.1.1.min.js'
  },
  {
    name: 'googleapis.com',
    url: 'https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js'
  }
];

console.log(`------`);
console.log(`jQuery is: ${window.jQuery}`);

Promise.race([
  import(CDNs[0].url).then(()=>console.log(CDNs[0].name, 'loaded')),
  import(CDNs[1].url).then(()=>console.log(CDNs[1].name, 'loaded'))
]).then(()=> {
  console.log(`jQuery version: ${window.jQuery.fn.jquery}`);
});
```

当然，如果你觉得这样写还不够优雅，也可以结合 `async/await` 语法糖来使用。

```
async function main() {
  const myModule = await import('./myModule.js');
  const {export1, export2} = await import('./myModule.js');
  const [module1, module2, module3] =
    await Promise.all([
      import('./module1.js'),
      import('./module2.js'),
      import('./module3.js'),
    ]);
}
```

动态 `import()` 为我们提供了以异步方式使用 ES 模块的额外功能。

根据我们的需求动态或有条件地加载它们，这使我们能够更快，更好地创建更多优势应用程序。




## webpack中加载3种模块 | 语法
Webpack允许使用不同的模块类型，但是`底层`必须使用同一种实现。所有的模块可以直接在盒外运行。

* ES6 模块

```
import MyModule from './MyModule.js';
```

* CommonJS(Require)

```
var MyModule = require('./MyModule.js');
```

* AMD

```
define(['./MyModule.js'], function (MyModule) {
});
```



