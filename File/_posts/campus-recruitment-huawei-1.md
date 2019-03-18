---
title: 华为2016校园招聘上机笔试题
date: 2017-03-19 20:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,华为笔试]
keywords: 校招笔试编程题,华为笔试
---



* 本文主要对华为笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/260145/summary)
* 下载本文中的[华为笔试试题](http://static.nowcoder.com/pdf/paper/260145.pdf)

<!--more-->


##  最高分是多少
### 题目
* 老师想知道从某某同学当中，分数最高的是多少，现在请你编程模拟老师的询问。当然，老师有时候需要更新某位同学的成绩. 
* 输入描述
	- **输入包括多组测试数据。**
	- 每组输入第一行是两个正整数`N`和`M`（`0 < N <= 30000`, `0 < M < 5000`）,分别代表学生的数目和操作的数目。
	- 学生ID编号从1编到N。
	- 第二行包含N个整数，代表这N个学生的初始成绩，其中第i个数代表ID为i的学生的成绩。
	- 接下来又M行，每一行有一个字符C（只取`Q`或`U`），和两个正整数A,B,当C为'Q'的时候, 表示这是一条询问操作，他询问ID从A到B（包括A,B）的学生当中，成绩最高的是多少
	- 当C为‘U’的时候，表示这是一条更新操作，要求把ID为A的学生的成绩更改为B。
* 输出描述
	- 对于每一次询问操作，在一行里面输出最高成绩.
* 输入例子

```
5 7
1 2 3 4 5
Q 1 5
U 3 6
Q 3 4
Q 4 5
U 4 5
U 2 9
Q 1 5
```

* 输出例子

```
5
6
5
9
```

### 分析
* 易错点
	- 输入的数据不止一组
	- 参数A可能大于B，程序中需要判断
* 思路1：暴力求解。修改复杂度`O(1)`，查询复杂度`O(N)`。
* 思路2：线段树。修改`O(logn)`，查询`O(logn)`。

### 求解
* 方法1：暴力求解，参考思路1。

```cpp
#include <iostream>
#include <vector>
using namespace std;

void teacherQuery() {
	int N, M, A, B;
	char command;
	int max;
	int temp;
	while (cin >> N >> M) {
		vector<int> arr;   //定义在while大循环之内
		for (int i = 0; i < N; i++) {
			cin >> temp;
			arr.push_back(temp);
		}
		for (int j = 0; j < M; j++) {
			cin >> command >> A >> B;
			if (command == 'Q') {
				if (A > B) {
					int x = A;
					A = B;
					B = x;
				}
				max = arr[A - 1];
				for (int k = A + 1; k <= B; k++) {
					if (arr[k - 1] > max) {
						max = arr[k - 1];
					}
				}
				cout << max << endl;
			}
			else if (command == 'U') {
				arr[A - 1] = B;
			}
		}
	}
}

int main() {
	teacherQuery();
	return 0;
}
```
需要注意的是
 (1) 注意比较A和B的大小；
 (2) 程序中注意逻辑关系。例如，示例1中，`vector<int> arr;`定义在while大循环内。因此，对于每组测试数据，`arr`都是新建的，其中不含数据，可以直接用`push_back`插入数据。

```cpp
//示例1
while (cin >> N >> M) {
	vector<int> arr;   //定义在while大循环之内
	for (int i = 0; i < N; i++) {
		cin >> temp;
		arr.push_back(temp);
	}
	//...
}
```
但是若将`vector<int> arr;`定义在while大循环外，依旧使用`push_back`插入数据时，就会发生错误。因为第2次测试数据中，`push_back`操作时候，`arr`中会保留第1次测试的数据。如下示例2所示。

```cpp
//示例2
vector<int> arr;   //定义在while大循环之内
while (cin >> N >> M) {
	for (int i = 0; i < N; i++) {
		cin >> temp;
		arr.push_back(temp);
	}
	//...
}
```
改正的方法为，每次循环开始时候先将`arr`清空，或者使用`cin >> arr[i];`操作，直接覆盖上一组测试数据。如下示例3所示。
```cpp
//示例3
vector<int> arr(N)   //定义在while大循环之内
while (cin >> N >> M) {
	for (int i = 0; i < N; i++) {
		cin >> arr[i];
	}
	//...
}
```



* 方法2：线段树，参考思路2。

示例程序1如下所示。题目中指定了N的最大长度为`100000`
 

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

const int MAXN = 100000;
int arr[MAXN + 5];
int st[MAXN * 4 + 5];

void build(int index, int l, int r) {
	if (l == r) {
		st[index] = arr[l];
		return;
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	build(lchild, l, m);
	build(rchild, m + 1, r);
	st[index] = max(st[lchild], st[rchild]);
}

int queryMax(int L, int R, int index, int l, int r) {
	if (L <= l && R >= r) {
		return st[index];
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	int lres = -1;
	int rres = -1;
	if (L <= m) {
		lres = queryMax(L, R, lchild, l, m);
	}
	if (m < R) {
		rres = queryMax(L, R, rchild, m + 1, r);
	}
	if (lres == -1) {
		return rres;
	}
	if (rres == -1) {
		return lres;
	}
	return max(lres, rres);
}

void update(int arr_index, int value, int index, int l, int r) {
	if (l == r && l == arr_index) {
		st[index] = value;
		return;
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	if (arr_index <= m) {
		update(arr_index, value, lchild, l, m);
	}
	else {
		update(arr_index, value, rchild, m + 1, r);
	}
	st[index] = max(st[lchild], st[rchild]);
}

int main() {
	int N, M, A, B;
	char c;
	while (cin >> N >> M) {
		for (int i = 0; i<N; i++) {
			cin >> arr[i];
		}
		build(0, 0, N - 1);
		for (int j = 0; j<M; j++) {
			cin >> c >> A >> B;
			if (c == 'U') {
				update(A - 1, B, 0, 0, N - 1);
			}
			else if (c == 'Q') {
				if (A>B) {
					swap(A, B);
				}
				cout << queryMax(A - 1, B - 1, 0, 0, N - 1) << endl;
			}
		}
	}
	return 0;
}
```
上述程序中设定了线段树可能的最大长度值。在一定程度上会造成空间的浪费。可以动态确定线段树所需的空间。如下示例程序2和示例程序3所示。但是很遗憾，示例程序2和示例程序3在本地调试通过，但是在牛客网提交会出现超时。
 

示例程序2如下所示。

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

//获取区间[left,right]中点坐标
int getMid(int left, int right) {
	return left + ((right - left) >> 1);
}
//构建线段树递归函数
//为区间[left,right]构建线段树
//index为当前线段树中节点下标
int constructSTUtil(int arr[], int left, int right, int *st, int index) {
	//递归终止条件
	if (left == right) {
		st[index] = arr[left];
	}
	else {
		//递归
		int mid = getMid(left, right);
		st[index] = max(constructSTUtil(arr, left, mid, st, 2 * index + 1),
			constructSTUtil(arr, mid + 1, right, st, 2 * index + 2));
	}
	return st[index];
}

//为区间[left,right]构建线段树
//函数为线段树分配内存空间
//并调用函数constructSTUtil()来填充分配的内存空间
//函数返回类型是一个int型指针
//arr[]为给定的数组，n为数组长度
int *constructST(int arr[], int n) {
	// 为线段树分配内存空间
	int depth = (int)(ceil(log2(n))); //线段树的深度，从0开始计数
	int max_size = (int)pow(2, depth + 1) - 1; //线段树的最大容量（满二叉树状态）
	int *st = new int[max_size];

	//填充线段树st
	constructSTUtil(arr, 0, n - 1, st, 0);

	// 返回构建的线段树
	return st;
}


//递归函数 在线段树区间[left,right]中查询数组区间[q_left,q_right]最大值操作  
//index为当前线段树中节点下标
//初试调用该函数时传入index=0  根节点坐标
int getMaxUtil(int *st, int left, int right, int q_left, int q_right, int index) {
	//查询区间[q_left,q_right]不在线段树区间[left,right]内
	if (q_left >right || q_right < left) {
		return 0;
	}
	else if (left >= q_left && right <= q_right) {
		//线段树区间[left,right]是查询区间[q_left,q_right]的一部分
		return st[index];
	}
	else if (q_left <= right || q_right >= left) {
		//线段树区间[left,right]和查询区间[q_left,q_right]有交集
		int mid = getMid(left, right);
		return max(getMaxUtil(st, left, mid, q_left, q_right, 2 * index + 1),
			getMaxUtil(st,mid+1,right, q_left, q_right, 2 * index + 2));
	}
}
//查询区间[q_left,q_right]最大值操作
//n为数组长度
int getMax(int *st, int n, int q_left, int q_right) {
	//输入查询区间不合法
	if (q_left < 0 || q_right > n - 1 || q_left > q_right) {
		cout << "Invalid Input" << endl;
		return -1;
	}
	else {
		return getMaxUtil(st, 0, n - 1, q_left, q_right, 0);
	}
}

//更新下标位于给定区间内节点值的递归函数，
//更新了数组中第arr_index个元素
//区间[left,right],线段树中当前点的下标为index
void updateValueUtil(int *st, int left, int right, int arr_index, int new_val, int index) {
	//数组下标节点不在当前区间[left,right]
	if (arr_index < left || arr_index > right) {
		return;
	}
	else {
		//数组下标节点在当前区间[left,right]
		//更新节点和其子节点的值
		st[index] = max(st[index],new_val);
		if (left != right) {
			int mid = getMid(left, right);
			updateValueUtil(st, left, mid, arr_index, new_val, 2 * index + 1);
			updateValueUtil(st, mid + 1, right, arr_index, new_val, 2 * index + 2);
		}
	}
} 

// 更新输入数组与线段树值的函数
// 使用递归函数 updateValueUtil() 来更新线段树的值
// 更新数组中第arr_index个元素的值，新值为new_val
// n为数组长度
void updateValue(int arr[], int *st, int n, int arr_index, int new_val) {
	//检查错误的输入下标
	if (arr_index < 0 || arr_index > n - 1) {
		cout << "Invalid Input" << endl;
		return;
	}
	
	//更新数组值
	arr[arr_index] = new_val;

	//更新线段树
	updateValueUtil(st, 0, n - 1, arr_index, new_val, 0);
}


int main() {
	int arr[] = {1,3,5,7,9,11};
	//int arr[] = { 1,2,3 };
	int n = sizeof(arr) / sizeof(arr[0]);
	
	int *st = constructST(arr,n);

	cout << getMax(st,n,1,5) << endl;
	updateValue(arr,st,n,3,20);  //更新arr[3] = 20
	cout << getMax(st, n, 1, 5) << endl;


	return 0;
}
```


示例程序3如下所示。使用`resize()`重新分配vector容器的大小。

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
void buildUtil(vector<int> arr, vector<int> &st, int index, int l, int r) {
	if (l == r) {
		st[index] = arr[l];
		return;
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	buildUtil(arr, st, lchild, l, m);
	buildUtil(arr, st, rchild, m + 1, r);
	st[index] = max(st[lchild], st[rchild]);
}

void build(vector<int> arr,vector<int> &st,int index, int l, int r) {
	int depth = (int)ceil(log2(arr.size()));
	int max_size = (2 << depth) - 1;
	st.resize(max_size);
	buildUtil(arr, st, index, l, r);	
}

int queryMax(vector<int> st,int L, int R, int index, int l, int r) {
	if (L <= l && R >= r) {
		return st[index];
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	int lres = -1;
	int rres = -1;
	if (L <= m) {
		lres = queryMax(st,L, R, lchild, l, m);
	}
	if (m < R) {
		rres = queryMax(st,L, R, rchild, m + 1, r);
	}
	if (lres == -1) {
		return rres;
	}
	if (rres == -1) {
		return lres;
	}
	return max(lres, rres);
}

void update(vector<int> st,int arr_index, int value, int index, int l, int r) {
	if (l == r && l == arr_index) {
		st[index] = value;
		return;
	}
	int m = (l + r) >> 1;
	int lchild = (index << 1) + 1;
	int rchild = (index << 1) + 2;
	if (arr_index <= m) {
		update(st,arr_index, value, lchild, l, m);
	}
	else {
		update(st,arr_index, value, rchild, m + 1, r);
	}
	st[index] = max(st[lchild], st[rchild]);
}

int main() {
	int N, M, A, B;
	char c;
	vector<int> arr;
	vector<int> st;
	while (cin >> N >> M) {
		arr.resize(N);
		for (int i = 0; i<N; i++) {
			cin >> arr[i];
		}
		build(arr,st,0, 0, N - 1);
		for (int j = 0; j<M; j++) {
			cin >> c >> A >> B;
			if (c == 'U') {
				update(st,A - 1, B, 0, 0, N - 1);
			}
			else if (c == 'Q') {
				if (A>B) {
					swap(A, B);
				}
				cout << queryMax(st,A - 1, B - 1, 0, 0, N - 1) << endl;
			}
		}
	}
	return 0;
}
```

## 简单错误记录
### 题目
* 开发一个简单错误记录功能小模块，能够记录出错的代码所在的文件名称和行号。处理:
	- 记录最多8条错误记录，对相同的错误记录(即文件名称和行号完全匹配)只记录一条，错误计数增加；(文件所在的目录不同，文件名和行号相同也要合并)
	- 超过16个字符的文件名称，只记录文件的最后有效16个字符；(如果文件名不同，而只是文件名的后16个字符和行号相同，也不要合并)
	- 输入的文件可能带路径，记录文件名称不能带路径

* 输入描述:
	- 一行或多行字符串。每行包括带路径文件名称，行号，以空格隔开。
	- 文件路径为windows格式
	- 如：`E:\V1R2\product\fpgadrive.c  1325`

* 输出描述
	- 将所有的记录统计并将结果输出，格式：文件名代码行数数目，一个空格隔开，如: `fpgadrive.c 1325 1` 
	- 结果根据数目从多到少排序，数目相同的情况下，按照输入第一次出现顺序排序
	- 如果超过8条记录，则只输出前8条记录
	- 如果文件名的长度超过16个字符，则只输出后16个字符
* 输入例子
`E:\V1R2\product\fpgadrive.c 1325`
* 输出例子
`fpgadrive.c 1325 1`

### 分析
* 使用结构体保存文件名的信息，错误出现的行数和计数值。
* 保存所有的记录，最后结果只输出前8条记录即可。可以不考虑入队列和出队列的问题。
* 保存记录时候先保存文件名信息（即使文件名长度大于16），因为后续重复记录比较中是比较的文件名完整的信息，而不是截取后的16个有效长度信息。只是在最后输出结果时候对文件名长度进行判断，若大于16，则只输出最后16个有效长度。

### 求解


```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

struct FileInfo {
	string fileName;
	int lineNumber;
	int count;
	//构造函数
	FileInfo(string fN = "", int lN = 0, int ct = 0)
		:fileName(fN), lineNumber(lN), count(ct) {}
};


void errorRecord() {
	string str;
	int lines;
	string filePath;
	vector<FileInfo> res;
	while (cin >> str >> lines) {
		if (str == "") {
			break;
		}
		filePath = str.substr(str.rfind("\\") + 1);
		auto ite = res.begin();
		for (; ite != res.end(); ite++) {
			if ((ite->fileName == filePath) && (ite->lineNumber == lines)) {
				//记录重复
				break;
			}
		}
		if (ite != res.end()) {
			//记录重复
			ite->count++;
		}
		else {
			//插入新的记录
			res.push_back(FileInfo(filePath, lines, 1));
		}
	}
	//对结果进行排序

	auto comp = [](FileInfo a, FileInfo b) {
		return a.count > b.count;
	};

	sort(res.begin(), res.end(), comp);
	
	//输出结果
	//输出前8个记录
	//文件名大于16时，输出后16个有效长度
	int ct = 0;
	int resLength;
	for (auto ites = res.begin(); (ites != res.end() && ct<8); ites++, ct++) {
		resLength = ites->fileName.size();
		if (resLength > 16) {
			ites->fileName = ites->fileName.substr(resLength - 16);
		}
		cout << ites->fileName << " " << ites->lineNumber << " " << ites->count << endl;
	}

}

int main() {
	errorRecord();
	return 0;
}
```



## 扑克牌大小
### 题目
扑克牌游戏大家应该都比较熟悉了，一副牌由54张组成，含3~A，2各4张，小王1张，大王1张。牌面从小到大用如下字符和字符串表示（其中，小写`joker`表示小王，大写`JOKER`表示大王） 
`3 4 5 6 7 8 9 10 J Q K A 2 joker JOKER` 

输入两手牌，两手牌之间用`-`连接，每手牌的每张牌以空格分隔，`-`两边没有空格，如：`4 4 4 4-joker JOKER`

请比较两手牌大小，输出较大的牌，如果不存在比较关系则输出`ERROR`

基本规则：
（1）输入每手牌可能是个子，对子，顺子（连续5张），三个，炸弹（四个）和对王中的一种，不存在其他情况，由输入保证两手牌都是合法的，顺子已经从小到大排列；
（2）除了炸弹和对王可以和所有牌比较之外，其他类型的牌只能跟相同类型的存在比较关系（如，对子跟对子比较，三个跟三个比较），不考虑拆牌情况（如：将对子拆分成个子）
（3）大小规则跟大家平时了解的常见规则相同，个子，对子，三个比较牌面大小；顺子比较最小牌大小；炸弹大于前面所有的牌，炸弹之间比较牌面大小；对王是最大的牌；
（4）输入的两手牌不会出现相等的情况。

答案提示：
（1）除了炸弹和对王之外，其他必须同类型比较。
（2）输入已经保证合法性，不用检查输入是否是合法的牌。
（3）输入的顺子已经经过从小到大排序，因此不用再排序了。

输入描述:
输入两手牌，两手牌之间用“-”连接，每手牌的每张牌以空格分隔，“-”两边没有空格，如4 4 4 4-joker JOKER。

输出描述:
输出两手牌中较大的那手，不含连接符，扑克牌顺序不变，仍以空格隔开；如果不存在比较关系则输出ERROR。

输入例子:
`4 4 4 4-joker JOKER`

输出例子:
`joker JOKER`
### 分析
* 易错点
**比较的是一手牌的大小，即一次可以将牌全部出完，而不是分两次或多次出牌。**
* 思路
	 - 先判断牌的种类。可以根据空格数的多少来判断牌的种类。
	 - 如果有王炸或者炸弹，则王炸牌为大牌，炸弹次之。
	 - 如果牌的种类相同，则比较牌值的大小。

### 求解
需要注意的是，程序中主函数接受输入字符串的时候，不要使用`while(cin>>str)`。因为输入的字符串中包括空格，`cin`中遇见空格会默认输入已经结束。
  

```cpp
#include <iostream>
using namespace std;

//比较牌的大小
void compCards(string s1, string s2);
//获取牌的类型
int getCardsType(string str);
//获取牌面大小
int getCardsValue(string str);

int main() {
	string str;
	string s1, s2;
	int splitIndex = 0;
	while (getline(cin,str)) {  //不要使用cin ！！
		splitIndex = str.find("-");
		s1 = str.substr(0, splitIndex);  //第1手牌
		s2 = str.substr(splitIndex + 1);  //第2手牌
		compCards(s1, s2);
	}
	return 0;
}


//获取牌面大小
int getCardsValue(string str) {
	int res = -1;
	switch (str[0]) {
		case '3': {
			res = 3;
			break;
		}
		case '4': {
			res = 4;
			break;
		}
		case '5': {
			res = 5;
			break;
		}
		case '6': {
			res = 6;
			break;
		}
		case '7': {
			res = 7;
			break;
		}
		case '8': {
			res = 8;
			break;
		}
		case '9': {
			res = 9;
			break;
		}
		case '1': {
			res = 10;
			break;
		}
		case 'J': {
			res = 11;
			break;
		}
		case 'Q': {
			res = 12;
			break;
		}
		case 'K': {
			res = 13;
			break;
		}
		case 'A': {
			res = 14;
			break;
		}
		case '2': {
			res = 15;
			break;
		}
		case 'j': { //已经按顺序排列，不会出现J的情况
			res = 16;
			break;
		}
	}
	return res;
}

//获取牌的类型
int getCardsType(string str) {
	//根据空格数来计算牌的类型
	int spaceCount = 0;
	int length = str.size();
	for (int i = 0; i<length; i++) {
		if (str[i] == ' ') {
			spaceCount++;
		}
	}
	if (spaceCount == 0) {
		return 1;//单张牌
	}
	else if (spaceCount == 1) {
		if (str[0] == 'j') {//已经按顺序排列，不会出现J的情况
			return 4;//王炸
		}
		else {
			return 2;  //对子
		}
	}
	else if (spaceCount == 2) {
		return 3;//3张
	}
	else if (spaceCount == 3) {
		return 4;//炸弹
	}
	else if (spaceCount == 4) {
		return 5;//顺子
	}
	else {
		return -1;  //出现错误
	}
}

//比较牌的大小
void compCards(string s1, string s2) {
	int type1 = getCardsType(s1);
	int type2 = getCardsType(s2);
	int value1 = getCardsValue(s1);
	int value2 = getCardsValue(s2);
	int res;
	//res = 1 第1手牌大 
	//res = 2 第2手牌大 
	//res = 0 Error 
	if (type1 == 4 && type2 == 4) {
		//均为炸弹或者王炸
		res = (value1>value2) ? 1 : 2;
	}
	else if (type1 == 4 && type2 != 4) {
		res = 1;
	}
	else if (type1 != 4 && type2 == 4) {
		res = 2;
	}
	else if (type1 != type2) {
		//Error
		res = 0;
	}
	else if (type1 == type2) {
		res = (value1>value2) ? 1 : 2;
	}

	switch (res) {
		case 1: {
			cout << s1 << endl;
			break;
		}
		case 2: {
			cout << s2 << endl;
			break;
		}
		case 0: {
			cout << "ERROR" << endl;
			break;
		}
	}
}
```
## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 