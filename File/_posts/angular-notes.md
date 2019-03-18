---
title: AngularJS学习笔记
date: 2016-10-09 10:32:27
tags: [Front-end, Angular]
comments: true
categories: Front-end
---
本文主要记录了学习AngularJS过程中的个人笔记。
<!-- more -->
## Angular基础知识
### Angular参考资料
* [AngularJS Offical Website](https://angularjs.org/)
* [AngularJS 实战](https://book.douban.com/subject/26642222/) & [随书源码](https://github.com/lbs0912/angular-practice/edit/master/ch02/2-11.html)
* [用AngularJS开发下一代Web应用](https://book.douban.com/subject/25752512/)
* [AngularJS Online Tutorial](http://www.runoob.com/angularjs/angularjs-tutorial.html)
### Angular适用范围
由于[Angular](https://angularjs.org/)是构建一个**MVC**类结构的JavaScript库，是一个开源的代码类库，因此，为了更好体现它的优势，建议在构建一个`CRUD`（增加Create，查询Retrieve，更新Update，删除Delete）应用时使用它，而对于那些图形编辑，游戏开发等应用，使用Angular就不如使用其他的JavaScript类库方便，如[jQuery](https://jquery.com/)等。
### Angular应用环境
* 在[AngularJS](https://angularjs.org/)官网网站下载Angular文件库:https://angularjs.org/
* 引用CND中的Angular框架文件

``` html
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.0/angular.min.js"></script>
```
### Angular表达式与JavaScript表达式的区别
* 由于Angular中表达式的值的来源固定，因此，在Angular表达式中，不允许出现各类判断和循环语句。
* Angular中表达式的值可以使用**管道符号**`“|”`进行格式化显示数值(即**过滤器**的使用)。例如，`$scope.expr = 20+1.2|number:0`中的`|number:0`表示小数点后的数值不显示。
* 二者之间可以相互调用。若在Angular中的表达式调用JavaScript代码，需要在控制器中定义一个方法，然后由表达式调用该方法即可；而若在JavaScript中执行Angular表达式，则需要借助`$eval()`方法。

### Angular1.3版本开始不支持全局的Controller
从Angular1.3版本开始，不再支持全局的Controller。因此，在使用controller之前，必须对模块进行定义。具体操作如下所示。

``` javascript
  var myApp=angular.module("MyApp",[]);    // 定义模块
  myApp.controller("ControllerName",function($scope){   //控制器定义
  
  });  
```
对于该部分更详细的介绍，请参考[我的博客](http://liubaoshuai.top/2016/09/15/web-002/)。

### Angular中的模板
#### 构建模板内容

* 直接在页面章添加元素和angular指令，以来控制器中构建的属性和方式绑定模板中的元素内容和事件，实现应用需求。
* 通过`<script type=text/script id="ID"></script>`来构建一个用于绑定数据的模板，在模板内添加数据绑定和元素的事件。
* 通过添加元素的`src`属性，导入一个外部文件作为绑定数据的模板。`src`属性设置为模板的文件名称或其ID号(不可以是`class`属性)。需要注意的是，`src`属性值是一个表达式，它可以是`$scope`中的属性名，也可以是普通字符串。如果是后者必须添加引号，例如，**`src="'ID'"`**。在导入数据时，除添加`src`属性外，还需要使用`ng-include`指令。

参考实例如下所示。

```html
<!-- 创建模板 -->
<script type="text/ng-template" class="lbs0912" id="myTemplate">
	Name:{{name}}<br/>
	Email:{{email}}
</script>

<!-- 使用模板 -->
<div ng-include src="'myTemplate'"  ng-controller="c2_4"></div>

<script type="text/javascript">
	//数据操作
	var myApp = angular.module("MyApp",[ ]);
	myApp.controller("c2_4",function($scope){
		$scope.name = "Liu Baoshuai";
		$scope.email = "lbs0912@sjtu.edu.cn";
	});
</script>

```

## Angularde的依赖注入
### 依赖注入的原理
```javascript
//代码段A
var myApp = angular.module("MyApp", []);
myApp.controller("controllerName", function($scope) {
	//控制器代码
});
```
上述代码是Angular中比较常见的一段代码（代码段A）。在本质上，上述代码段A在Angular中执行时是以下述代码（代码段B）的形式执行的。**代码段A和代码段B等价，作用效果相同。**
>在Angular中，可以通过模块中的`config`函数来声明需要注入的对象，而声明的方式是通过调用`provider`服务。但是在Angualr内部，`controller`控制器并不是由`provider`服务来创建的，而是由`controllerProvider`服务来创建的。因此，**当用户创建一个控制器时，实际上是在`config`函数中调用`controllerProvider`服务的`register`方法，来完成一个控制的创建。当控制器创建完成后，再调用`injector`注入器完成各个依赖对象的注入，这就是一个简单控制器实现以来依赖注入的工作原理。**——[《AngualarJS 实战》](https://book.douban.com/subject/26642222/)

```javascript
//代码段B
var myApp = angular.module("MyApp", []);
myApp.config(function($controllerProvider){
	$controllerProvider.register("controllerName",function($scope){
		//控制器代码
	});
});
```
### 依赖注入应用的场景
依赖注入经常应用到以下两个场景。
**(1) 构建控制器**
控制器是一个应用的行为集合，主要负责应用的各项动作和逻辑处理。因此，它常常需要注入各类服务，用于各类逻辑的调用，常用的控制代码如下所示。

```javascript
//代码段A

var ContrFun = function($scope,dep1,dep2,...){
	//控制器中的处理代码
};
ContrFun.$inject = ["$scope","dep1","dep2",...];
var myApp = angular.module("MyApp", []);
myApp.controller("controllerName", ContrFun);
```
上述代码段A和下面的代码段B是等价的。但是，在执行效率上，后者更好，因为后者代码更加简洁，执行效率较高。

```javascript
//代码段B
var myApp = angular.module("MyApp", []);
myApp.controller("controllerName", ["$scope","dep1","dep2",..., function($scope,dep1,dep2,...){
	//控制器中的处理代码
}]);
```
**(2) 调用工厂方法构建服务**
所谓的工厂方法，指的是类似于`config`，`factory`，`directive`和`filter`等构造性质的方法，她们在调用时，常用的代码如下所示。

```javascript
//代码段B
angular.module("MyApp", []);
	.config(["dep1","dep2",...,function(dep1,dep2,...){
		//函数体
	}])
	.factory("servicename",["dep1","dep2",...,function(dep1,dep2,...){
		//函数体
	}])
	.directive("directive",["dep1","dep2",...,function(dep1,dep2,...){
		//函数体
	}])
	.filter("filter",["dep1","dep2",...,function(dep1,dep2,...){
		//函数体
```


### $injector常用API
`$injector`的`has`方法的功能是根据传入的名称，从注册的列表中查找对应的服务。若找到，返回true；反之，返回false。

``` javascript
	injector.has(name)
```
需要注意的是，`has()`方法是在Angular v1.1.5中引入的。因此，在使用时，需要保证Angular的版本号不小于1.1.5。

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 