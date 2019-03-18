---
title: Table点击排序
date: 2017-08-22 15:35:26
categories: Front-end
tags: [Front-end,tableSort]
keywords: tableSort
toc: true
---







* [Demo源代码 | GitHub](https://github.com/lbs0912/TableSort-Demo)
* 效果展示：[TableSort Demo](http://liubaoshuai.com/projects/table-sort.html)
*  本文主要记录Table相应的功能扩展，包括鼠标悬浮背景色实现，奇偶行背景色实现，点击表头进行排序实现。


<!--more-->


## DOM结构

```vbscript-html
<style type="text/css">
table, th, td {
	border: 1px solid #ccc;
	border-collapse: collapse;
	text-align: center;
}
th, td {
	width: 120px;
}
.hover {
	background-color: #005eae;
}
</style>


<table>
	<tr>
		<th class="id">ID</th>
		<th class="name">Name</th>
		<th class="sex">Sex</th>
		<th class="age">Age</th>
		<th class="score">Score</th>
	</tr>
	<tr>
		<td>003</td>
		<td>David</td>
		<td>M</td>
		<td>20</td>
		<td>92.34</td>
	</tr>
	<tr>
		<td>001</td>
		<td>Tom</td>
		<td>M</td>
		<td>23</td>
		<td>98.67</td>
	</tr>
	<tr>
		<td>002</td>
		<td>Jilly</td>
		<td>F</td>
		<td>22</td>
		<td>65.39</td>
	</tr>
	<tr>
		<td>005</td>
		<td>Peter</td>
		<td>M</td>
		<td>30</td>
		<td>84.78</td>
	</tr>
	<tr>
		<td>004</td>
		<td>Alice</td>
		<td>F</td>
		<td>25</td>
		<td>82.84</td>
	</tr>
</table>
```

## 鼠标悬浮背景色


注意在for循环中，使用let声明变量，而不要使用var，以此来避免闭包。

```javascript
//功能1： 鼠标悬停背景色添加
var $th = $("th");
var $td = $("td");
for (let i = 0; i < $th.length; i++) {
	//使用let 避免闭包
	$th.eq(i).hover(function() {
		$th.eq(i).addClass("hover");
	}, function() {
		$th.eq(i).removeClass("hover");
	});
}
for (let i = 0; i < $td.length; i++) {
	//使用let 避免闭包
	$td.eq(i).hover(function() {
		$td.eq(i).addClass("hover");
	}, function() {
		$td.eq(i).removeClass("hover");
	});
}
```

## 奇偶行背景色

```javascript
//功能2： 表格隔行变色
$("tr:nth-child(even)").css("background-color","#00FFFF");

$("tr:nth-child(odd)").css("background-color","#FF6A00");
```

## 点击表头进行排序
### 实现思路
1. 获取td内容，将其存放到一个二维数组中。
2. 为表头添加点击排序事件。
3. 对二维数组进行冒泡排序。
4. 清空表格的内容，将排序好的`tr`逐个插入`table`中。


### 注意事项
* jQuery中`eq`使用

```javascript
$("tr:eq(index)").addClass("test");  //如果在选择器中，需要使用冒号：
$("tr").eq(index).addClass("test"); //如果不在选择器中，需要使用点号.
```

* `remove()` | `empty()`删除元素


`remove()`会彻底删除DOM元素。`empty()`只会清空所选择元素的内容和其子元素的所有内容。注意，此时，**子元素不存在DOM树中，但是所选元素依旧存在DOM树中。**

```

<div>0<p>123</p>
<div>1<p>123</p>
<div>2<p>123</p>
<div>3<p>123</p>
<div>4<p>123</p>
<button>Empty</button>
<script type="text/javascript">
	$("button").click(function(event) {
		$("div").empty();
	});
</script>
```

点击按钮之后，DOM结构为
```
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<button>Empty</button>
```

本实例中，注意如下区别。否则，在后续重新更新奇偶行背景色时候会出现错误。

```javascript
$("tr:not(:first-child)").remove();  //$("tr").length是0  彻底删除
$("tr:not(:first-child)").remove();  //$("tr").length长度还是6
```

* jQuery的`:not()`选择器

`:not()`用于取反。

```javascript
$("tr:first-child");   //第1个tr元素
$("tr:not(:first-child)") //非第1个tr元素
```
 
* JS声明二维数组

```
var arr = new Array();  //或者  var arr = [];
for(let i=0;i<5;i++){
	//下面的代码不可少，要声明其中的元素也是一个数组。
	//只有这样，后续才能调用数组的push方法
	arr[i] = new Array();  //或者  arr[i] = [];
	arr[i].push(123);
}
```


### 步骤1 获取td内容至二维数组

```javascript
var tdValueArr = new Array();  //两维数组
var rowLength = $th.length;  //一行的长度，即每行的td数目
var colLength = $("tr").length-1;  //不包括表头的行数
for(let i=0;i<colLength;i++){
	//遍历每一行的td内容
	tdValueArr[i] = new Array();  //不可少  声明是二维数组
	for(let j=0;j<rowLength;j++){
		tdValueArr[i].push($td.eq(j+i*rowLength).text());
	}
}
```

### 步骤2 绑定点击事件

```javascript
for(let i=0;i<rowLength;i++){
	$th.eq(i).click(function(event) {
		var index = i;  //点击的第几列
		//冒泡排序
		//..... step 3
	
		//清空表格内容 并插入新的内容
		//.... step 4
}
```

### 步骤3 td内容冒泡排序

```javascript
var index = i;  //点击的第几列
//冒泡排序
for(let i=0;i<colLength;i++){
	for(let j=1;j<colLength;j++){
		if(convertType(index,tdValueArr[j][index])<convertType(index,tdValueArr[j-1][index])){
			//升序排序
			let temp;
			for(let k=0;k<rowLength;k++){
				//交换两行内容
				temp = tdValueArr[j][k];
				tdValueArr[j][k] = tdValueArr[j-1][k];
				tdValueArr[j-1][k] = temp;
			}
		}
	}
}
```

排序过程中需要对比较值内容进行类型转换。因为第1步骤中获取的内容都是string类型，如果不进行类型转换，那么在对数字内容进行排序时候，会按照字典排序方式进行排序。

```javascript
// 类型转换
function convertType(index,str){
	if(index === 0 || index === 3){
		// id  age
		return parseInt(str);
	}
	else if(index === 4){
		//score
		return parseFloat(str);
	}
	else{
		// string类型
		return (typeof str === "string")?  str:str.toString();
	}
}
```

### 步骤4 清空表格原有内容并插入新内容

```javascript
$("tr:not(:first-child)").remove();  //清空列表内容  不清空表头
if(ascend){
	//升序排序
	for(let i=0;i<colLength;i++){
		var str= "<tr>";
		for(let j=0;j<rowLength;j++){
			str += "<td>";
			str += tdValueArr[i][j];
			str += "</td>";
		} 
		str += "</tr>";
		$("table").append(str);
	}
}
else{
	//降序排序
	for(let i=colLength-1;i>=0;i--){
		var str= "<tr>";
		for(let j=0;j<rowLength;j++){
			str += "<td>";
			str += tdValueArr[i][j];
			str += "</td>";
		} 
		str += "</tr>";
		$("table").append(str);
	}
}
ascend = !ascend;  //排序设置取反
//更新奇偶行颜色
$("tr:nth-child(even)").css("background-color","#00FFFF");

$("tr:nth-child(odd)").css("background-color","#FF6A00");
```


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
