---
title: 网易2017互联网前端实习生春招在线笔试
date: 2017-03-25 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,网易笔试]
keywords: 校招笔试编程题,网易笔试
---



* 本文主要对网易2017互联网前端实习生春招在线笔试题进行记录。
* 本文中所列举的试题是2017年3月25日的在线笔试题。考试时长2H，共有20道选择题，3道编程题和1道问答题。

<!--more-->


## 编程题1 - 集合
### 题目
集合具有3个特性：确定性，互异性和无序性。给定集合`S = {p/q | w <= p <= x, y <= q <= z}`，其中`1<=w<=x`,`1<=x<=100`, `1<=y<=z`,`1<=z<=100`。

* 输入描述
4个数字，分别为w, x, y, z
* 输出描述 
集合中元素的个数
* 测试样例

```
输入： 1 10 1 1 
输出： 10
```

### 分析
* **注意使用double作为存储的数据。如果使用int型，除法会默认取整。**
* 注意集合的互异性。

### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;


int calPoints(double w, double x, double y, double z) {
	vector<double> p;
	vector<double> q;
	vector<double> res;
	int count = 0;
	for (double i = w; i <= x; i++) {
		p.push_back(i);
	}
	for (double j = y; j <= z; j++) {
		q.push_back(j);
	}
	double temp;
	for (int m = 0; m < p.size(); m++) {
		for (int n = 0; n < q.size(); n++) {
			temp = p[m] / q[n];
			if (find(res.begin(), res.end(), temp) == res.end()) {
				count++;
				res.push_back(temp);
			}
			temp = 0;
		}
	}
	return count;

}



int main() {
	double w, x, y, z;
	while (cin >> w >> x >> y >> z) {
		cout << calPoints(w, x, y, z) << endl;
	}
	return 0;
}


```


## 编程题2 - 无优先级运算
### 题目
对于加，减和乘这3种运算，考虑进行无优先级的运算。即从左向右进行计算。例如，`3+5*7=8*7=56`。

* 输入描述
输入一个字符串，表示需要计算的表达式。

* 输出描述
计算结果

* 测试样例

```
输入
3+5*7

输出
56
```

### 分析
* 题目比较容易，直接求解即可。
* `INT_MAX`需要包含`<climits>`头文件。


### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <cctype>
using namespace std;

#define M_MAX 100000000

int getIndex(string str) {
	int index = M_MAX;
	int add = (str.find("+") == string::npos) ? M_MAX : str.find("+");
	int sub = (str.find("-") == string::npos) ? M_MAX : str.find("-");
	int mul = (str.find("*") == string::npos) ? M_MAX : str.find("*");
	index = min(add, sub);
	index = min(index, mul);
	if (index == M_MAX) {
		return -1;
	}
	return index;
}

int toValue(string str) {
	int num = 0;
	for (int i = 0; i < str.size(); i++) {
		num = num * 10 + (str[i] - '0');
	}
	return num;
}

int calNumber(string str) {
	int res=0;
	int index1,index2;
	int left, right;
	char m_opera;
	//先获取第1个数字
	index1 = getIndex(str);
	res = toValue(str.substr(0, index1));
	str = str.substr(index1);

	while (str.size()) {
		index1 = getIndex(str);
		if (index1 == -1) {
			break;
		}
		m_opera = str[index1];
		str = str.substr(index1+1);
		index2 = getIndex(str);
		if (index2 == -1) {
			right = toValue(str);
		}
		else {
			right = toValue(str.substr(0, index2));
		}

		if (m_opera == '+') {
			res = res + right;
		}
		else if (m_opera == '-') {
			res = res - right;
		}
		else if (m_opera == '*') {
			res = res * right;
		}
		if (index2 == -1) {
			break;
		}
		else {
			str = str.substr(index2);
		}
		

	}
	return res;
}

int main() {
	string str;
	while (cin >> str) {
		cout << calNumber(str) << endl;
	}

	return 0;
}
```

## 编程题3 - 数组去重复
### 题目
给定一个数组，去除重复的数字，最后数字。要求在去重过程中，若数字重复出现，保留最后出现的数字。
* 输入描述
第1行输入数组长度（小于10000）
第2行输入数组的每个元素，空格隔开

* 输出描述
去重后的数组。

* 测试样例

```
输入
9 
100 100 100 99 99 100 100 100

输出
99 100 
```

### 分析
题目比较简单，直接处理即可。


### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;


void changeArr(vector<int> arr) {
	vector<int> res;
	int size = arr.size();
	for (int i = size - 1; i >= 0; i--) {
		if (find(res.begin(), res.end(), arr[i]) == res.end()) {
			res.push_back(arr[i]);
		}
	}
	size = res.size();
	for (int j = size-1; j > 0; j--) {
		cout << res[j] << " ";
	}
    
	cout <<res[0]<< endl;
}

int main() {
	int length;
	while (cin >> length) {
		vector<int> arr(length);
		for (int i = 0; i < length; i++) {
			cin >> arr[i];
		}
		changeArr(arr);
	}

	return 0;
}
```



## 问答题 - 表格点击排序
### 题目
![js-table-1.PNG](http://ol3kbaay9.bkt.clouddn.com/js-table-1.PNG)
对于上述表格，实现点击成绩单元格，列表的数据按照成绩升序排列，再次点击成绩单元格，列表的数据按照成绩降序排列。可以使用原生JS或者jQuery实现。

### 分析
* 参考资料：[javascript实现对表格元素进行排序操作](http://m.jb51.net/article/75066.htm)

### 求解

测试文件中HTML和CSS部分代码如下所示。

```vbscript-html
<!DOCTYPE html>
<html>
<head>
	<title>Test</title>
	<meta charset="utf-8">
	<style type="text/css">
		table{
			width: 280px;
			height: 200px;
			border: solid 2px;
			border-collapse:collapse; /*边界合并*/
		}
		table th,td{
			border: solid 1px;
		}
		table th{
			background-color: #ccc;
		}		
		
		table tr:nth-child(odd){
			/*奇数行*/
			background-color: #80ff00; 
		}
		table tr:nth-child(even){
			/*偶数行*/
			background-color: #ff8040; 
		}
		table tr:hover{
			background-color: #0080c0;
		}
	</style>
</head>
<body>
<table>
	<tr>
		<th>学号</th>
		<th>姓名</th>
		<th>成绩</th>
	</tr>
	<tr>
		<td>01</td>
		<td>张三</td>
		<td>73</td>
	</tr>
	<tr>
		<td>02</td>
		<td>李四</td>
		<td>100</td>
	</tr>
	<tr>
		<td>03</td>
		<td>王五</td>
		<td>91</td>
	</tr>
</table>
</body>
</html>
```
 
 
* 方法1：使用原生JS

```javascript
window.onload=function(){
	var score = document.getElementsByTagName("th")[2];
	var datas = document.getElementsByTagName("td");
	/*获取每行元素的值，注意深复制*/
	var row1 = {
		id:datas[0].innerHTML,
		name:datas[1].innerHTML,
		score:parseInt(datas[2].innerHTML)
	};
	var row2 = {
		id:datas[3].innerHTML,
		name:datas[4].innerHTML,
		score:parseInt(datas[5].innerHTML)
	};
	var row3 = {
		id:datas[6].innerHTML,
		name:datas[7].innerHTML,
		score:parseInt(datas[8].innerHTML)
	};
	var rows_arr = [row1,row2,row3];
	rows_arr.sort(function(a,b){
		return a.score - b.score;
	}); //升序排列

	var countNum = 0;

	score.onclick = function(){
		//初次点击或者偶数次点击，升序排列
		if(countNum%2 == 0){
			for(var i=0,j=0;i<9;i=i+3,j++){
				datas[i].innerHTML = rows_arr[j].id;
				datas[i+1].innerHTML = rows_arr[j].name;
				datas[i+2].innerHTML = rows_arr[j].score;
			}
			countNum++;
			if(countNum == 10){
				countNum = 0;  //将countNum限制在10以内，防止溢出
			}
		}
		//奇数次点击，降序排列
		else if(countNum%2 != 0){
			for(var i=0,j=2;i<9;i=i+3,j--){
				datas[i].innerHTML = rows_arr[j].id;
				datas[i+1].innerHTML = rows_arr[j].name;
				datas[i+2].innerHTML = rows_arr[j].score;
			}
			countNum++;
		}
	};
};
```
注意，JS中存在深复制和浅复制。复制一个对象Object时候，如果只采用等号赋值，是进行的浅复制。改变其中的一个变量，另一个变量也会改变。

因此，上述程序中进行了深复制。将列表中的每个值以对象属性的形式进行拷贝，存放到row1，row2和row3中。

**如果像下述程序那样，只进行浅复制，则`row1.innerHTML = arr[0].innerHTML;`语句之后，也会改变数组arr的内容，造成程序错误。**
 

```javascript
window.onload=function(){
	var score = document.getElementsByTagName("th")[2];
	var datas = document.getElementsByTagName("td");
	var rows  = document.getElementsByTagName("tr");

	var row1  = rows[1];
	var row2  = rows[2];
	var row3  = rows[3];
	row1.score = parseInt(datas[2].innerHTML);
	row2.score = parseInt(datas[5].innerHTML);
	row3.score = parseInt(datas[8].innerHTML);
	var arr = [row1,row2,row3];
	arr.sort(function(a,b){
		return a.score - b.score;
	});
	var countNum = 0;
	score.onclick = function(){
	//初次点击或者偶数次点击，升序排列
		if(countNum%2 == 0){
			row1.innerHTML = arr[0].innerHTML;
			row2.innerHTML = arr[1].innerHTML;
			row3.innerHTML = arr[2].innerHTML;
			countNum++;
			if(countNum == 10){
				countNum = 0;  //将countNum限制在10以内，防止溢出
			}
		}
		//奇数次点击，降序排列
		else if(countNum%2 != 0){
			row1.innerHTML = arr[2].innerHTML;
			row2.innerHTML = arr[1].innerHTML;
			row3.innerHTML = arr[0].innerHTML;
			countNum++;
		}
	};
};
```

* 方法2：使用jQuery插件——TableSort

动画表格排序插件TableSort是专门针对表格Table中各列`<td>`以动画效果进行排序，排序形式包括正则匹配，按照字母升降，ASCII或数值。无论选择哪种排序形式，在表格中，都以动画的形式展示排序过程，效果非常好。

参考[jQuery-tableSort](http://plugins.jquery.com/tablesorter/2.17.7/)了解更多。

```vbscript-html
<!DOCTYPE html>
<html>
<head>
    <title>动画表格排序插件tableSort</title>
    <meta charset="utf-8">
    <style type="text/css"> 
        table{
            width: 280px;
            height: 200px;
            border: solid 2px;
            border-collapse:collapse; /*边界合并*/
        }
        table th,td{
            border: solid 1px;
        }
        table th{
            background-color: #ccc;
        }   
        
        table tr:nth-child(odd){
            /*奇数行*/
            background-color: #80ff00; 
        }
        table tr:nth-child(even){
            /*偶数行*/
            background-color: #ff8040; 
        }
        table tr:hover{
            background-color: #0080c0;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script src="./jquery.tableSort.js"></script>
    
</head>
<body>
<div id="tip"></div>
<table id="tbStudent" class="table">
    <tr>
       <th><a href="javascript:">编号</a></th>
       <th><a href="javascript:">姓名</a></th>
       <th><a href="javascript:">性别</a></th>
       <th><a href="javascript:">总分</a></th>
    </tr>
    <tr>
       <td>1031</td>
       <td>李渊</td>
       <td>男</td>
       <td>654</td>
    </tr>
    <tr>
       <td>1021</td>
       <td>张扬</td>
       <td>男</td>
       <td>624</td>
    </tr>
    <tr>
       <td>1011</td>
       <td>吴敏</td>
       <td>女</td>
       <td>632</td>
    </tr>
    <tr>
       <td>1026</td>
       <td>李小明</td>
       <td>男</td>
       <td>610</td>
    </tr>
    <tr>
       <td>1016</td>
       <td>周谨</td>
       <td>女</td>
       <td>690</td>
    </tr>
    <tr>
       <td>1041</td>
       <td>谢小敏</td>
       <td>女</td>
       <td>632</td>
    </tr>
    <tr>
       <td>1072</td>
       <td>刘明明</td>
       <td>男</td>
       <td>633</td>
    </tr>
    <tr>
       <td>1063</td>
       <td>蒋忠公</td>
       <td>男</td>
       <td>675</td>
    </tr>
</table>


<script type="text/javascript">
    $(function() {
        var $tb = $("#tbStudent");
        var $tip = $("#tip");
        var $bln = true;
        var $str = "降";
        //遍历table标题中的a元素
        $(".table tr th a").each(function(i) {
            $(this).bind("click", function() {
                $bln = $bln ? false : true;
                $str = $bln ? "降" : "升";
                $tip.show().html("当前按 <b>" +
                $(this).html() + $str + "序</b> 排列");
                $tb.sortTable({
                    onCol: i + 1,
                    keepRelationships: true,
                    sortDesc: $bln
                });
            });
        });
    });
</script>
</body>
</html>
```
 

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 