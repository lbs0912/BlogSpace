---
title: 从Angular1.3版本开始不支持全局的Controller
date: 2016-09-15 21:43:11
tags: [Angular,Front-end]
categories: Front-end
---
从Angular1.3版本开始不再支持全局的Controller。
<!-- more -->

## 问题描述
首先，运行如下所示的代码。

``` html
<!DOCTYPE html>
<html ng-app="">
    <head>
        <meta charset="utf-8">
        <title>Test Demo Angular</title>    
        <script  src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.0.8/angular.min.js"
        </script>     
        <!--当引入的AngularJS版本高于1.3之后，网页控制台会报错，界面元素不能正常显示-->
        <!-- https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.4/angular.min.js -->
    </head>
    <body>
        <div>
          <p>Name : <input type="text" ng-model="name"></p>
          <h1>Hello {{name}}</h1>
        </div>   

        <div ng-controller="HelloController">
            <p>{{greeting.text}}, World</p>
        </div>
      
        <script type="text/javascript">
            function HelloController($scope) {
                 $scope.greeting = { text:"Hello"};
            }  
        </script>
         
    </body>
</html>
```
若引入的AngularJS版本在1.3之前，则网页元素能够正常显示。

**图1 界面元素正常显示**
![AngularJS v1.0 HTML](http://oda53d5ps.bkt.clouddn.com/web002-01.PNG)
但是，当引入的AngularJS版本高于1.3（含1.3）之后，网页控制台会报错:``Angularjs Uncaught Error: [$injector:modulerr]``，界面元素不能正常显示。

**图2 当AngularJS版本高于1.3之后，界面元素正常显示**
![AngularJS v1.5 HTML](http://oda53d5ps.bkt.clouddn.com/web002-03.PNG)

**图3 网页控制台的报错信息**
![AngularJS Control Console Message](http://oda53d5ps.bkt.clouddn.com/web002-02.PNG)

## 问题分析
查阅资料可以发现，从AngulaJS 1.3版本开始，不再支持全局的Controller。因此，在使用controller之前，必须对模块进行定义。
将上述代码的JavaScript部分修改成下述代码，即可解决该错误。

``` html
<!DOCTYPE html>
<html ng-app="MyApp">
    <head>
        <meta charset="utf-8">
        <title>Test Demo Angular</title>    
        <script  src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.4/angular.min.js"
        </script>     
        <!--当引入的AngularJS版本高于1.3之后，网页控制台会报错，界面元素不能正常显示-->
        <!-- https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.4/angular.min.js -->
    </head>
    <body>
        <div>
          <p>Name : <input type="text" ng-model="name"></p>
          <h1>Hello {{name}}</h1>
        </div>   

        <div ng-controller="HelloController">
            <p>{{greeting.text}}, World</p>
        </div>
      
        <script type="text/javascript">
            var myApp=angular.module("MyApp",[]);
  　　　　  myApp.controller("HelloController",function($scope){
                $scope.greeting = { text: "Hello"};
            });  
        </script>
         
    </body>
</html>
```

## 参考文档
1. [AngularJS](https://angularjs.org/)
2. [The AngularJS Question on StackOverflow](http://stackoverflow.com/questions/28727919/angularjs-uncaught-error-injectormodulerr)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
