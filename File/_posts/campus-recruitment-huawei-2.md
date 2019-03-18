---
title: 华为2016研发工程师上机笔试题
date: 2017-03-24 20:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,华为笔试]
keywords: 校招笔试编程题,华为笔试
---


* 本文主要对华为笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/710802/summary)
* 下载本文中的[华为笔试试题](http://static.nowcoder.com/pdf/paper/%E5%8D%8E%E4%B8%BA2016%E7%A0%94%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%BC%96%E7%A8%8B%E9%A2%98.pdf)

<!--more-->




##  删数
### 题目
* 有一个数组a[N]顺序存放0~N-1，要求每隔两个数删掉一个数，到末尾时循环至开头继续进行，求最后一个被删掉的数的原始下标位置。以8个数(N=7)为例:｛0，1，2，3，4，5，6，7｝，0->1->2(删除)->3->4->5(删除)->6->7->0(删除),如此循环直到最后一个数被删除。
* 输入描述:
每组数据为一行一个整数n(小于等于1000)，为数组成员数,如果大于1000，则对a[999]进行计算。
* 输出描述:
一行输出最后一个被删掉的数的原始下标位置。
* 输入例子:
8
* 输出例子:
6

### 分析
* 思路1：暴力求解。创建一个标志数组，标记每个元素是否被删除。遍历数组元素求解。
* 思路2：使用队列。


### 求解

* 方法1：暴力求解。创建一个标志数组，标记每个元素是否被删除。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int lastDelete(int length) {
	int res = -1;
	if (length > 1000) {
		length = 1000;
	}
	vector<int> nums(length, 0);
	int count = 0;
	int i = (2) % length;
	while (count < length - 1) {
		nums[i] = 1;
		count++;
		int add = 0;
		while (add < 3) {
			i = (i + 1) % length;
			if (nums[i] == 0) {
				add++;
			}
		}
	}
	for (int i = 0; i<length; i++) {
		if (nums[i] == 0) {
			res = i;
		}
	}
	return res;
}

int main() {
	int length;
	while (cin >> length) {
		int res = lastDelete(length);
		cout << res << endl;
	}

	return 0;
}
```

* 方法2：使用队列。每个2个元素，将队列首元素删除。中间跳过的元素，先从队列首端弹出，再插入队列尾部。

```cpp
#include<iostream>
#include<queue>
using namespace std;

int main()
{
	int n;
	while (cin >> n)
	{
		queue<int> q;
		for (int i = 0; i<n; i++)
		{
			q.push(i);
		}
		int count = 0;
		while (q.size() != 1)
		{
			if (count != 2)
			{
				int b = q.front();
				q.pop();
				q.push(b); //将队首元素插入队末
				count++;
			}
			else
			{
				q.pop();
				count = 0;
			}
		}
		int c = q.front();
		cout << c << endl;
	}
	return 0;
}
```


## 字符集合

### 题目
* 输入一个字符串，求出该字符串包含的字符集合
* 输入描述:
每组数据输入一个字符串，字符串最大长度为100，且只包含字母，不可能为空串，区分大小写。
* 输出描述:
每组数据一行，按字符串原有的字符顺序，输出字符集合，即重复出现并靠后的字母不输出。

* 输入例子:
abcqweracb
* 输出例子:
abcqwer


### 分析

* 对于vector容器或数组，使用`find()`函数。阅读[std::find Tutorials](http://www.cplusplus.com/reference/algorithm/find/)了解更多。

> `template <class InputIterator, class T>  InputIterator find (InputIterator first, InputIterator last, const T& val);`  
> Find value in range
>Returns an **iterator** to the first element in the range **[first,last)** that compares  equal to val. If no such element is found, the function returns last.
> --- [std::find Tutorials](http://www.cplusplus.com/reference/algorithm/find/)


```cpp
#include <iostream>     
#include <algorithm>   
#include <vector>      
using namespace std;

int main() {
	// using std::find with array and pointer:
	int myints[] = { 10, 20, 30, 40 };
	int * p;
	p = find(myints, myints + 4, 30);
	//或者使用auto
	/*
	auto dem= find(myints, myints + 4, 30);
	if (dem != myints + 4) {
		cout << "Element found in myints: " << *dem << '\n';  //注意此处使用*dem
	}
	*/
	if (p != myints + 4)
		cout << "Element found in myints: " << *p << '\n';
	else
		cout << "Element not found in myints\n";

	// using std::find with vector and iterator:
	vector<int> myvector = {10,20,30,40};
	vector<int>::iterator it;
	//或者使用auto
	//auto it= find(myvector.begin(), myvector.end(), 30);

	it = find(myvector.begin(), myvector.end(), 30);
	if (it != myvector.end())
		cout << "Element found in myvector: " << *it << '\n';
	else
		cout << "Element not found in myvector\n";

	system("pause");
	return 0;
}
```


* 对于string，使用`find()`函数。阅读[std::string::find Tutorials](http://www.cplusplus.com/reference/string/string/find/)了解更多。


>Return Value
>The position of the first character of the first match.
>If no matches were found, the function returns `string::npos`.
> --- [std::string::find Tutorials](http://www.cplusplus.com/reference/string/string/find/)


```cpp

#include <iostream>       
#include <string>        
using namespace std;

int main()
{
	string str("There are two needles in this haystack with needles.");
	string str2("needle");

	// different member versions of find in the same order as above:
	size_t found = str.find(str2);
	if (found != string::npos)
		cout << "first 'needle' found at: " << found << '\n';

	found = str.find("needles are small", found + 1, 6);
	if (found != string::npos)
		std::cout << "second 'needle' found at: " << found << '\n';

	found = str.find("haystack");
	if (found != string::npos)
		std::cout << "'haystack' also found at: " << found << '\n';

	found = str.find('.');
	if (found != string::npos)
		std::cout << "Period found at: " << found << '\n';

	// let's replace the first needle:
	str.replace(str.find(str2), str2.length(), "preposition");
	std::cout << str << '\n';

	return 0;
}
```


### 求解

* 方法1：使用`find(vect.begin(),vect.end(),ele)`判断。

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;
int main()
{
    string s;
    vector<char> cvec;
    while (cin >> s)
    {
        for (int i = 0; i < s.size(); ++i)
        {
            auto beg = find(cvec.begin(), cvec.end(), s[i]);
            if (beg == cvec.end())
            {
                cvec.push_back(s[i]);
                cout << s[i];
            }
        }      
        cout << endl;
        s.clear(); //清零
        cvec.clear();//清零
    }
    return 0;
}
```


* 方法2：使用`string.find() != string::npos`判断。

```
#include <iostream>
#include <string>
using namespace std;

string strSet(string str){
	string res;
    int size = str.size(); 
    for(int i=0;i<size;i++){
    	if(res.find(str[i]) == string::npos){
            res += str[i];
        }
    }
    return res;
}

int main(){
    string str;
    while(cin>>str){
        cout<<strSet(str)<<endl;
    }
    return 0;
}
```



## 数独

### 题目
* 数独是一个我们都非常熟悉的经典游戏，运用计算机我们可以很快地解开数独难题，现在有一些简单的数独题目，请编写一个程序求解。
* 输入描述:
输入9行，每行为空格隔开的9个数字，为0的地方就是需要填充的。
* 输出描述:
输出九行，每行九个空格隔开的数字，为解出的答案。


### 分析
* 参考资料
	- [数独游戏介绍](http://www.aichengxu.com/other/5618118.htm)
	- [数独(DFS)求解](http://blog.csdn.net/you_are_my_dream/article/details/54864775)
	- [DFS搜索中的回溯](http://wenku.baidu.com/link?url=tyGTRlN_eJzF9rxNjp0MMrDF2LRT0PsvfaDIe8AJHj_zdnCJLq-kpJaJyEENxpw7H1jx5-PvVWyj5hGhOz7kCyPyYNpqOnCBY5YSxaVnyX3)
* 数独游戏中要求在9*9的方格中，每行数字，每列数字都包括1~9数字，并且不能出现重复数字。此外，还要求，每个小的3*3九宫格中，数字也不能重复，要包括1~9数字。
* 针对上述要求，可以编写两个判断函数，对数字是否符合进行判断。一个是判断数字所在行和所在列是否含有重复数字，一个是判断小的3*3九宫格中是否有重复数字。
* 对81个点进行编号，从0~80，采用DFS深度优先搜索的思想，遍历所有的点，如果该点符合情况，则判断下一个点。如果不合适，则**回溯**到上一个点进行判断。
* 注意，判断过程中是有回溯的可能性的，因此，**一定要记得将原来是0的位置重新置于0**，否则回溯之后下次再判断时候会误将该位置的数字当作已知数字处理。如下程序所示。

```cpp
if (datas[row][col] == 0) {
		//该点数字未填充
		//遍历1~9数字，尝试填充
		for (int num = 1; num <= 9; num++) {
			datas[row][col] = num;
			//判断是否合理
			if (rowColJudge(row, col, num) && subJudge(row, col, num)) {
				//递归，判断后续所有的点，如果无解，则回溯!!
				if (dfs(pointNum + 1)) {
					return true;
				}
			}
			//回溯清0  ！！！！
			//若发生回溯，则将该位置0，便于下次判断
			//如果不进行该操作，回溯的位置会被置于9
			//下次再判断时候会误操作,将该位置当作已知数字处理
			datas[row][col] = 0;
		}
```

* 此外，注意输出的格式，每行9个数字，最后一个数字不要带空格。

```cpp
//输出结果
for (int i = 0; i<9; i++) {
	for (int j = 0; j<9; j++) {
		//每行最后一个数字后面不带空格
		cout << datas[i][j] << " ";
	}
	cout << endl;
}
```
* 测试用例 
7 2 6 9 0 4 0 5 1 
0 8 0 6 0 7 4 3 2 
3 4 1 0 8 5 0 0 9 
0 5 2 4 6 8 0 0 7 
0 3 7 0 0 0 6 8 0 
0 9 0 0 0 3 0 1 0 
0 0 0 0 0 0 0 0 0 
9 0 0 0 2 1 5 0 0 
8 0 0 3 0 0 0 0 0 
 
* 对应输出 
7 2 6 9 3 4 8 5 1 
5 8 9 6 1 7 4 3 2 
3 4 1 2 8 5 7 6 9 
1 5 2 4 6 8 3 9 7 
4 3 7 1 9 2 6 8 5 
6 9 8 5 7 3 2 1 4 
2 1 5 8 4 6 9 7 3 
9 6 3 7 2 1 5 4 8 
8 7 4 3 5 9 1 2 6 


### 求解


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

int datas[9][9] = { 0 };  //用于保存数据

 //在[row,col]位置插入num值，是否在其3*3的子区域中合理
bool subJudge(int row, int col, int num) {
	int r = row / 3; //子区域的行编号值
	int c = col / 3; //子区域的列编号值
	for (int i = (r * 3); i<(r * 3 + 3); i++) {
		for (int j = (c * 3); j<(c * 3 + 3); j++) {
			if (datas[i][j] == num) {
				if (i == row && j == col) {
					continue;
				}
				else {
					return false;
				}
			}
		}
	}
	return true;
}


//在[row,col]位置插入num值，是否在该行和该列合理
bool rowColJudge(int row, int col, int num) {

	for (int i = 0; i<9; i++) {
		if (datas[row][i] == num && i != col) {
			return false;
		}
		if (datas[i][col] == num && i != row) {
			return false;
		}
	}
	return true;
}

//深度优先搜索   递归实现 
//pointNum 九宫格中点的编号 0~80

bool dfs(int pointNum) {
	//递归终止条件
	if (pointNum >= 81) {
		return true;
	}
	int row = pointNum / 9; //该点所在的行号
	int col = pointNum % 9; //该点所在的列号

	if (datas[row][col] == 0) {
		//该点数字未填充
		//遍历1~9数字，尝试填充
		for (int num = 1; num <= 9; num++) {
			datas[row][col] = num;
			//判断是否合理
			if (rowColJudge(row, col, num) && subJudge(row, col, num)) {
				//递归，判断后续所有的点，如果无解，则回溯!!
				if (dfs(pointNum + 1)) {
					return true;
				}
			}
			//若发生回溯，则将该位置0，便于下次判断
			datas[row][col] = 0;
		}
	}
	else {
		//该点数字已经填充,开始判断下一个点
		return dfs(pointNum + 1);
	}
	return false;
}

int main() {

	while (cin >> datas[0][0]) {
		//获取数据
		for (int i = 0; i<9; i++) {
			for (int j = 0; j<9; j++) {
				if (i == 0 && j == 0) {
					continue;  //跳过data[0][0]
				}
				cin >> datas[i][j];
			}
		}
		//进行深度优先搜索
		dfs(0);
		//输出结果
		for (int i = 0; i<9; i++) {
			for (int j = 0; j<9; j++) {
				cout << datas[i][j] << " ";
			}
			cout << endl;
		}
	}
	return 0;
}
```

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 