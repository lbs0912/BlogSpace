---
title: 微软笔试
date: 2017-04-02 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,微软笔试]
keywords: 校招笔试编程题,微软笔试
---




* 本文主要对微软笔试题，进行分析和记录。其中，使用到了树这种数据结构。


<!--more-->

## Video game quest
### 题目
Little Hi is playing a video game. Each time he accomplishes a quest in the game, Little Hi has a chance to get a legendary item.

At the beginning the probality is `P%`. Each time Little Hi accomplishes a quest without getting a legendary item, the probality will go up `Q%`.

After getting a legendary item the probality will be reset to `floor(P/(2^l))`(ceil(x) represents the largest integer no more than x) where `l` is the number of legendary items he already has. The probality will also go up `Q%` each time Little Hi accomplishes a quest untill he gets  another legendary item.

Now Little Hi wants to know the expected number of quests he has to accomplish to get `N` legendary item.

Assume P=50, Q=75 and N=2, as the below figure shows the expected number of quest is

`2*50%*25% + 3*50%*75%*100% + 3*50%*100%*25% + 4*50%*100%*75%*100% = 3.25`

![video-game-2.jpg](http://ol3kbaay9.bkt.clouddn.com/video-game-2.jpg)

* Input
The first line contains three integers P, Q and N.
1<= N <= 10^6,  0<= P <= 100,   1<= Q <= 100

* Output
Output the expected number of quests rounded to 2 deimal places

* Test sample

```
//input
50  75   2
//output
3.25 
```
   
   
### 分析

分析上图可以发现，本题可以采用一个树结构求解。树的一些信息如下
* 所有的左节点表示得到legendary item，所有的右节点表示未抽中legendary item。即所有的右节点的item都为0。
* 计算概率时候先计算左子树的概率，其右子树概率为`1-左子树概率`。
* `2*50%*25% + 3*50%*75%*100% `计算式子中每一项前面的整数不是legendary item，而是抽取的次数。这在树结构中，等于树的深度（从0开始计算）。

* **题目中隐含`Q>=P`的限定。如果P大于Q，则树会无限制地生长下去。**如下图所示，下图中P=50，Q=40，N=1。从图中可以看出，树会无限制地生长下去。这是不科学的。

![video-game-3.PNG](http://ol3kbaay9.bkt.clouddn.com/video-game-3.PNG)


若树不出现无限制地生长，则需`100-P+Q>100`，即`Q>=P`。以P=10，Q=20，N=1为例，其树的结构如下图所示。

![video-game-4.PNG](http://ol3kbaay9.bkt.clouddn.com/video-game-4.PNG)


* C++ 中输出指定精度

```
#include <iomanip>

//显示bits位有效数字
cout.precision(bits);  //设定有效数字的个数(包括小数点之前的数字)


//显示bits位小数
cout.setf(ios::showpoints) //显示小数位
cout.precision(bits);  //设定精度（不包括小数点前的位数，只计算小数点后的位数）
cout.setf(ios::fixed);  //固定显示bits位小数
```

程序实现过程中，引入结构体。结构体信息如下所示。

```cpp
struct TreeNode {
	int item;  //节点的标号
	int cal_item;  //节点的累计标号
	int possibility; //概率
	int depth; //树的深度，从0开始
	double sum; //概率累计求和
	TreeNode* left;
	TreeNode* right;

	TreeNode(int _item = 0, int _cal = 0, int _p = 0, int _depth = 0, double _sum = 1)
		: item(_item), cal_item(_cal),possibility(_p), depth(_depth), sum(_sum),left(NULL), right(NULL) {};
};

```


针对此结构体，求解过程如下图所示。

![video-game-1.PNG](http://ol3kbaay9.bkt.clouddn.com/video-game-1.PNG)

创建树的过程充分利用了上述分析的树的特征信息。代码参见求解部分。

### 求解

```cpp
#include <iostream>
#include <string>
#include <cmath>
#include <algorithm>
#include <iomanip>
using namespace std;

struct TreeNode {
	int item;  //节点的标号
	int cal_item;  //节点的累计标号
	int possibility; //概率
	int depth; //树的深度，从0开始
	double sum; //概率累计求和
	TreeNode* left;
	TreeNode* right;

	TreeNode(int _item = 0, int _cal = 0, int _p = 0, int _depth = 0, double _sum = 1)
		: item(_item), cal_item(_cal),possibility(_p), depth(_depth), sum(_sum),left(NULL), right(NULL) {};
};

void buildTreeNode(TreeNode* &root, int P, int Q, int N, double &total) {
	if (root == NULL) {
		//递归终止条件1
		return;
	}
	int item = root->item;
	int cal_item = root->cal_item;
	int possibility = root->possibility;
	int depth = root->depth + 1;
	double sum = root->sum;
	if (item != 0) {
		possibility = (int)floor(P / (pow(2, item)));
	}
	else {
		if (possibility == 0) {
			//初始根节点
			possibility = P;
		}
		else {
			possibility = (possibility + Q >= 100) ? 100 : possibility + Q;
		}

	}
	item = max(item + 1, cal_item+1);
	cal_item = max(item, cal_item);
	sum = sum *  possibility / 100;
	root->left = new TreeNode(item, cal_item, possibility, depth, sum);    //构建左子树  中

    //右子树  不中
	possibility = 100 - possibility; //左右子树的概率和100
	sum = root->sum * possibility / 100;
	item = 0;
	cal_item = root->cal_item;
	if (possibility == 0) {
		//右子树为空
		root->right = NULL;
	}
	else {
		root->right = new TreeNode(item, cal_item, possibility, depth, sum);  //构建右子树  不中
	}

	//递归终止条件
	if ((root->left->item == N) && (root->right == NULL)) {
		//左子树和右子树均完成，递归终止
		total += root->left->sum * root->left->depth;
		return;
	}
	else if ((root->left->item == N) && (root->right != NULL)) {
		//左子树完成，但是右子树未完成
		//只递归右子树
		total += root->left->sum * root->left->depth;
		buildTreeNode(root->right, P, Q, N, total);
	}
	else {
		//左子树和右子树均未完成
		buildTreeNode(root->left, P, Q, N, total);
		buildTreeNode(root->right, P, Q, N, total);
	}
}

int main() {
	int P, Q, N;

	cout.setf(ios::showpoint); //显示小数位
	cout.precision(2);  //设定精度
	cout.setf(ios::fixed);  //固定显示2位小数



	while (cin >> P >> Q >> N) {
		double total = 0.0;
		if (P > Q) {
			cout << "Error" << endl;
		}
		else {
			TreeNode* root = new TreeNode(0, 0, 0, 0, 1);

			buildTreeNode(root, P, Q, N, total);
			cout << total << endl;
		}
		
	}
	return 0;
}
```

## Farthest Point
### 题目
* 描述

Given a circle on a two-dimentional plane.

Output the integral point in or on the boundary of the circle which has the largest distance from the center.

* 输入

One line with three floats which are all accurate to three decimal places, indicating the coordinates of the center x, y and the radius r.

For 80% of the data: |x|,|y|<=1000, 1<=r<=1000

For 100% of the data: |x|,|y|<=100000, 1<=r<=100000

* 输出

One line with two integers separated by one space, indicating the answer.

If there are multiple answers, print the one with the largest x-coordinate.

If there are still multiple answers, print the one with the largest y-coordinate.


* 样例输入
1.000 1.000 5.000

* 样例输出
6 1

### 分析
题目比较简单，直接处理即可。

先确定x取值范围，从x最大值开始遍历。利用两点之间距离公式求解。每一个x对应的两个y值，优先考虑较大的y值。

求解过程中，注意`ceil`和`floor`的选取。

### 求解

```cpp
#include <iostream>
#include <string>
#include <cmath>
#include <algorithm>
#include <vector>
#include <climits>
using namespace std;

int main() {
	double x0, y0, r;
	int x, y;
	double dis = 0;
	double temp;
	int resX, resY;
	while (cin >> x0 >> y0 >> r) {
		int xMax = floor(x0+r);  //x最大取值
		int xMin = ceil(x0 - r); //x最小取值

		//优先遍历x的较大值
		for (int x = xMax; x >= xMin; x--) {
			//优先遍历y的较大值
			//(y-y0)*(y-y0) + (x-x0)*(x-x0) = r*r
			
			//y在圆心上方
			y = floor(y0 + sqrt(r*r - (x - x0)*(x - x0)));
			temp = (y - y0)*(y - y0) + (x - x0)*(x - x0);
			if (temp > dis) {
				resX = x;
				resY = y;
				dis = temp;
			}

			//y在圆心下方
			y = ceil(y0 - sqrt(r*r - (x - x0)*(x - x0)));
			temp = (y - y0)*(y - y0) + (x - x0)*(x - x0);
			if (temp > dis) {
				resX = x;
				resY = y;
				dis = temp;
			}
		}
		cout << resX <<" "<< resY << endl;
	}
	system("pause");
	return 0;
}

```



## Total Highway Distance
### 题目
* 描述

Little Hi and Little Ho are playing a construction simulation game. They build N cities (numbered from 1 to N) in the game and connect them by N-1 highways. It is guaranteed that each pair of cities are connected by the highways directly or indirectly.

The game has a very important value called Total Highway Distance (THD) which is the total distances of all pairs of cities. Suppose there are 3 cities and 2 highways. The highway between City 1 and City 2 is 200 miles and the highway between City 2 and City 3 is 300 miles. So the THD is 1000(200 + 500 + 300) miles because the distances between City 1 and City 2, City 1 and City 3, City 2 and City 3 are 200 miles, 500 miles and 300 miles respectively.

During the game Little Hi and Little Ho may change the length of some highways. They want to know the latest THD. Can you help them?

* 输入

Line 1: two integers N and M.

Line 2 .. N: three integers u, v, k indicating there is a highway of k miles between city u and city v.

Line N+1 .. N+M: each line describes an operation, either changing the length of a highway or querying the current THD. It is in one of the following format.

EDIT i j k, indicating change the length of the highway between city i and city j to k miles.

QUERY, for querying the THD.

For 30% of the data: 2<=N<=100, 1<=M<=20

For 60% of the data: 2<=N<=2000, 1<=M<=20

For 100% of the data: 2<=N<=100,000, 1<=M<=50,000, 1 <= u, v <= N, 0 <= k <= 1000.

* 输出

For each QUERY operation output one line containing the corresponding THD.

* 样例输入
3 5
1 2 2
2 3 3
QUERY
EDIT 1 2 4
QUERY
EDIT 2 3 2
QUERY

* 样例输出
10
14
12

### 分析
采用最短距离思想求解。

构建一个无向图，计算每两个点之间的距离。最后再遍历该无向图，得到总距离THD的大小。需要注意的是，由于图是无向图，因此，`dis[i][j] = dis[j][i] = k`。对于输入的城市信息和路程，每次要初始化两个数组元素。

无向图构建完毕后，可以采用Floyd最短路径算法求解最短路径。

另外需要注意的是，题目中指明N个城市由N-1条公路连接，即构建的图中是没有环路的。因此，**如果跟新了其中一个边的权值，不需要重新调用Floyd算法构建最短路径（因为图中没有环路），只需在最后的总距离THD上进行修改即可，这样大大优化了算法。**

其修改方式为`THD = THD + (N-1)*(k-dis[i][j])`。更新的边，在总距离THD中被计算了N-1次。

### 求解

```cpp
#include <iostream>
#include <string>
#include <cmath>
#include <algorithm>
#include <vector>
#include <climits>
using namespace std;

void floydDis(vector<vector<int>> & dis) {
	int size = dis.size();
	for (int k = 1; k < size; k++) {
		for (int i = 1; i < size; i++) {
			for (int j = 1; j < size; j++) {
				if (dis[i][k] < INT_MAX && dis[k][j] < INT_MAX && dis[i][j] > dis[i][k] + dis[k][j]) {
					dis[i][j] = dis[i][k] + dis[k][j];
				}
			}
		}
	}
}
int main() {
	int N, M;
	int i, j, k;
	string str;
	int total;
	while (cin >> N >> M) {
		total = 0;
		vector<vector<int>> dis(N + 1, vector<int>(N + 1, INT_MAX));
				for (int x = 0; x < N - 1; x++) {
			cin >> i >> j >> k;
			dis[i][j] = k;
			dis[j][i] = k;  //无向距离  dis[i][j] = dis[j][i]
		}
		
		floydDis(dis);  //构建最短路径
		for (int m = 1; m <= N; m++) {
			for (int n = m + 1; n <= N; n++) {
				total += dis[m][n];  //路径和
			}
		}
		for (int y = 0; y < M; y++) {
			cin >> str;
			if (str == "QUERY") {
				cout << total << endl;
			}
			if (str == "EDIT") {
				cin >> i >> j >> k;
				total += (N - 1)*(k - dis[i][j]);
			}
		}
	}
	//system("pause");
	return 0;
}
```



## Fibonacci
### 题目

* 描述

Given a sequence {an}, how many non-empty sub-sequence of it is a prefix of fibonacci sequence.

A sub-sequence is a sequence that can be derived from another sequence by deleting some elements without changing the order of the remaining elements.

The fibonacci sequence is defined as below:

F1 = 1, F2 = 1

Fn = Fn-1 + Fn-2, n>=3

* 输入

One line with an integer n.

Second line with n integers, indicating the sequence {an}.

For 30% of the data, n<=10.

For 60% of the data, n<=1000.

For 100% of the data, n<=1000000, 0<=ai<=100000.

* 输出

One line with an integer, indicating the answer modulo 1,000,000,007.

* 样例提示

The 7 sub-sequences are:

```
{a2}

{a3}

{a2, a3}

{a2, a3, a4}

{a2, a3, a5}

{a2, a3, a4, a6}

{a2, a3, a5, a6}
```


* 样例输入
6
2 1 1 2 2 3
* 样例输出
7
### 分析
参考[微软笔试题 | CSDN](http://blog.csdn.net/wusecaiyun/article/details/48947595)了解更多。

采用动态规划思想求解。时间复杂度为O(n)。

设count[i]为以第i个斐波那契数结尾的斐波那契数列的个数，当遇到第i+1个斐波那契数的时候，可知`count[i+1] += count[i]`。**因为这个数(也就是第i+1个fibo数)，可以和之前的任意一个以第i个斐波那契数结尾的斐波那契数列组成一个新的以第i+1个斐波那契数结尾的斐波那契数列。**

需要注意的两点是
* 为了把斐波那契数和它在斐波那契数列中的位置建立联系，使用了`unordered_map`。其作用为将fibonacci数列映射为其出现的顺序。

```
fib(1) = 1;    //fibMap[1] = 1;      忽略
fib(2) = 1;    //fibMap[1] = 2;      忽略
fib(3) = 2;    fibMap[2] = 3;  
fib(4) = 3;    fibMap[3] = 4;
fib(5) = 5;    fibMap[5] = 5;
```
由于`unordered_map`中不允许关键字重复出现，因此，若将fibonacci的前2个数值也进行映射，会出现关键字的重复。因此，后续过程中不使用fibMap[1]，而是直接使用`num==1`进行判断。

* 如果遇到的数为1，则1既可以作为第2个斐波那契数，也可以作为第一个斐波那契数，因此在代码中处理为

```
count[2] += count[1];  //巧！！！
count[1]++;
```

* 未初始化长度的vector不能直接使用vec[index]增加元素，只能使用push_back
* 未初始化长度的map可以直接使用mapName[index]增加元素，会自动插入元素

### 求解

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int main(){
	
	
	vector<int> fibo;  //fibonacci数字
	unordered_map<int, int> fibMap;
	//将fibonacci数字和对应的顺序映射起来

	fibo.push_back(0);   //fibo[0] = 0; 
	fibo.push_back(1);   //fibo[1] = 1;
	fibo.push_back(1);   //fibo[2] = 1;
	
	//unordered_map中不允许关键字重复出现
	//因此fib(1)=1,fib(2)=1若也进行映射，会出现key的重复
	//因此，不再考虑前2个fib的映射，直接用后面的num==1进行判断
	//fibMap[0] = 0;
	//fibMap[1] = 1;
	//fibMap[1] = 2;

	//未初始化长度的vector不能直接使用vec[index]增加元素，只能使用push_back
	//未初始化长度的map可以直接使用mapName[index]增加元素，会自动插入元素


	int index = 2;
	
	while (fibo[index] <= 1000000) {
		index++;
		fibo.push_back(fibo[index - 1] + fibo[index - 2]);
		fibMap[fibo[index]] = index;
	}
	int size = fibo.size();

	int n,num;
	vector<long long> count(size,0);
	while (cin >> n) {
		for (int i = 0; i < n; i++) {
			cin >> num;
			if (num == 1) {
				count[2] += count[1];  //巧妙 ！！
				count[1]++;
			}
			else if(fibMap.count(num)) {  //如果num是fibonacci数字
				count[fibMap[num]] += count[fibMap[num]-1];  //count[i+1] += count[i]
			}
		}

		long long res = 0;  //long long 防止溢出
		for (int i = 0; i < size; i++) {
			res += count[i];
		}
		cout << res % 1000000007 << endl;
	}
	
	system("pause");
	return 0;

}

```

## Image Encryption
### 题目

* 描述

A fancy square image encryption algorithm works as follow:
0. consider the image as an N x N matrix

1. choose an integer k∈ {0, 1, 2, 3}

2. rotate the square image k * 90 degree clockwise

3. if N is odd stop the encryption process

4. if N is even split the image into four equal sub-squares whose length is N / 2 and encrypt them recursively starting from step 0

Apparently different choices of the k serie result in different encrypted images. Given two images A and B, your task is to find out whether it is POSSIBLE that B is encrypted from A. B is possibly encrypted from A if there is a choice of k serie that encrypt A into B.

* 输入

Input may contains multiple testcases.

The first line of the input contains an integer T(1 <= T <= 10) which is the number of testcases.

The first line of each testcase is an integer N, the length of the side of the images A and B.

The following N lines each contain N integers, indicating the image A.

The next following N lines each contain N integers, indicating the image B.

For 20% of the data, 1 <= n <= 15

For 100% of the data, 1 <= n <= 100, 0 <= Aij, Bij <= 100000000

* 输出

For each testcase output Yes or No according to whether it is possible that B is encrypted from A.

* 样例输入
3
2
1 2
3 4
3 1
4 2
2
1 2
4 3
3 1
4 2
4
4 1 2 3
1 2 3 4
2 3 4 1
3 4 1 2
3 4 4 1
2 3 1 2
1 4 4 3
2 1 3 2

* 样例输出
Yes
No
Yes

### 分析
* 易错点
若矩阵一维长度length为偶数，会将矩阵分成4小块，再对每小块进行判断。在判断过程中，这4个小块的旋转度数可以不同，每块可以任意选择旋转度数。

* 思路：DFS深度优先搜索

如果矩阵一维长度length为奇数或者length=2，则不进行DFS，直接判断是否满足加密关系。判断旋转0度，90度，180度，270度后，是否满足相等关系。

如果矩阵一维长度length为偶数，先对整个大矩阵进行旋转，再对分成的4个小矩阵进行判断。旋转大矩阵，共有4种情况，如下图所示。此时进行深度优先搜索DFS，对每一种情况下的4个小矩阵再进行判断。

![ms-isencrypted-1.PNG](http://ol3kbaay9.bkt.clouddn.com/ms-isencrypted-1.PNG)

以大矩阵旋转90度且length为偶数为例，则需要判断矩阵A旋转后的4个小矩阵是否和矩阵B的4个小矩阵相等。进行DFS，函数递归实现。代码如下。

```
//90度   C A D B
if (isEncrypted(dataA, dataB, length, xA + length, yA, xB, yB) &&
	isEncrypted(dataA, dataB, length, xA, yA, xB, yB + length) &&
	isEncrypted(dataA, dataB, length, xA + length, yA + length, xB + length, yB) &&
	isEncrypted(dataA, dataB, length, xA, yA + length, xB + length, yB + length)) {
	return true;
}
```


* 矩阵旋转数学公式推导

本题中考察了矩阵旋转的数学公式。

若矩阵左上角坐标为(0,0)，即相对原点进行旋转，

旋转90度对应的公式为`data[i][j] ---> data[j][n-1-i]`。

旋转180度（即在旋转90度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-i][n-1-j]`。

旋转270度（即在旋转180度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-j][i]`。

若矩阵左上角坐标为(xA,yA)，旋转过程中先将矩阵整体平移到原点（即`i=i-xA，j=j-yA`），再套用相对原点的矩阵旋转公式，最后再对将矩阵平移到参考点(xA,yA)（即将横坐标值加上xA，纵坐标值加上yA）

旋转90度对应的公式为`data[i][j] ---> data[j-yA+xA][n-1-i+xA+yA]`。逆推则有，` data[n-1+xA+yA-j][i+yA-xA] ---> data[i][j]`。

旋转180度（即在旋转90度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-i+2*xA][n-1-j+2*yA]`。逆推则有，`ddata[n-1+2*xA-i][n-1+2*yA-j] ---> data[i][j]`。

旋转270度（即在旋转180度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-j+xA+yA][i-xA+yA]`。逆推则有，`data[j+xA-yA][n-1+yA+xA-i] ---> data[i][j]`。

在程序设计时，判断两个矩阵的值是否相等，采用两个for循环实现。判断的是**旋转后的**A矩阵的值和矩阵B的值是否相等。for循环中坐标值是从左上到右下进行遍历的，因此，上述矩阵旋转公式需要进行逆推，来满足for循环中坐标值的顺序。



### 求解

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool judgeUtility(vector<vector<int>> &dataA, vector<vector<int>> &dataB, int length, 
	int xA, int yA, int xB, int yB) {

	int i, j;
	int tempXB, tempYB;
	//0度
	for (i = xA,tempXB = xB; i < xA + length; i++,tempXB++) {
		for (j = yA, tempYB = yB; j < yA + length; j++, tempYB++) {
			if (dataA[i][j] != dataB[tempXB][tempYB]) {
				i = xA + length;  //终止外层循环
				break;  //终止内层循环
			}
		}
	}
	//0度 判断成功
	if ((i == xA + length) && (j == yA + length)){
		return true;
	}

	//90度
	//若相对原点旋转(即左上角坐标为0,0)
	// data[i][j] ---> data[j][n-1-i]
	//同理，若左上角坐标为(xA,yA)
	// data[i][j] ---> data[j-yA+xA][n-1-i+xA+yA]
	//倒推有， data[n-1+xA+yA-j][i+yA-xA] ---> data[i][j]
	for (i = xA, tempXB = xB; i < xA + length; i++, tempXB++) {
		for (j = yA, tempYB = yB; j < yA + length; j++, tempYB++) {
			if (dataA[length - 1 + xA + yA - j][i+yA-xA] != dataB[tempXB][tempYB]) {
				i = xA + length;  //终止外层循环
				break;  //终止内层循环
			}
		}
	}
	//90度 判断成功
	if ((i == xA + length) && (j == yA + length)) {
		return true;
	}

	//180度
	//若相对原点旋转(即左上角坐标为0,0)
	// data[i][j] ---> data[n-1-i][n-1-j]
	//同理，若左上角坐标为(xA,yA)
	// data[i][j] ---> data[n-1-i+2*xA][n-1-j+2*yA]
	//倒推有， data[n-1+2*xA-i][n-1+2*yA-j] ---> data[i][j]
	for (i = xA, tempXB = xB; i < xA + length; i++, tempXB++) {
		for (j = yA, tempYB = yB; j < yA + length; j++, tempYB++) {
			if (dataA[length - 1 + 2*xA - i][length-1+2*yA-j] != dataB[tempXB][tempYB]) {
				i = xA + length;  //终止外层循环
				break;  //终止内层循环
			}
		}
	}
	//180度 判断成功
	if ((i == xA + length) && (j == yA + length)) {
		return true;
	}

	//270度
	//若相对原点旋转(即左上角坐标为0,0)
	// data[i][j] ---> data[n-1-j][i]
	//同理，若左上角坐标为(xA,yA)
	// data[i][j] ---> data[n-1-j+yA+xA][i-xA+yA]
	//倒推有，data[j+xA-yA][n-1+yA+xA-i] ---> data[i][j]
	for (i = xA, tempXB = xB; i < xA + length; i++, tempXB++) {
		for (j = yA, tempYB = yB; j < yA + length; j++, tempYB++) {
			if (dataA[j+xA-yA][length-1+yA+xA-i] != dataB[tempXB][tempYB]) {
				i = xA + length;  //终止外层循环
				break;  //终止内层循环
			}
		}
	}
	//270度 判断成功
	if ((i == xA + length) && (j == yA + length)) {
		return true;
	}
	return false;
}

bool isEncrypted(vector<vector<int>> &dataA,vector<vector<int>> &dataB,int length,
	int xA,int yA,int xB,int yB){
	
	//length是奇数或者length=2，不进行DFS深度优先判断
	if ((length % 2) != 0 || length == 2) {
		return judgeUtility(dataA,dataB,length,xA,yA,xB,yB);
	}
	else {
		length /= 2;
		//0度   A B C D
		if (isEncrypted(dataA, dataB, length, xA, yA, xB, yB) &&
			isEncrypted(dataA, dataB, length, xA, yA + length, xB, yB + length) &&
			isEncrypted(dataA, dataB, length, xA + length, yA, xB + length, yB) &&
			isEncrypted(dataA, dataB, length, xA + length, yA + length, xB + length, yB + length)) {
			
			return true;
		}
		//90度   C A D B
		if (isEncrypted(dataA, dataB, length, xA + length, yA, xB, yB) &&
			isEncrypted(dataA, dataB, length, xA, yA, xB, yB + length) &&
			isEncrypted(dataA, dataB, length, xA + length, yA + length, xB + length, yB) &&
			isEncrypted(dataA, dataB, length, xA, yA + length, xB + length, yB + length)) {

			return true;
		}
		//180度   D C B A
		if (isEncrypted(dataA, dataB, length, xA + length, yA + length, xB, yB) &&
			isEncrypted(dataA, dataB, length, xA + length, yA, xB, yB + length) &&
			isEncrypted(dataA, dataB, length, xA, yA + length, xB + length, yB) &&
			isEncrypted(dataA, dataB, length, xA, yA, xB + length, yB + length)) {

			return true;
		}
		//270度   B D A C
		if (isEncrypted(dataA, dataB, length, xA, yA + length, xB, yB) &&
			isEncrypted(dataA, dataB, length, xA + length, yA + length, xB, yB + length) &&
			isEncrypted(dataA, dataB, length, xA, yA, xB + length, yB) &&
			isEncrypted(dataA, dataB, length, xA + length, yA, xB + length, yB + length)) {

			return true;
		}
	}
	return false;
}


int main() {
	int items;
	int length;
	bool res;
	
	while (cin >> items) {
		for (int i = 0; i < items; i++) {
			cin >> length;
			vector<vector<int>> dataA(length,vector<int>(length,0));
			vector<vector<int>> dataB(length, vector<int>(length, 0));
			for (int i = 0; i < length; i++) {
				for (int j = 0; j < length; j++) {
					cin >> dataA[i][j];
				}
			}
			for (int i = 0; i < length; i++) {
				for (int j = 0; j < length; j++) {
					cin >> dataB[i][j];
				}
			}
			res = isEncrypted(dataA, dataB, length,0,0,0,0);
			if (res == true) {
				cout << "Yes" << endl;
			}
			if (res == false) {
				cout << "No" << endl;
			}
		}
	}

	system("pause");
	return 0;
}
```

## 参考资料
[1] [微软笔试题 | CSDN](http://blog.csdn.net/wusecaiyun/article/details/48947595)

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 