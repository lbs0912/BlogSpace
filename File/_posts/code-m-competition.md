---
title: CodeM 美团编程大赛-资格赛
date: 2017-06-24 20:35:48
categories: 算法
tags: [算法,Algorithm]
keywords: 算法
---


* 参考链接：[美团点评CodeM编程大赛|资格赛非官方题解](https://www.nowcoder.com/discuss/28562?type=0&order=0&pos=15&page=1)
* [官方推荐题解资料下载](https://www.nowcoder.com/live/121) | [题解资料备份](http://ojx8u3g1z.bkt.clouddn.com/codem_qulification.rar)
* [题目在线练习](https://www.nowcoder.com/test/5513596/summary)


<!--more-->

## 音乐研究
### 题目
* 时间限制：1秒
* 空间限制：32768K

美团外卖的品牌代言人袋鼠先生最近正在进行音乐研究。他有两段音频，每段音频是一个表示音高的序列。现在袋鼠先生想要在第二段音频中找出与第一段音频最相近的部分。

具体地说，就是在第二段音频中找到一个长度和第一段音频相等且是连续的子序列，使得它们的 difference 最小。两段等长音频的 difference 定义为：

```
difference = SUM(a[i] - b[i])2 (1 ≤ i ≤ n)
```
其中SUM()表示求和， n 表示序列长度，a[i], b[i]分别表示两段音频的音高。现在袋鼠先生想要知道，difference的最小值是多少？数据保证第一段音频的长度小于等于第二段音频的长度。

* 输入描述

第一行一个整数n(1 ≤ n ≤ 1000)，表示第一段音频的长度。
第二行n个整数表示第一段音频的音高（0 ≤ 音高 ≤ 1000）。
第三行一个整数m(1 ≤ n ≤ m ≤ 1000)，表示第二段音频的长度。
第四行m个整数表示第二段音频的音高（0 ≤ 音高 ≤ 1000）。


* 输出描述

输出difference的最小值

* 输入例子
2
1 2
4
3 1 2 4

* 输出例子
0

### 分析

### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

int main(){
    vector<int> f;
    vector<int> s;
    int n,m,temp;
    int sum = INT_MAX;
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>temp;
        f.push_back(temp);
    }
    cin>>m;
    for(int j=0;j<m;j++){
        cin>>temp;
        s.push_back(temp);
    }

    int tempSum;
    for(int i=0;i<m+1-n;i++){
        tempSum = 0;
        for(int j=0;j<n;j++){
            tempSum += (f[j]-s[i+j])*(f[j]-s[i+j]);
        }
        sum = min(sum,tempSum);
    }
    cout<<sum<<endl;
    return 0;
}
```



## 锦标赛
### 题目
* 时间限制：1秒
* 空间限制：32768K

组委会正在为美团点评CodeM大赛的决赛设计新赛制。

比赛有 n 个人参加（其中 n 为2的幂），每个参赛者根据资格赛和预赛、复赛的成绩，会有不同的积分。比赛采取锦标赛赛制，分轮次进行，设某一轮有 m 个人参加，那么参赛者会被分为 m/2 组，每组恰好 2 人，m/2 组的人分别厮杀。我们假定积分高的人肯定获胜，若积分一样，则随机产生获胜者。获胜者获得参加下一轮的资格，输的人被淘汰。重复这个过程，直至决出冠军。

现在请问，参赛者小美最多可以活到第几轮（初始为第0轮）？

* 输入描述

第一行一个整数 n (1≤n≤ 2^20)，表示参加比赛的总人数。

接下来 n 个数字（数字范围：-1000000…1000000），表示每个参赛者的积分。

小美是第一个参赛者。


* 输出描述

小美最多参赛的轮次。

* 输入例子
4
4 1 2 3

* 输出例子
2
### 分析
* 思路1

为了求小美参赛最多的场数，应该尽量让小美和分数低（或相同）的人比赛。因此，需要依据参赛分数进行排序，将参赛者分为2大阵营：低于小美的阵营`small`和大于等于小美的阵营`big`。

情况1： 若big=0，即小美分数最高，则直接返回small/2即可。否则，转至情况2。

情况2：小美分数排在中间时，若small为偶数（small和big的奇偶情况相同），则small阵营内两两对抗，big阵营内两两对抗，即`big /= 2, small /= 2`。若small为奇数（small和big的奇偶情况相同），则存在small阵营的一人和big阵营的一人对阵，结果一定为big阵营获胜，即`++big, --small`。

该情况2循环终止的条件是small阵营人数为0。

这里需要注意的是，题目问的是小美最多能活到第几轮（初始为第0轮）。若测试数据为4人，且分数为`2 1 3 4`，则小美可以参加第0轮，第1轮失败，则输出答案应该为0，而不是1。（失败的轮数不计算在内）


* 思路2

在思路1的基础上，求得分数小于小美的人数为small。比赛终止条件是small为0。每次进行一轮比赛，则small人数变为原来的一半。因此有`2^(比赛次数)=small`。

因此，本题结果为

```cpp
if(small == 0){
	cout<<0<<endl;
}
else{
	cout<<log(small)/log(2)<<endl;
}
```






### 求解

* 方法1：常规处理


```cpp
#include <iostream>
using namespace std;

typedef long long ll;
int main()
{
    ll n, m;  //n为参赛人数， m为小美的积分
    while (cin >> n >> m) {
        ll t;
        ll cnt = 0;
        if(n == 0 || n == 1){
            cout<< 0<<endl;  //只有0人或1人参赛
            return 0;
        }
        for (int i = 1; i < n; ++i)
        {
            cin >> t;
            if (t <= m){
                ++cnt;
            }
        }
        int small = cnt;  //积分小于小美的参赛人数
        int big = n - cnt; //积分小于小美的参赛人数
        //small 和 big 的奇偶性相同
        if (big == 0)
        {
            //小美为分数最高者，直接返回人数/2
            cout << small / 2 << endl;
            return 0;
        }
        cnt = 0;
        //比小美分数小的人数小于1时，循环终止
        while (big > 0 && small > 1)
        {
            //初试为第0轮 ！！
            //big为奇数 small 和 big 的奇偶性相同
            if (big%2){
                ++big;
                --small;
            }
            big /= 2;
            small /= 2;
            ++cnt;
        }
        cout << cnt << endl;
    }
    return 0;
}
```

* 方法2: log(small)/log(2)

```cpp
#include <iostream>
#include <cmath>
using namespace std;

typedef long long ll;
int main()
{
	ll n, m;  //n为参赛人数， m为小美的积分
	while (cin >> n >> m) {
		ll t;
		ll cnt = 0;
		if (n == 0 || n == 1) {
			cout << 0 << endl;  //只有0人或1人参赛
			return 0;
		}
		for (int i = 1; i < n; ++i)
		{
			cin >> t;
			if (t <= m) {
				++cnt;
			}
		}
		int small = cnt;  //积分小于小美的参赛人数
		int big = n - cnt; //积分小于小美的参赛人数
		//small 和 big 的奇偶性相同
		if (small == 0) {
			cout << 0 << endl;
		}
		else {
			cout << ll(log(small) / log(2)) << endl;
		}

	}
	return 0;
}
```





## 数码
### 题目
* 时间限制：1秒
* 空间限制：32768K


给定两个整数 l 和 r ，对于所有满足1 ≤ l ≤ x ≤ r ≤ 10^9 的 x ，把 x 的所有约数全部写下来。对于每个写下来的数，只保留最高位的那个数码。求1～9每个数码出现的次数。

* 输入描述
一行，两个整数 l 和 r (1 ≤ l ≤ r ≤ 10^9)。

* 输出描述

输出9行。

第 i 行，输出数码 i 出现的次数。

输入例子:
1 4

* 输出例子

4
2
1
1
0
0
0
0
0
### 分析

**本题难点在于算法的复杂度和运行时间的限制。**

 * 思路1

常规求解。很遗憾，系统会判断运行超时。您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。



* 思路2

 ![code-m-pro-5.JPG](http://ojx8u3g1z.bkt.clouddn.com/code-m-pro-5.JPG)

考虑 `[left,right] = [1,right] - [1,left-1]`，将问题转化为`[1,right]`和`[1,left-1]`两个子区间问题。下面只要求解`[1,n]`区间问题即可。

对于一个数d，它是d，2d，3d， ... 的约数，也就是区间[1, n]内约数d出现的次数为`n/d`（向下取整）。

考虑枚举约数`d`的数码`p`（即最高位的值）和数字位数`q`，那么有d属于`[p*10^(q-1),(p+1)*10^(q-1))`。例如，最高位为1的约数d出现在[1,2)，[10,20)，[100,200) 等区间；最高位为4的约数d出现在[4,5)，[40,50)，[400,500) 等区间。

> 这样划分区间的好处是区间中值的最高位都一样，方便统计。不用再去求解约数的最高位。
>

由于d最大是10^9，因此，通过上式计算得，q最大取值为10。

 参照上图，计算约数为p的个数为

```cpp
//枚举d的长度为q，q取值1~10，将10^(q-1)值保存到pow中
vector<ll> m_pow = { 1,10,100,1000,10000,100000,1000000,10000000,100000000,1000000000 };

for (int p = 1; p <= 9; p++) {   //p为枚举d的数码 1~9
	for (int q = 1; q <= 10; q++) {  //j表示枚举d的长度q  1~10
		res[p] += (getSum(num, (p + 1) * m_pow[q - 1] - 1) - getSum(num, p * m_pow[q - 1] - 1));
	}
}
```

对上述算法再进行优化，限制区间小于等于num。

```cpp
for (int p = 1; p <= 9; p++) {   //p为枚举d的数码 1~9
	for (int q = 1; q <= 10; q++) {  //j表示枚举d的长度q  1~10
		if ((p * m_pow[q - 1] - 1) > num) {
			break;
		}
		else {
			res[p] += (getSum(num, min(num,(p + 1) * m_pow[q - 1] - 1)) - getSum(num, p * m_pow[q - 1] - 1));
		}
	}
}
```

下面分析如何计算`Sum(n,m)`。**如果直接计算，则复杂度为O(n)。牛客网系统仍然会判断超时。应该对算法进行优化处理，这也是本题的难点。**

 ![code-m-pro-5-2.JPG](http://ojx8u3g1z.bkt.clouddn.com/code-m-pro-5-2.JPG)

对应的代码实现为

```cpp
for (ll k = n / t; k >= n / m; k--) {
	cnt = min(m, n / k) - n / (k + 1);
	res += k*cnt;
}
```




### 求解

* 方法1：常规解法。（时间超时，未AC）

采用常规方法求解，会显示运行超时：您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。



```cpp
#include <iostream>
#include <vector>
using namespace std;


int getHighBit(int num) {
	while (num>=10) {
		num = num / 10;
	}
	return num;
}

void getFactors(int num,vector<int>& res) {
	int temp;
	for (int i = 1; (i*i) <= num; i++) {
		if (((int)(num / i) * i) == num) {
			//可以整除
			temp = num / i;
			if (temp != i) {
				res[getHighBit(i)]++;
				res[getHighBit(temp)]++;
			}
			else {
				res[getHighBit(i)]++;
			}
		}
	}
}

int main() {
	int left, right;
	while (cin >> left >> right) {
		vector<int> res(10, 0);
		for (int i = left; i <= right; i++) {
			getFactors(i, res);
		}
		for (int i = 1; i <= 9; i++) {
			cout << res[i] << endl;
		}
	}

	system("pause");

	return 0;
}
```


* 方法2：AC代码


```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
using namespace std;
typedef long long ll;

//枚举d的长度为q，q取值1~10，将10^(q-1)值保存到pow中
vector<ll> m_pow = { 1,10,100,1000,10000,100000,1000000,10000000,100000000,1000000000 };

ll getSum(ll n,ll m) {
	if (n == 0 || m == 0) {
		return 0;
	}
	m = min(n, m);
	ll res = 0;
	ll t = 0;
	ll cnt;
	for (t = 1; t <= m && (t*t) <= n; t++) {
		res += n / t;
	}
	for (ll k = n / t; k >= n / m; k--) {
		cnt = min(m, n / k) - n / (k + 1);
		res += k*cnt;
	}
	return res;
}

//计算[1,num]内约数情况
void getFactors(ll num, vector<ll>& res) {
	for (int p = 1; p <= 9; p++) {   //p为枚举d的数码 1~9
		for (int q = 1; q <= 10; q++) {  //j表示枚举d的长度q  1~10
			if ((p * m_pow[q - 1] - 1) > num) {
				break;
			}
			else {
				res[p] += (getSum(num, min(num,(p + 1) * m_pow[q - 1] - 1)) - getSum(num, p * m_pow[q - 1] - 1));
			}

		}
	}
}
int main(){
	ll left, right;


	while (cin >> left >> right) {

		vector<ll> res1(10);  //用于记录结果
		vector<ll> res2(10);  //用于记录结果

		getFactors(left-1,res1);
		getFactors(right, res2);
		for (int i = 1; i <= 9; i++) {
			cout << res2[i] - res1[i] << endl;
		}

	}
	system("pause");
	return 0;
}
```



## 优惠券
### 题目
* 时间限制：1秒
* 空间限制：32768K

美团点评上有很多餐馆优惠券，用户可以在美团点评App上购买。每种优惠券有一个唯一的正整数编号。每个人可以拥有多张优惠券，但每种优惠券只能同时拥有至多一张。每种优惠券可以在使用之后继续购买。

当用户在相应餐馆就餐时，可以在餐馆使用优惠券进行消费。某人优惠券的购买和使用按照时间顺序逐行记录在一个日志文件中，运营人员会定期抽查日志文件看业务是否正确。业务正确的定义为：一个优惠券必须先被购买，然后才能被使用。

某次抽查时，发现有硬盘故障，历史日志中有部分行损坏，这些行的存在是已知的，但是行的内容读不出来。假设损坏的行可以是任意的优惠券的购买或者使用。

现在给一个日志文件，问这个日志文件是否正确。若有错，输出最早出现错误的那一行，即求出最大s，使得记录1到s-1满足要求；若没有错误，输出-1。

* 输入描述

输入包含多组数据

m 分别表示 m (0 ≤ m ≤ 5 * 10^5) 条记录。

下面有m行，格式为：

I x （I为Input的缩写，表示购买优惠券x）；

O x（O为Output的缩写，表示使用优惠券x）；

? （表示这条记录不知道）。

这里x为正整数，且x ≤ 10^5 。


* 输出描述

-1 或 x(1 ≤ x ≤ m) 其中x为使得1到x-1这些记录合法的最大行号。

* 输入例子
0
1
O 1
2
?
O 1
3
I 1
?
O 1
2
I 2
O 1

* 输出例子
-1
1
-1
-1
2
### 分析
set和map操作中
* `lower_bound(val)` :  返回容器中第一个值大于或等于val的元素的iterator位置。
* `upper_bound(val)` :  返回容器中第一个值大于val的元素的iterator位置。

**该题考虑的情况比较多，可以当作经典例题分析和学习。**

应该避免这种误区——将`?`当作是万能的。例如，如下测试用例中，正确结果是输出4。如果把`?`当作是万能的，则是不对的。

```
4
?
?
O 1
O 1
```

下面，对本题思路进行分析。

1. 情况1：买入优惠券
* 如果没有该优惠券买入记录，则直接买入
* 如果已经有买入该优惠券的记录，则寻找上次买入优惠券操作之后最近的一次`?`操作，将其当作卖出操作，并从日志中删去该`?`记录。

2. 情况2：使用优惠券
* 如果有该优惠券买入记录，则直接使用
* 如果已经有卖出该优惠券的记录，则寻找上次卖出优惠券操作之后最近的一次`?`操作，将其当作买入操作，并从日志中删去该`?`记录。


针对上述思路分析，引入`set`容器，用于记录`?`操作的行数。引入`map`容器，用于记录买入和卖出操作。**为了使用一个容器，同时记录买入和卖出两种操作，规定对于买入操作，存储该记录行数（大于0）； 对于卖出操作，存储该记录行数的相反数（小于0）。**





```cpp
if (oper == "I") {
	if (numData[num] > 0) {
		//已经存在购买优惠券的操作
		//do something here
	}
	numData[num] = i; //买入操作存储行号
	//语句每次都要执行  ！！！
}

//如下书写是错误的
if (oper == "I") {
	if (numData[num] > 0) {
		//已经存在购买优惠券的操作
		//do something here
	}
	else{  	// Wrong  ！！！
		numData[num] = i; //买入操作存储行号
	}
}
```

注意，`numData[num] = i`每次都应该执行。因为记录的是**最新的**买入和卖出操作。该语句并不是和上述if语句构成`if...else...`关系。








### 求解

```
#include <iostream>
#include <vector>
#include <set>
#include <string>
#include <map>
using namespace std;

typedef long long ll;

int main() {
	ll length;  //操作次数
	string oper; //操作符
	ll num; //优惠券编号
	ll has_error = -1;  //是否有错

	while (cin >> length) {
		if (length == 0) {
			cout << -1 << endl;
			continue;
		}
		has_error = -1;
		map<ll, ll> numData;  //存储优惠券信息
		set<ll> unknown; //存储?号操作
		for (ll i = 1; i <= length; i++) {
			if (has_error != -1) {
				continue;   //已经确定了出错行
			}
			//i为输入的行号 从1开始
			cin >> oper;
			if (oper == "?") {
				unknown.insert(i);
				continue;
			}
			cin >> num;
			if (oper == "I") {
				if (numData[num] > 0) {
					//已经存在购买优惠券的操作
					set<ll>::iterator it = unknown.lower_bound(numData[num]);
					//查找距离上次购买最近的操作
					if (it == unknown.end()) {
						has_error = i;  //记录出错行
					}
					else {
						unknown.erase(it);  //清除该?操作
					}
				}
				numData[num] = i; //买入操作存储行号

			}
			else if (oper == "O") {
				if (numData[num] < 0) {
					//已经存在卖出优惠券的操作
					set<ll>::iterator it = unknown.lower_bound(-numData[num]);
					//查找距离上次购买最近的操作
					if (it == unknown.end()) {
						has_error = i;  //记录出错行
					}
					else {
						unknown.erase(it);  //清除该?操作
					}
				}
				numData[num] = -i; //卖出操作存储行号的相反数
			}
		}
		cout << has_error << endl;

	}
	return 0;
}
```

















## 送外卖
### 题目
* 时间限制：1秒
* 空间限制：32768K

n 个小区排成一列，编号为从 0 到 n-1 。一开始，美团外卖员在第0号小区，目标为位于第 n-1 个小区的配送站。
给定两个整数数列 a[0]~a[n-1] 和 b[0]~b[n-1] ，在每个小区 i 里你有两种选择：
1) 选择a：向前 a[i] 个小区。
2) 选择b：向前 b[i] 个小区。

把每步的选择写成一个关于字符 ‘a’ 和 ‘b’ 的字符串。求到达小区n-1的方案中，字典序最小的字符串。如果做出某个选择时，你跳出了这n个小区的范围，则这个选择不合法。
* 当没有合法的选择序列时，输出 “No solution!”。
* 当字典序最小的字符串无限长时，输出 “Infinity!”。
*  否则，输出这个选择字符串。

字典序定义如下：串s和串t，如果串 s 字典序比串 t 小，则
* 存在整数 i ≥ -1，使得∀j，0 ≤ j ≤ i，满足s[j] = t[j] 且 s[i+1] < t[i+1]。
* 其中，空字符 < ‘a’ < ‘b’。


----------


* 输入描述

输入有 3 行。

第一行输入一个整数 n (1 ≤ n ≤ 10^5)。

第二行输入 n 个整数，分别表示 a[i] 。

第三行输入 n 个整数，分别表示 b[i] 。

−n ≤ a[i], b[i] ≤ n


* 输出描述
输出一行字符串表示答案。

* 输入例子
7
5 -3 6 5 -5 -1 6
-6 1 4 -2 0 -2 0

* 输出例子
abbbb
### 分析

创建二维数组`vector<evector<int>> oneStep`，用于存储向前回溯一步的信息。例如，`a[5] = -1`，则从第5个点可以到达第5个点，则存储`oneStep[4][0] = 5`。同时，`b[4] = 0`，则从第4个点可以到达第4个点，则存储`oneStep[4][1] = 4`。

**判断是否存在从第0个点到第n-1个点的路径，只需从第n-1个点回溯DFS，看是否可以达到第0个点即可**。（无向图，运动方向不影响结果）。如果不能到达第0个点，则无解 No Solution。将该回溯DFS记作第1轮回溯DFS，并标记哪些点被访问到了。**只有被访问到的点，才有可能出现在字典排序最小的路径上，没有被访问到的点，在第2轮遍历中可以直接排除。**

如何记录字典排序最小的路径？ 此处采用贪婪算法
* 从第0个点出发，每次优先选择使用数组a的信息。如果下一个点在第1轮DFS中被访问（即该点在路径备选信息中），并且第2轮没有被访问到，则记录该点信息，输出结果字符串上添加a信息。如果该点在第1轮被访问，在第2轮中也已经被访问，则表示出现了环路，输出Infinity。
* 如果选用数组a的方案，获取的下一个点不再路径备选方案中（即第1轮没有被访问），则考虑数组b方案。处理方法同上。
* 如果数组b方案也不符合要求，则表示无解 No Solution。


下面解释下为什么会出现环路？


 ![code-m-pro5-1.JPG](http://ojx8u3g1z.bkt.clouddn.com/code-m-pro5-1.JPG)

要求的路径是字典排序最小，而不是长度最小。上图中，字典排序最小的是aaaa...aaaa，而不是aab




### 求解

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <string>
using namespace std;

void getInput(vector<int>& datas, vector<vector<int>>& oneStep, int n) {
	int temp;
	int sourceStep;

	for (int i = 0; i<n; i++) {
		cin >> temp;
		datas.push_back(temp);
		//存储向前回溯一步的信息
		sourceStep = i + temp;
		if ((sourceStep >= 0) && (sourceStep <= n - 1)) {
			oneStep[sourceStep].push_back(i);
		}
	}
}

void reverseDFS(int n, vector<vector<int>> oneStep, vector<bool>& reverseVis) {
	queue<int> m_queue;
	int temp;
	reverseVis[n - 1] = true;
	m_queue.push(n - 1);
	while (!m_queue.empty()) {
		temp = m_queue.front();
		m_queue.pop();
		for (int i = 0; i<oneStep[temp].size(); i++) {
			if (!reverseVis[oneStep[temp][i]]) {
				//该点未访问
				reverseVis[oneStep[temp][i]] = true;
				m_queue.push(oneStep[temp][i]);
			}
		}
	}
}

int main() {
	int n;
	vector<int> a, b;
	vector<vector<int>> oneStep;  //存储向前回溯一步的信息
	vector<bool> reverseVis; //第1轮 逆向 DFS
	vector<bool> Vis;   //第2轮  正向DFS
	string str;

	while (cin >> n) {
		a.clear();
		b.clear();
		oneStep.clear();
		oneStep.resize(n);
		reverseVis.clear();
		reverseVis.resize(n, false);
		Vis.clear();
		Vis.resize(n, false);
		str = "";

		getInput(a, oneStep, n);
		getInput(b, oneStep, n);

		//从n-1点回溯到第0点
		reverseDFS(n, oneStep, reverseVis);
		if (!reverseVis[0]) {
			//第0点未被访问
			cout << "No solution!" << endl;
			continue;
		}

		//从第0点正向DFS至第n-1点
		Vis[0] = true;
		int nxt;   //下一个点
		bool infFlag = false;   //是否出现环路
		bool isNoSolution = false;  //是否出现无解
		for (int i = 0; i<(n-1) && !infFlag && !isNoSolution;) {
			//优先考虑a情况  字典排序最小
			nxt = i + a[i];
			if ((nxt >= 0) && (nxt <= n - 1) && reverseVis[nxt]) {
				//该点在第1轮逆向DFS的路径上
				if (!Vis[nxt]) {
					Vis[nxt] = true;
					str += "a";
				}
				else {
					//该点已经被访问，出现环路
					infFlag = true;
					continue;
				}
				i = nxt;
			}
			else {
				//该点在第1轮逆向DFS的路径 考虑b情况
				nxt = i + b[i];
				if ((nxt >= 0) && (nxt <= n - 1) && reverseVis[nxt]) {
					//该点在第1轮逆向DFS的路径上
					if (!Vis[nxt]) {
						Vis[nxt] = true;
						str += "b";
					}
					else {
						//无解
						isNoSolution = true;
						continue;
					}
				}
				i = nxt;
			}

		}

		if (isNoSolution) {
			cout << "No solution!" << endl;
		}
		else if (infFlag) {
			cout << "Infinity!" << endl;
		}
		else {
			cout << str << endl;
		}

	}
	return 0;
}
```



##  围棋
### 题目
* 时间限制：2秒
* 空间限制：32768K


围棋是起源于中国有悠久历史的策略性棋类游戏。它的规则如下：
1. 棋盘19*19。
2. 棋子分黑白两色，双方各执一色。
3. 下法：每次黑或白着一子于棋盘的空点上。棋子下定后，不再向其他点移动。
4. 棋子的气：一个棋子在棋盘上，与它相邻的空点是这个棋子的“气”（这里相邻是指两个点有公共边）。 相邻的点上如果有同色棋子存在，这些棋子就相互连接成一个不可分割的整体，气合并计算。
相邻的点上如果有异色棋子存在，此处的气便不存在。
如果棋子所在的连通块失去所有的气，即为无气之子，不能在棋盘上存在。
5. 提子：把无气之子清理出棋盘的手段叫“提子”。提子有二种：
1) 着子后，对方棋子无气，应立即提取对方无气之子。
2) 着子后，双方棋子都呈无气状态，应立即提取对方无气之子。
6. 禁着点：棋盘上的任何一空点，如果某方在此下子，会使该子立即呈无气状态，同时又不能提取对方的棋子，这个点叫做“禁着点”，该方不能在此下子。
7. 禁止全局同形：无论哪一方，在成功进行了着子、提子操作后，棋盘局面不能和任何之前的局面相同。

你要做的是：输入一些操作，从空棋盘开始模拟这些操作。
对于每一步，若结果不正确，则输出对应的miss并且忽略这个操作，并在最后输出棋盘的局面。

* 输入描述

第一行，测试数据组数≤100

第二行，每组测试数据，执行的步数 n ≤ 2000

然后 n 行

B x y

W x y

(1 ≤ x ≤ 19,1 ≤ y ≤ 19)
其中，二元组 x,y 表示围棋棋盘上第 x 行第 y 列对应的点。
输入数据保证是黑白轮流下的。


* 输出描述


多行
对于miss的情况，输出是哪一种错误格式，其中：
miss 1 表示下的位置已经有棋了
miss 2 表示违反规则6
miss 3 表示违反规则7
对于正常的操作，不用输出。
最后输出最终盘面。“B表示黑子，W表示白子，如果是空点的话，就输出'.'字符。”

* 输入例子

1
12
B 1 3
W 1 2
B 2 4
W 2 1
B 1 1
W 2 3
B 3 3
W 3 2
B 1 1
W 2 3
B 2 2
W 2 3

对应的棋形是这样的

![nowcode-codeM-5.png](http://ol3kbaay9.bkt.clouddn.com/nowcode-codeM-5.png)


* 输出例子

miss 2
miss 2
miss 1
miss 3
.WB................
WB.B...............
.WB................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................
...................

### 分析
根据题意，对情况进行模拟即可。

算法中注意时间复杂度，否则会运行超时。算法优化注意
* 利用多项式哈希，进行棋盘信息的比较。

若直接比较两种棋盘信息，需要比较19*19种情况。此处，利用多项式哈希进行信息比较，对算法进行优化。

```cpp
ull has[2]={0,0};
for(int i=1;i<=19;i++)
	for(int j=1;j<=19;j++)
    {
		//利用哈希判读当前局面是否出现  miss 3
        //多项式哈希
        has[0]=has[0]*131+nxt[i][j];
        has[1]=has[1]*137+nxt[i][j];
    }
    //....
}
```

* 棋盘信息输出

如果直接输出，时间复杂度是O(n^2)。

```cpp
for(int i=1;i<=19;i++){
	for(int j=1;j<=19;j++){
		cout<<cur[i][j];
	}
	cout<<endl;
}
```

采用如下算法，进行优化，时间复杂度为O(n)。

```cpp
for(int i=1;i<=19;i++){
	cout<<cur[i]+1<<endl;
}
```

> When you call `cur[0][0]` it's the same as calling `*(*(cur + 0) + 0)` and so calling `cur[0] + 1` is the same as `*(cur + 0) + 1` which returns a pointer to the second element of the first subarray.
>
> When you give a char pointer to cout, it will print each char it finds until it hits a `0`, then it stops.
>



For example,
```cpp
char cur[3][3] = {{'.','.','B'},{'B','W','\0'},{'B','.','.'}};
for(int x=0;x<3;x++){
    cout<<cur[x]+1<<endl;
}

//Output
.BBW
W
..
```

参考[StackOverflow](https://stackoverflow.com/questions/44689263/output-elments-of-a-two-dimension-array?answertab=active#tab-top)了解更多。
### 求解

```cpp
#include <iostream>
#include <cstring>
#include <cmath>
#include <algorithm>
#include <set>
using namespace std;
typedef unsigned long long ull;

const int dir[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
char cur[25][25],nxt[25][25]; //棋盘当前信息和信息备份
set<pair<ull,ull>> st; //多项式哈希存储当前棋盘信息

inline void init()
{
    st.clear();
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
            cur[i][j]='.';
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
            nxt[i][j]='.';
}

//copy  将当前棋盘情况复制到nxt棋盘情况
inline void cop()
{
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
            nxt[i][j]=cur[i][j];
}


//pass  将nxt值传递到cur
inline void pas()
{
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
            cur[i][j]=nxt[i][j];
}

//检查棋盘信息和之前是否相等
inline bool check()
{
    ull has[2]={0,0};
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
        {
			//利用哈希判读当前局面是否出现  miss 3
            //多项式哈希
            has[0]=has[0]*131+nxt[i][j];
            has[1]=has[1]*137+nxt[i][j];
        }
    if(st.find(make_pair(has[0],has[1]))==st.end())
    {
        st.insert(make_pair(has[0],has[1]));
        return 1;
    }
    else return 0;
}

//打印输出棋盘信息
inline void show()
{
    for(int i=1;i<=19;i++)
        printf("%s\n",cur[i]+1);
}
int vis[25][25];

//标记节点均未被访问
inline void fre()
{
    for(int i=1;i<=19;i++)
        for(int j=1;j<=19;j++)
            vis[i][j]=0;
}

//dfs 搜索节点的四周 寻找有无相邻的空点 并找出气的范围
bool dfs(int x,int y,char g)
{
    if(nxt[x][y]=='.')return 1;
    if(nxt[x][y]!=g)return 0;
    vis[x][y]=1;
    bool isok=0;
    for(int i=0;i<4;i++)
    {
        int tx=x+dir[i][0],ty=y+dir[i][1];
        if(!vis[tx][ty])
            isok|=dfs(tx,ty,g);
    }
    return isok;
}
void pick(int x,int y,char g)
{
    nxt[x][y]='.';
    for(int i=0;i<4;i++)
    {
        int tx=x+dir[i][0],ty=y+dir[i][1];
        if(nxt[tx][ty]==g)pick(tx,ty,g);
    }
}
int main()
{
    int T; //测试数据组数
    scanf("%d",&T);
    while(T--)
    {
        int n;  // 每组测试数据的数目
        scanf("%d",&n);
        init();
        check();
        for(int i=1;i<=n;i++)
        {
            char go[5];
            int x,y;
            scanf("%s%d%d",go,&x,&y);
            char a=go[0],b="BW"[a=='B'];
            //cur用于存储当前棋盘
            // 如果当前不为空  miss 1
            if(cur[x][y]!='.'){
            	printf("miss 1\n");
            }
            else
            {
                cop(); //复制棋盘信息
                nxt[x][y]=a;  //保存输入的信息
                fre();  //标记节点未被访问
                bool isok=dfs(x,y,a);

                for(int i=0;i<4;i++)
                {
                    int tx=x+dir[i][0],ty=y+dir[i][1];
					//如果节点没有被访问 并且是不同的颜色  并且周围没有空点
					//即 对方棋子无气，应立即提取对方无气之子。
                    if(!vis[tx][ty] && nxt[tx][ty]==b && !dfs(tx,ty,b))
                    {
                        pick(tx,ty,b);
                        isok=1;  //标记本点周围有气
                    }
                }
                if(isok)
                {
                    if(check())pas();
                    else printf("miss 3\n");
                }
                else printf("miss 2\n");
            }
        }
        show();
    }
    return 0;
}
```



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile)