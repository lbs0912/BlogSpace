---
title: JavaScript中的浅拷贝和深拷贝
date: 2017-09-07 11:15:26
categories: JavaScript
tags: [Front-end,JavaScript]
keywords: Front-end,JavaScript
toc: true
---


* [JS深拷贝和浅拷贝 | 掘金社区](https://juejin.im/post/59ac1c4ef265da248e75892b)

<!--more-->





## 堆和栈
**深拷贝和浅拷贝的主要区别就是其在内存中的存储类型不同。**

堆和栈都是内存中划分出来用来存储的区域。

栈（stack）为自动分配的内存空间，它由系统自动释放。

堆（heap）则是动态分配的内存，大小不定也不会自动释放。

## ECMAScript 的数据类型

### 基本数据类型值不可变

* **基本数据类型（`undefined`，`boolean`，`number`，`string`，`null`）存放在栈中。基本数据类型值不可变。**



**基本数据类型值不可改变**，参考如下例程。

```javascript
var str = "abc";
console.log(str[1]="f");    // f
console.log(str);           // abc
```

上述修改字符串的值，实际上是返回一个修改后的字符串，原字符串保持不变。

更进一步的，修改基本数据类型的值，都是返回一个新的基本数据类型，原基本数据类型是保持不变的。

### 引用数据类型值可直接改变

* **引用数据类型（`object`）存放在堆中，可以直接改变它的值。**

```javascript
var arr = [1,2,3];
console.log(arr[0]=4);    // 4
console.log(arr);         // [4,2,3]
```





**引用类型（object）是存放在堆内存中的，变量实际上是一个存放在栈内存的指针，这个指针指向堆内存中的地址。** 每个空间大小不一样，要根据情况开进行特定的分配，例如

```javascript
var person1 = {name:'jozo'};
var person2 = {name:'xiaom'};
var person3 = {name:'xiaoq'};
```

![js-shallow-copy-1](http://ol3kbaay9.bkt.clouddn.com/js-shallow-copy-1.JPG)



### 基本类型的比较是值的比较

基本类型的比较是值的比较，只要它们的值相等就认为它们是相等的。

```javascript
var a = 1;
var b = 1;
console.log(a === b);//true
```

比较的时候最好使用严格等，因为 `==` 是会进行类型转换的，比如

```javascript
var a = 1;
var b = true;
console.log(a == b);//true
```

### 引用类型的比较是引用的比较

**对JS中的引用类型进行操作的时候，都是操作其对象的引用（保存在栈内存中的指针），所以比较两个引用类型，是看其的引用是否指向同一个对象。** 例如

```javascript
var a = [1,2,3];
var b = [1,2,3];
console.log(a === b); // false
```

虽然变量`a`和变量`b`都是表示一个内容为`1，2，3`的数组，但是其在内存中的位置不一样，也就是说变量`a`和变量`b`指向的不是同一个对象，所以它们是不相等的。

![js-shallow-copy-1](http://ol3kbaay9.bkt.clouddn.com/js-shallow-copy-2.JPG)



## 赋值中的传值与传址

### 基本数据类型的赋值是传值
基本数据类型的赋值（`=`）是在内存中新开辟一段栈内存，然后再把再将值赋值到新的栈中。**基本类型的赋值的两个变量是两个独立相互不影响的变量。**

```javascript
var a = 10;
var b = a;
a ++ ;
console.log(a); // 11
console.log(b); // 10
```
![js-shallow-copy-1](http://ol3kbaay9.bkt.clouddn.com/js-shallow-copy-3.JPG)


### 基本数据类型的赋值是传址

**引用类型的赋值（`=`）是传址。只是改变指针的指向，也就是说引用类型的赋值是对象保存在栈中的地址的赋值，这样的话两个变量就指向同一个对象，因此两者之间操作互相有影响。**

```javascript
var a = {}; // a保存了一个空对象的实例
var b = a;  // a和b都指向了这个空对象
a.name = 'jozo';
console.log(a.name); // 'jozo'
console.log(b.name); // 'jozo'

b.age = 22;
console.log(b.age);// 22
console.log(a.age);// 22

console.log(a == b);// true
```

![js-shallow-copy-1](http://ol3kbaay9.bkt.clouddn.com/js-shallow-copy-4.JPG)



## 浅拷贝

* 浅复制是指当对象的字段值被复制时，字段引用的对象不会被复制。
* 浅复制仅仅复制了引用。源对象和拷贝对象引用同一个实体。改变其中的任何一个对象，都会影响另外一个对象。
* **ECMAScript中基本类型值(`Undefined`，`Null`，`Boolean`，`Number`和`String`)都是深拷贝。本文中讨论的深拷贝和浅拷贝主要针对引用类型值（如`Array`，`Object`等）。**

```javascript
//Object
var src = {
	name:"src"
}
var target = src;  //shallow copy
target.name = "target";
console.log(src.name);   //"target"
```
上述程序中，将src对象浅复制至target对象。改变target对象的属性值，也会影响到src对象。

```javascript
//Array
var src = [1,2,3];
var target = src;
target[0] =5;
console.log(src);  //[5,2,3]   
console.log(target); //[5,2,3]
```
 

### 赋值（`=`）和浅拷贝区别

**上述讲解的赋值操作不是浅拷贝**。上述赋值操作只能算作`引用`。


```javascript
var obj1 = {
    'name' : 'zhangsan',
    'age' :  '18',
    'language' : [1,[2,3],[4,5]],
};

var obj2 = obj1;

var obj3 = shallowCopy(obj1);

function shallowCopy(src) {
    var dst = {};
    for (var prop in src) {
        if (src.hasOwnProperty(prop)) {
            dst[prop] = src[prop];
        }
    }
    return dst;
}

obj2.name = "lisi";
obj3.age = "20";
obj2.language[1] = ["二","三"];
obj3.language[2] = ["四","五"];
console.log(obj1);  
//obj1 = {
//    'name' : 'lisi',
//    'age' :  '18',
//    'language' : [1,["二","三"],["四","五"]],
//};

console.log(obj2);
//obj2 = {
//    'name' : 'lisi',
//    'age' :  '18',
//    'language' : [1,["二","三"],["四","五"]],
//};

console.log(obj3);
//obj3 = {
//    'name' : 'zhangsan',
//    'age' :  '20',
//    'language' : [1,["二","三"],["四","五"]],
//};
```

上述程序中
* `obj1`：原始数据
* `obj2`：赋值操作得到
* `obj3`：浅拷贝得到

**改变基本数据类型**

改变`obj2`的`name`属性和`obj3`的`name`属性，改变赋值得到的对象`obj2`， 同时也会改变原始值`obj1`，而改变浅拷贝得到的`obj3`则不会改变原始对象 `obj1`。这就可以说明赋值得到的对象`obj2`只是将指针改变，其引用的仍然是同一个对象，而浅拷贝得到的`obj3` 则是重新创建了新对象。


**改变引用数据类型**

改变了赋值得到的对象`obj2`和浅拷贝得到的`obj3`中的`language`属性的第二个值和第三个值，无论是修改赋值得到的对象`obj2`和浅拷贝得到的`obj3`都会改变原始数据。

**这是因为浅拷贝只复制一层对象的属性，并不包括对象里面的为引用类型的数据。所以就会出现改变浅拷贝得到的`obj3`中的引用类型时，会使原始数据得到改变。**


> 深拷贝：将`B`对象拷贝到`A`对象中，包括`B`里面的子对象。**深拷贝是对对象以及对象的所有子对象进行拷贝。**
>
> 浅拷贝：将`B`对象拷贝到`A`对象中，但不包括`B`里面的子对象。

![js-shallow-copy-1](http://ol3kbaay9.bkt.clouddn.com/js-shallow-copy-5.JPG)


## 深拷贝

深拷贝是对对象以及对象的所有子对象进行拷贝。

* 深复制不是简单的复制引用，而是在堆中重新分配内存，并且把源对象实例的所有属性都进行新建复制，以保证深复制的对象的引用图不包含任何原有对象或对象图上的任何对象。
* **深复制后的对象与源对象是完全隔离的，二者之间不存在相互影响。**

### 如何实现深拷贝
思路就是递归调用浅拷贝方法，把所有属于对象的属性类型都遍历赋给另一个对象即可。

下面是`Zepto`中深拷贝的代码

```javascript
// 内部方法：用户合并一个或多个对象到第一个对象
// 参数：
// target 目标对象  对象都合并到target里
// source 合并对象
// deep 是否执行深度合并
function extend(target, source, deep) {
	for (key in source)
		if (deep && (isPlainObject(source[key]) || isArray(source[key]))) {
			// source[key] 是对象，而 target[key] 不是对象， 则 target[key] = {} 初始化一下，否则递归会出错的
			if (isPlainObject(source[key]) && !isPlainObject(target[key]))
				target[key] = {}
				// source[key] 是数组，而 target[key] 不是数组，则 target[key] = [] 初始化一下，否则递归会出错的
			if (isArray(source[key]) && !isArray(target[key]))
				target[key] = []
				// 执行递归
			extend(target[key], source[key], deep)
		}
		// 不满足以上条件，说明 source[key] 是一般的值类型，直接赋值给 target 就是了
		else if (source[key] !== undefined) target[key] = source[key]
}

// Copy all but undefined properties from one or more
// objects to the `target` object.
$.extend = function(target) {
	var deep, args = slice.call(arguments, 1);

	//第一个参数为boolean值时，表示是否深度合并
	if (typeof target == 'boolean') {
		deep = target;
		//target取第二个参数
		target = args.shift()
	}
	// 遍历后面的参数，都合并到target上
	args.forEach(function(arg) {
		extend(target, arg, deep)
	})
	return target
}
```




## 数组的拷贝

### 一维数组深拷贝的实现
* 通过逐个拷贝数组中的元素来实现一维数组的深拷贝。
* 逐个拷贝数组中元素时，每个元素不再是引用类型值(`Array`)，而是基本类型值。**基本类型值的拷贝属于深拷贝。**

```javascript
//deep copy Array
var src = [1,2,3];
var deepArray = [];
function deepCopy(src, target){
    for(var i = 0,l= src.length;i<l;i++){
        target[i] = src[i];
    }
}
deepCopy(src,deepArray);
deepArry[0] =5;
console.log(src);     //[1,2,3]   
console.log(deepArray);  //[5,2,3]   
```

### 二维数组深拷贝的实现
* 二维数组深拷贝实现过程中，需要对一维数组深拷贝中的`deepCopy()`函数进行重写，来保证每次拷贝的对象都是基本类型值。（二维数组是由一维数组组成的）

```javascript
//deep copy Array
var src = [1,[2,3,4],5];
var deepArray = [];
function deepCopy(src, target){
    for(var i = 0,l= src.length;i<l;i++){
        if(src[i] instanceof Array){   //判断是否为数组
			deepCopy(src[i]); //递归函数实现
		}
		else{    //若为基本类型值，直接进行拷贝
			target[i] = src[i];
		}
    }
}
deepCopy(src,deepArray);
deepArry[1][2] = 10;
console.log(src);     //[1,[2,3,4],5]
console.log(deepArray);  //[1,[2,3,10],5]
```


### 数组中的slice()和concat()方法




**`slice()`和`concat()`方法，对于数组元素为基本数据类型的情况，可以实现数组的深拷贝。**


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


## 对象的拷贝
如果直接对对象进行拷贝，由于其是引用类型值，因此，会进行浅拷贝。

### 对象深拷贝的实现
* 实现思路同数组的深拷贝，逐项对对象的属性进行拷贝。由于对象的属性值是基本类型值，因此，逐项对对象的属性进行拷贝，可以实现对象的深拷贝。
* 对象的深拷贝的实现代码如下

```javascript
//深复制一个对象
for (var key in children){
    if(children.hasOwnProperty(key)) {  //排除继承的属性
        cloneObject[key] = children[key];  //基本类型值的复制
    }
}
```
* 由于JS中对象有可能存在继承关系。因此，可以调用`if(children.hasOwnProperty(key))`语句来排除继承的属性，即只拷贝对象本身的属性，不拷贝该对象继承而来的属性值。具体实例代码如下所示。
 

```javascript
function Test(){
    this.name='xiaohong',
    this.age=18,
    this.run =function(){
        console.log('run');
    }
}
var test = new Test();
console.log(test.age);   // 18
test.run();   // 'run'

function ChilrTest () {
    this.name = 'xiaogang',
    this.age =15,
    this.sing =function(){
        console.log('sing');
    }
};

ChilrTest.prototype =  new Test();    //继承

var children = new ChilrTest();
children.sing();   // 'sing'
children.run();    // 'run'
children.age;    //15

console.log('----childre的属性（包括继承的属性）----') ;
for (var key in children){
	//程序会查询children的原型链，从父元素中查询到run属性
    console.log(key) ;   // name, age, sing, run
}

console.log('----childre的属性（不包括继承的属性）----') ;
for (var key in children){
    if(children.hasOwnProperty(key)) {  
        //只输出children本身的属性，不包括继承的属性
        console.log(key);   // name, age, sing
    }
}


var cloneObject ={};
//深复制一个对象
for (var key in children){
    if(children.hasOwnProperty(key)) {
        cloneObject[key] = children[key];  //基本类型值的复制
    }
}

console.log('----cloneObject的属性（不包括继承的属性）----') ;
for (var key in cloneObject){
	console.log(key) ;   // name, age, sing
}
 
cloneObject.age = 26;
console.log(test.age);  //18
console.log(children.age);  //15
console.log(cloneObject.age);  //26
```


## jQuery.extend
* `jQuery.extend( [deep ], target, object1 [, objectN ] )`: When two or more object arguments are supplied to `$.extend()`, properties from all of the objects are added to the target object. Arguments that are null or undefined are ignored.

> `jQuery.extend( [deep ], target, object1 [, objectN ] )`
> `deep`
Type: Boolean
If true, the merge becomes recursive (aka. **deep copy**). （注：此处指深度合并，不是深度拷贝）
> `target`
Type: Object
The object to extend. It will receive the new properties.
>`object1`
Type: Object
An object containing additional properties to merge in.
> `objectN`
Type: Object
Additional objects containing properties to merge in.     
>    --- [jQuery API](https://api.jquery.com/jquery.extend/#jQuery-extend-deep-target-object1-objectN)


###  深度合并

* Keep in mind that the target object (first argument) will be modified, and will also be returned from $.extend(). If, however, you want to preserve both of the original objects, you can do so by passing an empty object as the target

```javascript
var object = $.extend({}, object1, object2);
```

* The merge performed by `$.extend()` is not recursive by default; **if a property of the first object is itself an object or array, it will be completely overwritten by a property with the same key in the second or subsequent object.** The values are not merged. This can be seen in the example below by examining the value of banana. **However, by passing true for the first function argument, objects will be recursively merged.** 即`deep`参数可选/Boolean类型，用来指示是否深度合并对象，默认为false。如果该值为true，且多个对象的某个同名属性也都是对象，则该”属性对象”的属性也将进行合并。

* Merge two objects, modifying the first.

```javascript
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};
 
// Merge object2 into object1
$.extend( object1, object2 );

console.log(JSON.stringify(object1));  
//{"apple":0,"banana":{"price":200},"cherry":97,"durian":100}
```
`object2.banana`属性覆盖（重写）了`object1.banana`属性。


* Merge two objects recursively, modifying the first.（深度合并）

```javascript
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};
 
// Merge object2 into object1, recursively
$.extend( true, object1, object2 );
console.log(JSON.stringify(object1));  
//{"apple":0,"banana":{"weight":52,"price":200},"cherry":97,"durian":100}
```

### 深度拷贝
**`jquery.extend()`方法实现的是深度拷贝**，不是复制引用，而是创建了新的对象。创建的新对象和源对象之间不存在互相影响。

```javascript
var a = { k1: 1, k2: 2, k3: 3 };
var c ;
c=$.extend(a);           //将a对象复制到jquery对象上，并赋值给c
console.log(c === $);    //c对象指向的是$对象,所以结果true
console.log(a === $);   // false
console.log(c.k2);       //相当于$.k2，输出2
    
c.k2 = 777;
console.log(a);  //Object {k1: 1, k2: 2, k3: 3}
console.log($.k2);  //777
console.log(c.k2);  //777

var d = $.extend({}, a)
console.log(d);  //Object {k1: 1, k2: 2, k3: 3}
d.k2 = 3456;
console.log(d);  //Object {k1: 1, k2: 3456, k3: 3}
console.log(a);  //Object {k1: 1, k2: 2, k3: 3}

var test = [1,2,34,];
console.log(test); //[1, 2, 34]
var contest= $.extend([],test);
console.log(contest);  //[1, 2, 34]
contest.push(567);  
console.log(test);  //[1, 2, 34]
console.log(contest);  //[1, 2, 34, 567]
```

### 总结
* **`jQuery.extend()`或`$.extend()`实现的都是深度拷贝。**
* `jQuery.extend( [deep ], target, object1 [, objectN ] )`中deep参数为true时，实现深度合并，深度合并时，会进行递归复制。deep为false时，影响的是是否进行深度合并，不会影响深度拷贝。
* 该函数复制的对象属性包括方法在内。此外，还会复制对象继承自原型中的属性(JS内置的对象除外)。
* 参数deep的默认值为false，可以为该参数明确指定true值，但不能明确指定false值。简而言之，第一个参数不能为false值。 
* 如果参数为null或undefined，则该参数将被忽略。 
* 如果只为`$.extend()`指定了一个参数，则意味着参数target被省略。此时，**target就是jQuery对象本身**（参见深度拷贝章节例程中的`c === $`）。通过这种方式，我们可以为全局对象jQuery添加新的函数。 
* 如果多个对象具有相同的属性，则后者会覆盖前者的属性值。（参见深度合并章节的例程）


## JSON中parse和stringify
* JSON对象是ES5中引入的新的类型（支持的浏览器为IE8+），JSON对象`parse`方法可以将JSON字符串反序列化成JS对象，`stringify`方法可以将JS对象序列化成JSON字符串
* **借助这两个方法，也可以实现对象的深拷贝。其实现语句为`var target = JSON.parse(JSON.stringify(source))`**

```javascript
var source = {
    name:"source",
    child:{
        name:"child"
    }
}
var target = JSON.parse(JSON.stringify(source));  //深复制

target.name = "target";
console.log(source.name);   //source
console.log(target.name);   //target

target.child.name = "target child";
console.log(source.child.name);  //child
console.log(target.child.name);  //target child
```

* 从代码的输出可以看出，复制后的target与source是完全隔离的，二者不会相互影响。
* 这个方法使用较为简单，可以满足基本的深复制需求，而且能够处理JSON格式能表示的所有数据类型，但是对于正则表达式类型、函数类型等无法进行深复制(而且会直接丢失相应的值)，同时如果对象中存在循环引用的情况也无法正确处理。
* 该方法的深复制会抛弃对象的`constructor`。也就是深复制之后，无论这个对象原本的构造函数是什么，在深复制之后都会变成Object。


## 比较各个深复制方法

![js-shallow-copy.PNG](http://ojx8u3g1z.bkt.clouddn.com/js-shallow-copy.PNG)

参考[深入剖析 JavaScript 的深复制](https://segmentfault.com/a/1190000002801042)了解更多。
 
 

## 自定义复制方法
### 实现1
* 目标要求
	- 对于任何对象，它可能的类型有`Boolean`, `Number`, `Date`, `String`, `RegExp`, `Array` 以及 `Object`（所有自定义的对象全都继承于Object）
	- 复制后必须保留对象的构造函数信息（从而使新对象可以使用定义在prototype上的函数）

```javascript
Object.prototype.clone = function () {
    var Constructor = this.constructor;  //保存构造函数
    var obj = new Constructor();

    for (var attr in this) {
        if (this.hasOwnProperty(attr)) {
            if (typeof(this[attr]) !== "function") {
                if (this[attr] === null) {
                    obj[attr] = null;
                }
                else {
                    obj[attr] = this[attr].clone();
                }
            }
        }
    }
    return obj;
};
```
定义在`Object.prototype`上的`clone()`函数是整个方法的核心，对于任意一个非js预定义的对象，都会调用这个函数。而对于所有js预定义的对象，如`Number`, `Array`等，需要实现一个辅助`clone()`函数来实现完整的克隆过程

```javascript
/* Method of Array */
Array.prototype.clone = function () {
    var thisArr = this.valueOf();
    var newArr = [];
    for (var i=0; i<thisArr.length; i++) {
        newArr.push(thisArr[i].clone());
    }
    return newArr;
};

/* Method of Boolean, Number, String*/
Boolean.prototype.clone = function() { return this.valueOf(); };
Number.prototype.clone = function() { return this.valueOf(); };
String.prototype.clone = function() { return this.valueOf(); };

/* Method of Date*/
Date.prototype.clone = function() { return new Date(this.valueOf()); };

/* Method of RegExp*/
RegExp.prototype.clone = function() {
    var pattern = this.valueOf();
    var flags = '';
    flags += pattern.global ? 'g' : '';
    flags += pattern.ignoreCase ? 'i' : '';
    flags += pattern.multiline ? 'm' : '';
    return new RegExp(pattern.source, flags);
};
```

### 实现2
 
自己编写一个复制函数，根据传入参数deep的值，实现浅复制或者深复制。
 
```javascript
//util作为判断变量具体类型的辅助模块
var util = (function(){
    var class2type = {};
    ["Null","Undefined","Number","Boolean","String","Object","Function","Array","RegExp","Date"].forEach(function(item){
		class2type["[object "+ item + "]"] = item.toLowerCase();
    })
 
    function isType(obj, type){
        return getType(obj) === type;
    }
    function getType(obj){
        return class2type[Object.prototype.toString.call(obj)] || "object";
    }
    return {
        isType:isType,
        getType:getType
    }
})();
 
function copy(obj,deep){
    //如果obj不是对象，那么直接返回值就可以了
    if(obj === null || typeof obj !== "object"){
	    return obj;
    }
　　//定义需要的局部变量，根据obj的类型来调整target的类型
    var i, target = util.isType(obj,"array") ? [] : {},value,valueType;
    for(i in obj){
        value = obj[i];
        valueType = util.getType(value);
　　  　//只有在明确执行深复制，并且当前的value是数组或对象的情况下才执行递归复制
        if(deep && (valueType === "array" || valueType === "object")){
            target[i] = copy(value);
        }
        else{
            target[i] = value;
        }
    }
    return target;
}
```

## 参考资料
* [JS深拷贝和浅拷贝 | 掘金社区](https://juejin.im/post/59ac1c4ef265da248e75892b)！！！
* [ javascript中的浅拷贝和深拷贝](http://blog.csdn.net/yisuowushinian/article/details/45544343)

* [也来谈一谈js的浅复制和深复制](http://www.cnblogs.com/tracylin/p/5346314.html)

* [在js中的深复制实现方法](https://segmentfault.com/a/1190000000501320)

* [深入剖析 JavaScript 的深复制](https://segmentfault.com/a/1190000002801042)

* [Javascript中的一种深复制实现](http://jerryzou.com/posts/deepcopy/)



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 