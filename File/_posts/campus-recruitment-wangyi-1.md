---
title: 网易2016研发工程师编程题
date: 2017-03-20 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,网易笔试]
keywords: 校招笔试编程题,网易笔试
---


* 本文主要对网易笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/970447/summary)
* 下载本文中的[网易笔试试题](http://static.nowcoder.com/pdf/paper/%E7%BD%91%E6%98%932016%E7%A0%94%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%BC%96%E7%A8%8B%E9%A2%98.pdf)

<!--more-->


##  小易的升级之路
### 题目
小易经常沉迷于网络游戏.有一次,他在玩一个打怪升级的游戏,他的角色的初始能力值为 a.在接下来的一段时间内,他将会依次遇见n个怪物,每个怪物的防御力为b1,b2,b3...bn. 如果遇到的怪物防御力bi小于等于小易的当前能力值c,那么他就能轻松打败怪物,并 且使得自己的能力值增加bi;如果bi大于c,那他也能打败怪物,但他的能力值只能增加bi 与c的最大公约数.那么问题来了,在一系列的锻炼后,小易的最终能力值为多少?

* 输入描述
对于每组数据,第一行是两个整数n(1≤n<100000)表示怪物的数量和a表示小易的初始能力值.
第二行n个整数,b1,b2...bn(1≤bi≤n)表示每个怪物的防御力

* 输出描述
对于每组数据,输出一行.每行仅包含一个整数,表示小易的最终能力值

* 输入例子
3 50
50 105 200
5 20
30 20 15 40 100

* 输出例子
110
205

### 分析
辗转相除法求最大公约数。
 
### 求解


```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int gcd(int a,int b){
	if(b == 0){
        return a;
    }
    else{
        return gcd(b,a%b);
    }
}

int finalValue(vector<int> arr,int &a){
    int size = arr.size();
    int temp = 0;
    for(int i=0;i<size;i++){
        if(arr[i] <= a){
            a += arr[i];
        }
        else{
            temp = a;
            if(arr[i] > temp){
                swap(arr[i],temp);
            }
            a += gcd(temp,arr[i]);
        }
    }
    return a;
}

int main(){
    int n,a;
    while(cin>>n>>a){
        vector<int> arr(n);
        for(int i=0;i<n;i++){
            cin>>arr[i];
        }
        cout<<finalValue(arr,a)<<endl;
    }
    return 0;
}
```

## 炮台攻击
### 题目
兰博教训提莫之后,然后和提莫讨论起约德尔人,谈起约德尔人,自然少不了一个人,那 就是黑默丁格------约德尔人历史上最伟大的科学家. 提莫说,黑默丁格最近在思考一个问题:黑默丁格有三个炮台,炮台能攻击到距离它R的敌人 (两点之间的距离为两点连续的距离,例如(3,0),(0,4)之间的距离是5),如果一个炮台能攻击 到敌人,那么就会对敌人造成1×的伤害.黑默丁格将三个炮台放在N*M方格中的点上,并且给出敌人 的坐标. 问:那么敌人受到伤害会是多大?

* 输入描述
第一行9个整数,R,x1,y1,x2,y2,x3,y3,x0,y0.R代表炮台攻击的最大距离,(x1,y1),(x2,y2),
(x3,y3)代表三个炮台的坐标.(x0,y0)代表敌人的坐标.
* 输出描述
输出一行,这一行代表敌人承受的最大伤害,(如果每个炮台都不能攻击到敌人,输出0×
* 输入例子
1 1 1 2 2 3 3 1 2
* 输出例子
2x
### 分析
注意count清零。
### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

struct Point {
	int x;
	int y;
	Point(int _x = 0, int _y = 0) :x(_x), y(_y) {};
};

double disSquare(Point a, Point b) {
	return (double)(a.x - b.x)*(a.x - b.x) + (double)(a.y - b.y)*(a.y - b.y);
}

int main() {
	double length;
	vector<Point> points(4);
	vector<int> arr(9);
	int count;
	while (cin >> length) {
        count = 0;  //清零
		for (int i = 0; i<4; i++) {
			cin >> points[i].x;
			cin >> points[i].y;
		}
		length *= length;
		for (int j = 0; j<3; j++) {
			if (disSquare(points[j], points[3]) <= length) {
				count += 1;
			}
		}
		cout << count << 'x' << endl;
	}
	
	return 0;
}

```



## 扫描透镜
### 题目
* 在`N*M`的草地上,提莫种了`K`个蘑菇,蘑菇爆炸的威力极大,兰博不想贸然去闯,而且蘑菇是隐形的.只 有一种叫做扫描透镜的物品可以扫描出隐形的蘑菇,于是他回了一趟战争学院,买了2个扫描透镜,一个 扫描透镜可以扫描出(3*3)方格中所有的蘑菇,然后兰博就可以清理掉一些隐形的蘑菇. 问:兰博最多可以清理多少个蘑菇?
* 注意：每个方格被扫描一次只能清除掉一个蘑菇。 
* 输入描述
第一行三个整数:N,M,K,(1≤N,M≤20,K≤100),N,M代表了草地的大小;
接下来K行,每行两个整数x,y(1≤x≤N,1≤y≤M).代表(x,y)处提莫种了一个蘑菇.
一个方格可以种无穷个蘑菇.

* 输出描述:
输出一行,在这一行输出一个整数,代表兰博最多可以清理多少个蘑菇.

### 分析
参考[网易16年研发笔试题 - 扫描透镜](http://blog.csdn.net/roamer_nuptgczx/article/details/52122258)了解更多。

该题目整体思路比较简单，遍历区域中的3*3九宫格区域，找出第1次搜索的最大值。然后将该区域中的蘑菇去除（每个空格只去除1次）。然后再次遍历九宫格区域，找出第2次搜索的最大值。但是由于第1次遍历的最大值区域可能有多个，因此，第2次搜索也需要对1次最大值的位置进行判断。
 
 
* 为矩形区域设置padding
遍历九宫格区域时，采用九宫格的最中间的空格作为标志。为了边界区域也能使用此方法进行遍历，可以对区域设置一个padding，如下图所示。此时，创建数组为
`vector<vector<int>> arr(N+2,vector<int>(M+2))`。

![wangyi-1.PNG](http://ol3kbaay9.bkt.clouddn.com/wangyi-1.PNG)
 
* 第2次搜索时的易错点
 
 ![wangyi-2.PNG](http://ol3kbaay9.bkt.clouddn.com/wangyi-2.PNG)
 
 如上图所示，第1次扫描的最大结果是6，但是最大值位置有3个，若选择第1次清除最中间的6个蘑菇，则第2次扫描最大结果只有3。即局部最优解并不是全局最优解。第1次选择左侧的6个蘑菇，第2次选择右侧的6个蘑菇，则全局最优解为12。
 
针对上述描述的易错点，在第1次扫描时候除了保存最大值外，还应该保存最大值的位置。**第2次扫描的最大结果要建立在第1次扫描结果位置的基础上。**
 
### 求解 

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;


int cover9(vector<vector<int>> &mush, int row, int col) {
	int count = 0;
	for (int i = row - 1; i<row + 2; i++) {
		for (int j = col - 1; j<col + 2; j++) {
			if (mush[i][j] != 0) {
				count += 1;
			}
		}
	}
	return count;
}

vector<vector<int>> searchMax(vector<vector<int>> &mush, int N, int M) {
	vector<vector<int>> res(N+2, vector<int>(M+2, 0));
	int temp = 0;
	int maxCount = -1;
	for (int i = 1; i<=N; i++) {
		for (int j = 1; j<=M; j++) {
			temp = cover9(mush, i, j);
			if (temp > maxCount) {
				res.clear();
				res.resize(N + 2, vector<int>(M + 2,0));
				maxCount = temp;
				res[i][j] = maxCount;
			}
			if (temp == maxCount) {
				res[i][j] = maxCount;
			}
		}
	}
	return res;
}

int removeSearch(int N,int M,int row,int col,vector<vector<int>> mush) {
	for (int i = row - 1; i<row + 2; i++) {
		for (int j = col - 1; j<col + 2; j++) {
			mush[i][j] = (mush[i][j] == 0) ? 0 : mush[i][j] - 1;
		}
	}

	int temp = 0;
	int maxCount = -1;
	for (int i = 1; i <= N; i++) {
		for (int j = 1; j <= M; j++) {
			temp = cover9(mush, i, j);
			if (temp >=  maxCount) {
				maxCount = temp;
			}
		}
	}
	return maxCount;

}
int main() {
	int N, M, K;
	int tempX, tempY;
	while (cin >> N >> M >> K) {
		vector<vector<int>> mush(N+2, vector<int>(M+2, 0));
		for (int i = 0; i<K; i++) {
			cin >> tempX;
			cin >> tempY;
			mush[tempX][tempY] += 1;
		}
		int res1;
		int res2 = -1;
		vector<vector<int>> max1 = searchMax(mush, N, M);
		for (int i = 1; i <= N; i++) {
			for (int j = 1; j <= M; j++) {
				if (max1[i][j] != 0) {
					res1 = max1[i][j];
					//移除该区域的蘑菇,并进行第2次搜索
					res2 = max(res2,removeSearch(N,M,i, j, mush));
					
				}
			}
		}
		cout <<res1+res2 << endl;
	}
	return 0;
}
```
## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 