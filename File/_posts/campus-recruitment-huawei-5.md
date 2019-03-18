---
title: 华为2017软件实习生在线笔试题-2
date: 2017-04-21 22:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,笔试]
keywords: 校招笔试编程题,动态规划
---



* 本文主要对华为2017暑期实习生在线笔试题进行记录。
* 本文中所列举的试题是2017年4月21日的在线笔试题。考试时长2H，共有3道编程题。

<!--more-->


## 日期的天数序号
### 题目
[编程|100分] 
时间限制：3秒
空间限制：32768K

* 题目描述

输入某年某月某日，判断这一天是这一年的第几天？ 
年份限制在<10000以内；

1.程序分析：以3月5日为例，应该先把前两个月的加起来，然后再加上5天即本年的第几天，特殊情况，闰年且输入月份大于3时需考虑多加一天。

说明： 闰年规则（<10000）
1、普通年能被4整除且不能被100整除的为闰年.（如2004年就是闰年,1901年不是闰年）
2、世纪年能被400整除的是闰年.(如2000年是闰年,1900年不是闰年)

* 输入描述

输入年月日，年份<10000


* 输出描述

该日期在本年度的序号,参考输出样例。如果日期非法,输出invalid input

* 输入例子

2000-2-28

* 输出例子

2000-2-28 is the No.59 day of 2000.

### 分析
注意对不合理情况的判断。
* 非闰年出现2月29日
* 年份大于1000
* 月大于12
* 天数大于当月最大的天（勿忘记！！）


将string转换为int的方法

```
#include <string>

string str = "10";
int number = atoi(str.c_str());
```

### 求解

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool isRunYear(int year) {
	bool res = false;
	if (year % 400 == 0) {
		res == true;
	}
	else if(year%4 == 0 && year%100 != 0) {
		res == true;
	}
	return res;
}

void getDate(string str, int& year, int& month, int& day,bool& inputError) {
	int index = str.find("-");
	if (index < str.size()) {
		year = atoi(str.substr(0, index).c_str());
	}
	else {
		cout << "invalid input" << endl;
		inputError = false;
		return;
	}

	str = str.substr(index+1);
	index = str.find("-");
	if (index < str.size()) {
		month = atoi(str.substr(0, index).c_str());
	}
	else {
		cout << "invalid input" << endl;
		inputError = false;
		return;
	}

	str = str.substr(index + 1);
	day = atoi(str.c_str());
}
int main() {
	string str;
	int year, month, day;
	vector<int> days = { 0,31,28,31,30,31,30,31,31,30,31,30,31 };

	int res;
	bool inputError;
	while (cin >> str) {
		inputError = true;
		res = 0;
		getDate(str,year,month,day,inputError);
		if (inputError == false) {
			continue;  //输入格式不合理
		}
		//输入内容不合理
		//内容不合理1 - 非闰年出现2月29日
		if(month == 2 && isRunYear(year) == false && day == 29){
			inputError = false;
		}
		//内容不合理2 - 年份大于1000
		//内容不合理3 - 月大于12
		//内容不合理4 - 天数大于当月最大的天 ！！！
		if(year > 10000 || month > 12 || day > days[month]){
			inputError = false;
		}
		if (inputError == false) {
			cout<< "invalid input" <<endl;
			continue;  //输入格式不合理
		}
		
		for (int i = 1; i < month; i++) {
			res += days[i];
		}
		if (month >= 3 && isRunYear(year) == true) {
			res++;
		}
		res += day;
		cout << str << " is the No." << res << " day of " << year << endl;
	}
	//system("pause");
	return 0;
}
```

## 德州扑克
### 题目
[编程|200分] 
时间限制：3秒
空间限制：32768K

* 题目描述

五张牌，每张牌由牌大小和花色组成，牌大小2~10、J、Q、K、A，牌花色为红桃、黑桃、梅花、方块四种花色之一。 判断牌型:
牌型1，同花顺：同一花色的顺子，如红桃2红桃3红桃4红桃5红桃6。
牌型2，四条：四张相同数字 + 单张，如红桃A黑桃A梅花A方块A + 黑桃K。
牌型3，葫芦：三张相同数字 + 一对，如红桃5黑桃5梅花5 + 方块9梅花9。
牌型4，同花：同一花色，如方块3方块7方块10方块J方块Q。
牌型5，顺子：花色不一样的顺子，如红桃2黑桃3红桃4红桃5方块6。
牌型6，三条：三张相同 + 两张单。
牌型7，其他。
说明：
1）五张牌里不会出现牌大小和花色完全相同的牌。
2）前面的牌型比后面的牌型大，如同花顺比四条大，依次类推。
输入描述:
输入由5行组成
每行为一张牌大小和花色，牌大小为2~10、J、Q、K、A，花色分别用字符H、S、C、D表示红桃、黑桃、梅花、方块。


* 输出描述
输出牌型序号，5张牌符合多种牌型时，取最大的牌型序号输出

* 输入例子
2 H
3 C
6 S
5 S
4 S

* 输出例子
5

### 分析
采用map数据结构求解。

`map<int,string> datas`中插入数据用`insert`方法实现。

```cpp
#include <utility>
typedef pair<int,string> Pair;

datas.insert(Pair(value,style))；
//等价于
datas.insert(pair<int,string>(value,style))；
```

使用`pair`要包括`#include<utility>`头文件。

访问map中的元素，其方法如下
```cpp
//map中会按照value值大小排序
for (auto it = datas.begin(); it != datas.end(); it++, i++) {
	value[i] = (*it).first;
	style[i] = (*it).second;
}
```

**map存储时，会按照index的值自动进行升序排序。**


### 求解

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <utility>
using namespace std;

typedef  pair<int, string> Pair;


int toValue(string value) {
	int number;
	if (value == "J") {
		number = 11;
	}
	else if (value == "A") {
		number = 1;
	}
	else if (value == "Q") {
		number = 12;
	}
	else if (value == "K") {
		number = 13;
	}
	else {
		number = atoi(value.c_str());
	}
	return number;
}
int typeNumber(map<int, string> datas) {
	int res = 7;
	vector<int> value(5);
	vector<string> style(5);
	int i = 0;
	//map中会按照value值大小排序
	for (auto it = datas.begin(); it != datas.end(); it++, i++) {
		value[i] = (*it).first;
		style[i] = (*it).second;
	}

	//同花顺 1 同花顺：同一花色的顺子，如红桃2红桃3红桃4红桃5红桃6。
	if (style[0] == style[1] && style[1] == style[2]
		&& style[2] == style[3] && style[3] == style[4]) {
		bool flag1 = true;
		for (int i = 1; i < 5; i++) {
			if (value[i] != value[i - 1] + 1) {
				flag1 = false;
			}
		}
		if (flag1 == true) {
			res = 1;
			return res;
		}
	}

	// 牌型 2 四条 四张相同数字 + 单张
	if (value[0] == value[1]) {
		if (value[1] == value[2] && value[2] == value[3] && value[3] != value[4]) {
			res = 2;
			return res;
		}
	}
	if (value[0] != value[1]) {
		if (value[1] == value[2] && value[2] == value[3] && value[3] == value[4]) {
			res = 2;
			return res;
		}
	}
	// 牌型 3 葫芦：三张相同数字 + 一对，如红桃5黑桃5梅花5 + 方块9梅花9。
	if (value[0] == value[1] && value[1] == value[2]
		&& value[2] != value[3] && value[3] == value[4]) {
		res = 3;
		return res;
	}
	if (value[0] == value[1] && value[1] != value[2]
		&& value[2] == value[3] && value[3] == value[4]) {
		res = 3;
		return res;
	}

	//牌型4，同花：同一花色，如方块3方块7方块10方块J方块Q。
	if (style[0] == style[1] && style[1] == style[2]
		&& style[2] == style[3] && style[3] == style[4]) {
		res = 4;
		return res;
	}

	//牌型5，顺子：花色不一样的顺子，如红桃2黑桃3红桃4红桃5方块6。
	bool flag5 = true;
	for (int i = 1; i < 5; i++) {
		if (value[i] != value[i - 1] + 1) {
			flag5 = false;
		}
	}
	if (flag5 == true) {
		res = 5;
		return res;
	}

	//牌型6，三条：三张相同 + 两张单。
	if (value[0] == value[1] && value[1] == value[2]
		&& value[2] != value[3] && value[2] != value[4]
		&& value[3] != value[4]) {
		res = 6;
		return 6;
	}
	if (value[4] == value[3] && value[3] == value[2]
		&& value[2] != value[1] && value[2] != value[0]
		&& value[1] != value[0]) {
		res = 6;
		return 6;
	}
	else {
		res = 7;
	}
	return res;
}

int main() {
	string value, style;
	int number;
	
	while (cin >> value >> style) {
		map<int, string> datas;
		
		datas.insert(Pair(number, style));
		//等价于
		//datas.insert(pair<int,string>(number,style));

		for (int i = 1; i < 5; i++) {
			cin >> value >> style;
			if (value == "J") {
				number = 11;
			}
			else if (value == "A") {
				number = 1;
			}
			else if (value == "Q") {
				number = 12;
			}
			else if (value == "K") {
				number = 13;
			}
			else {
				number = atoi(value.c_str());
			}
			datas.insert(Pair(number, style));
		}
		cout << typeNumber(datas) << endl;
	}





	//system("pause");
	return 0;
}
```


## 圣诞的祝福
### 题目

[编程|300分] 圣诞的祝福
时间限制：3秒
空间限制：32768K

* 题目描述
简要描述： 给定一个M行N列的矩阵(M*N个格子)，每个格子中放着一定数量的平安果。
你从左上角的格子开始，只能向下或向右走，目的地是右下角的格子。
每走过一个格子，就把格子上的平安果都收集起来。求你最多能收集到多少平安果。
注意：当经过一个格子时，需要要一次性把格子里的平安果都拿走。
限制条件：1 < N, M <= 50；每个格子里的平安果数量是0到1000(包含0和1000)。
输入描述:
输入包括两行:
第一行为矩阵的行数M和列数N
第二行为一个M*N的矩阵，矩阵的数字代表平安果的数量，例如：
1 2 3 40
6 7 8 90


* 输出描述
输出一个数字,表示能可以收集到的平安果的数量。

* 输入例子
2 4
1 2 3 40
6 7 8 90

* 输出例子
136


### 分析

采用动态规划求解。

* 设原数据数组为`data[i][j]`。动态规划创建的数组为`acc[i][j]`，表示从左上角起点到该点所累计的平安果数量。
* 动态规划中边界条件是acc矩阵的第1行和第1列。对于这2种情况，直接累加求和即可。即`acc[0][i] = data[0][i]; acc[0][i] += acc[0][i-1]`。
* 对于acc矩阵中其他的位置，DP转移方程为`acc[i][j] = data[i][j] + max(acc[i][j-1],acc[i-1][j])`。


### 求解

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;


int main() {
	int M, N;
	while (cin >> M >> N) {
		vector<vector<int>> datas(M, vector<int>(N, 0));
		vector<vector<int>> acc(M, vector<int>(N, 0));
		for (int i = 0; i < M; i++) {
			for (int j = 0; j < N; j++) {
				cin >> datas[i][j];
				
			}
		}
		//DP 动态规划
		//边界条件 - 列
		acc[0][0] = datas[0][0];
		for (int i = 1; i < M; i++) {
			acc[i][0] = datas[i][0];
			acc[i][0] += acc[i - 1][0];
		}
		//边界条件 - 行
		for (int j = 1; j < N; j++) {
			acc[0][j] = datas[0][j];
			acc[0][j] += acc[0][j-1];
		}
		for (int i = 1; i < M; i++) {
			for (int j = 1; j < N; j++) {
				acc[i][j] = datas[i][j];
				acc[i][j] += max(acc[i][j - 1], acc[i - 1][j]);
			}
		}
		cout << acc[M - 1][N - 1] << endl;
	}
	//system("pause");
	return 0;
}
```




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 