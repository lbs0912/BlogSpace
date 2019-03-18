---
title: 华为2017软件实习生在线笔试题
date: 2017-03-24 22:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,在线笔试]
keywords: 校招笔试编程题
---




* 本文主要对华为2017暑期实习生在线笔试题进行记录。
* 本文中所列举的试题是2017年3月24日的在线笔试题。考试时长2H，共有3道编程题。



<!--more-->

## 数字翻转求和
### 题目
* 给定两个数字a和b，将其翻转，然后输出求和结果。
* 例如，`a=123,b=456`，反转之后变成`321`和`654`，输出求和结果为`975`。
* 需要注意的是，数字的后导0，在反转之后需要去掉。例如，`a=100,b=200`，反转之后变成`1`和`2`，输出求和结果为`3`。
* 样例

```
输入： 123,456
输出： 975

输入： 300,400
输出： 7
```

### 分析
题目本身并不难，需要注意的是输入格式为`a,b`，中间有逗号，是一个字符串，而不是两个数字。**这是一个很容易忽略的地方。**（考试时候因为这个细节问题，耽误了五六分钟时间）

### 求解

```cpp
#include <iostream>
#include <string>
using namespace std;


int reverseAdd(int a, int b) {
	int res;
	if ((a < 1) || (a > 70000) || (b < 1) || (b > 70000)) {
		return -1;
	}
	bool flagA = false;
	bool flagB = false;
	int tempA = 0;
	int tempB = 0;
	while (a>0) {
		if ((a % 10) == 0 && flagA == false) {
			a = a / 10;
		}
		else {
			flagA = true;
			tempA = tempA * 10 + (a % 10);
			a = a / 10;
		}
	}
	while (b>0) {
		if ((b % 10) == 0 && flagB == false) {
			b = b / 10;
		}
		else {
			flagB = true;
			tempB = tempB * 10 + (b % 10);
			b = b / 10;
		}
	}
	res = tempA + tempB;
	return res;
}

int main() {
	int a = 0;
	int b = 0;
	string str;
	while (cin >>str) {
		int index = str.find(",");
		for (int i = 0; i < index; i++) {
			a = a * 10 + str[i]-'0';
		}
		for (int j = index+1; j < str.size(); j++) {
			b = b * 10 + str[j]-'0';
		}

		cout << reverseAdd(a, b);
	}

	return 0;
}
```


## 魔方翻转
### 题目
一个魔方，6个面分别标有1~6共6个数字。左面数字为1，右面数字为2，前面数字为3，后面数字为4，上面数字为5，下面数字为6。记初试状态为1,2,3,4,5,6。

下面对魔方进行操作，共6种操作。
* 向左翻转，记作L
* 向右翻转，记作R
* 向前翻转，记作F
* 向后翻转，记作B
* 顺时针旋转90度，记作C（从上向下看）
* 逆时针旋转90度，记作A（从上向下看）

* 输入描述
输入一个字符串，表示一系列操作，如LRCAFFFFB。字符串长度小于等于50。
* 输出描述
魔方最终的状态。

* 测试样例

```
输入
RA

输出
563421
```

### 分析
暴力求解，处理6种操作即可。（暂未想到更优化的算法）


### 求解

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> arr = { 1,2,3,4,5,6 };
//左
void Lchange(){
	vector<int> res = arr;
	arr[0] = res[4];
	arr[1] = res[5];
	arr[4] = res[1];
	arr[5] = res[0];
}
//右
void Rchange() {
	vector<int> res = arr;
	arr[0] = res[5];
	arr[1] = res[4];
	arr[4] = res[0];
	arr[5] = res[1];
}

//F
void Fchange() {
	vector<int> res = arr;
	arr[2] = res[4];
	arr[3] = res[5];
	arr[4] = res[3];
	arr[5] = res[2];
}
//B
void Bchange() {
	vector<int> res = arr;
	arr[2] = res[5];
	arr[3] = res[4];
	arr[4] = res[2];
	arr[5] = res[3];
}

void Achange() {
	vector<int> res = arr;
	arr[0] = res[3];
	arr[1] = res[2];
	arr[2] = res[0];
	arr[3] = res[1];
}
void Cchange() {
	vector<int> res = arr;
	arr[0] = res[2];
	arr[1] = res[3];
	arr[2] = res[1];
	arr[3] = res[0];
}


void calState(string str) {
	int size = str.size();
	int t1,t2,t3,t4;
	for (int i = 0; i < size; i++) {
		if (str[i] == 'L') {
			Lchange();
		}
		else if (str[i] == 'R') {
			Rchange();
		}
		else if (str[i] == 'F') {
			Fchange();
		}
		else if (str[i] == 'B') {
			Bchange();
		}
		else if (str[i] == 'A') {
			Achange();
		}
		else if (str[i] == 'C') {
			Cchange();
		}
	}

	for (int i = 0; i < arr.size(); i++) {
		cout << arr[i];
	}
	cout << endl;
}


int main(){
	string str;
	while (cin >> str) {
		calState(str);
	}
	return 0;
}
```

## 最短路径
### 题目
共有6个城市，编号为1~6。小K是一个公司经理，居住在5号城市中，由于公司业务，经常需要去1,2,3,4,6号城市出差。城市之间路途耗时如下数组所示。`map[i][j]`表示从`i+1`号城市去`j+1`号城市的耗时，M表示不可到达，计算时候用1000进行计算。例如，从1号城市到3号城市需要`map[0][2]=10`个小时，从3号城市无法到达1号城市，计算时候用1000小时进行计算。即从A到B城市和从B到A城市的耗时是不一样的。


```
#define M 1000
int map[6][6] = {
	{ 0,2,10,5,3,M },
	{ M,2,12,M,M,10 },
	{ M,M,0,M,7,M },
	{ 2,M,M,0,2,M },
	{ 4,M,M,1,0,M },
	{ 3,M,1,M,2,0 }
};
```

有时候出差若遇到城市有雾霾，则无法从该城市出发，也无法从其他城市到达该城市。

* 输入描述
第1行输入小K要到达的城市。
第2行输入发生雾霾的城市。若输入的是0，则表示没有城市发生雾霾。

* 输出描述
第1行输出最短路径。（从5号城市到指定的城市） 
第2行输出最短路径的路线。

* 测试样例

```
输入 
2 
4

输出
6 
[5, 1, 2]
```

### 分析
* 该问题属于最短路径问题。可以使用Floyd算法求解。
* 注意，输出的第2行中数组元素之间有空格。
* C++中两个输出的连接用`<<`，而Java和JavaScript中用`+`连接。不要搞混了。（考场上因为这个错误，耽误了快10分钟了，欲哭无泪）

```
//输出最短路径的路线
cout << "[";
for(int xx=0;xx<res.size()-1;xx++){
	cout << res[xx]<< ",";   //OK
	cout << res[xx]+ ",";   //Error
}
cout << res[res.size() - 1] << "]" << endl;
```


### 求解

```cpp

#include <iostream>
#include <vector>
using namespace std;

#define M 1000
int map[6][6] = {
	{ 0,2,10,5,3,M },
	{ M,2,12,M,M,10 },
	{ M,M,0,M,7,M },
	{ 2,M,M,0,2,M },
	{ 4,M,M,1,0,M },
	{ 3,M,1,M,2,0 }
};

int main() {
	int end;
	int foggy;
	int distance;
	int i, j, k;

	while (cin >> end) {
		cin >> foggy;
		if (foggy != 0) {
			for(int temp = 0;temp<6;temp++){
				map[foggy - 1][temp] = M;
				map[temp][foggy - 1] = M;
			}
		}
		vector<int> res;
		res.push_back(5);
		//Floyd算法  求最短路径
		for (k = 0; k < 6; k++) {
			for (i = 0; i < 6; i++) {
				for (j = 0; j < 6; j++) {
					if (map[i][j] > map[i][k] + map[k][j]) {
						map[i][j] = map[i][k] + map[k][j];
						//记录最短路径
						if (i == 4 && j == end - 1) {
							res.push_back(k+1);
						}
					}
				}
			}
		}
		res.push_back(end);
		//输出最短路径
		cout << map[4][end - 1] << endl;
	    //输出最短路径的路线
		cout << "[";
		for(int xx=0;xx<res.size()-1;xx++){
			cout << res[xx]<< ",";
		}
		cout << res[res.size() - 1] << "]" << endl;

	}
	return 0;
}
```


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 