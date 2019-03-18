---
title: 网易2017秋招编程题集合
date: 2017-04-01 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,网易笔试,动态规划]
keywords: 校招笔试编程题,网易笔试,动态规划
---



* 本文主要对网易2017秋招编程题集合进行记录。
*  注意本文中列举题目的动态规划考题。
* 在线联系本文中的试题：[牛客网](https://www.nowcoder.com/test/2811407/summary)

<!--more-->


## 回文序列
### 题目
如果一个数字序列逆置之后跟原序列是一样的就称这样的数字序列为回文序列。例如

{1, 2, 1}, {15, 78, 78, 15} , {112} 是回文序列, 
{1, 2, 2}, {15, 78, 87, 51} ,{112, 2, 11} 不是回文序列。

现在给出一个数字序列，允许使用一种转换操作：选择任意两个相邻的数，然后从序列移除这两个数，并用这两个数字的和插入到这两个数之前的位置(只插入一个和)。
现在对于所给序列要求出最少需要多少次操作可以将其变成回文序列。

* 输入描述
输入为两行，第一行为序列长度n ( 1 ≤ n ≤ 50)
第二行为序列中的n个整数item[i]  (1 ≤ iteam[i] ≤ 1000)，以空格分隔。


* 输出描述
输出一个数，表示最少需要的转换次数

* 输入例子
4
1 1 1 3

* 输出例子
2

### 分析
使用双端队列`deque`数据结构进行求解。双端队列`deque`数据结构支持高效地首尾两端元素的插入和删除。

参考资料
* [deque - CPlusPlus.com](http://www.cplusplus.com/reference/deque/deque/)
* [deque  | CSDN](http://blog.csdn.net/xiajun07061225/article/details/7442816)

`deque`常用的操作包括
* `#include <deque>`：需要包含的头文件
* pop_back:  弹出队首元素
* pop_front: 弹出队尾元素
* push_back: 队尾插入元素
* push_front: 队首插入元素
* front():  获取队首元素
* back():  获取队尾元素
* size():  队列大小
* clear():  清空队列


本题思路为：判断队首和队尾元素。若二者相等，则将这两个元素都弹出队列，将队列规模缩小2个，再对该问题进行判断；若二者不相等，则选择其中较小的一个，将该元素和与其相邻的元素都弹出队列，再将其和插入队列，从而将队列规模缩小1个，再对该问题进行判断。

### 求解
 
```cpp
#include <iostream>
#include <deque>
using  namespace std;

int main() {
	int length;
	deque<int> datas;
	int count = 0;
	int temp;
	int start;
	int end;
	int add;
	while (cin >> length) {
		count = 0;
		datas.clear(); 

		for (int i = 0; i<length; i++) {
			cin >> temp;
			datas.push_back(temp);
		}

		while (datas.size() > 1) {
			
			start = datas.front();
			end = datas.back();
			if (start == end) {
				//若相等，删除队首和队末元素
				datas.pop_back();
				datas.pop_front();
			}
			else {
				//不相等
				add = 0;
				count++;
				if (start <= end) {
					add += start;
					datas.pop_front();
					add += datas.front();
					datas.pop_front();
					datas.push_front(add);
				}
				else {
					add += end;
					datas.pop_back();
					add += datas.back();
					datas.pop_back();
					datas.push_back(add);
				}
				
			}
		}
		cout << count << endl;
	}
	return 0;
}
```


## 优雅的点
小易有一个圆心在坐标原点的圆，小易知道圆的半径的平方。小易认为在圆上的点而且横纵坐标都是整数的点是优雅的，小易现在想寻找一个算法计算出优雅的点的个数，请你来帮帮他。

例如，半径的平方如果为25，
优雅的点就有：`(+/-3, +/-4), (+/-4, +/-3), (0, +/-5) (+/-5, 0)`，一共12个点。 

* 输入描述
输入为一个整数，即为圆半径的平方,范围在32位int范围内。


* 输出描述
输出为一个整数，即为优雅的点的个数

* 输入例子
25

* 输出例子
12

### 分析
直接求解即可。注意程序不要运行超时。求解部分给出了使用两个for循环造成运行超时的程序。可以和正确答案对比，理解超时原因。（牛客网上程序运行大于1s，会算作超时）

### 求解
```cpp
//运行时间 < 1ms
#include <iostream>
#include <cmath>
using namespace std;

int main() {
	int radSq;
	int count;
	while (cin >> radSq) {
		count = 0;
        int rad = (int)sqrt(radSq);
		for (int i = 0; i <= rad; i++) {
			double j = sqrt(radSq - i*i);
            if((int)j >= j){
            	count += (i==0 || (int)j == 0) ? 2:4;    
            }
		}
		cout << count << endl;
	}
	return 0;
}
```

注意，可以虽然计算`sqrt()`会带来较大的计算量，但是相比于下述程序中的两个for循环，计算`sqrt()`带来的计算了更小。下述程序中为了避免计算`sqrt()`，使用了两个for循环，提交时候会显示运行超时（运行时间1001ms，大于1s）。

```cpp
/*！！两重for循环，提交时候显示运行超时！！*/
//运行时间  1001ms  超时 
#include <iostream>
using namespace std;

int main() {
	int radiusSquare;
	int count;
	while (cin >> radiusSquare) {
		count = 0;
		for (int i = 0; (i*i) <= radiusSquare; i++) {
			for (int j = i; (j*j) <= radiusSquare; j++) {
				/*！！两重for循环，提交时候显示运行超时！！*/
				if ((i*i) + (j*j) == radiusSquare) {
					if (i == 0 || i == j) {
						count += 4;  //(0,+-R)  (+-R,0)  4个点
									 // (+-i,+-i)  4个点
					}
					else {
						count += 8; //(j>i) 每次情况对应8个点
					}

				}
			}
		}
		cout << count << endl;
	}
	return 0;
}
```


## 跳石板
### 题目
小易来到了一条石板路前，每块石板上从1挨着编号为：1、2、3.......
这条石板路要根据特殊的规则才能前进：对于小易当前所在的编号为K的 石板，小易单次只能往前跳K的一个约数(不含1和K)步，即跳到K+X(X为K的一个非1和本身的约数)的位置。 小易当前处在编号为N的石板，他想跳到编号恰好为M的石板去，小易想知道最少需要跳跃几次可以到达。

例如，N = 4，M = 24：
4->6->8->12->18->24

于是小易最少需要跳跃5次，就可以从4号石板跳到24号石板 

* 输入描述
输入为一行，有两个整数N，M，以空格隔开。
(4 ≤ N ≤ 100000)
(N ≤ M ≤ 100000)


* 输出描述
输出小易最少需要跳跃的步数,如果不能到达输出-1

* 输入例子
4 24

* 输出例子
5


### 分析
* 思路1：动态规划
采用动态规划思想求解。创建一个vector容器steps，`steps[i]`表示到达i号石板所需的最小步数。初始化为steps容器为INT_MAX。从序号N的石板开始逐个遍历，若`steps[i]`为INT_MAX，表示该点不可到达，直接开始下次循环。若`steps[i]`不为INT_MAX，表示该点可以到达，下面求解编号i的约数，进行动态规划。动态规划的转移方程为

```
steps[i+j] = min(steps[i]+1,steps[i+j])   //i为石板编号，j为i的约束
steps[0] = 0
```

* 思路2：贪婪算法
**贪婪算法并不一定能得到最优解，但是一个可行的，较好的解。**

该问题若采用贪婪算法求解，并不会得到最优解，只会得到一个可行的，较好的解。例如，下述程序中采用了贪婪算法求解。每次都选取最大的约数前进一步。若后续发生不可到达目标点，则进行**回溯**，取第2大的约数作为步进值。下述程序通过率为80%，并不能AC。例如，对于N=676, M=12948情况，贪婪算法求解为13步，而动态规划算法求解为10步。

> 贪婪算法并不一定能得到最优解，但是一个可行的，较好的解。例如，给定硬币coins=[1,2,10,25]，金额总数amounts=30，不限制每种币值的硬币数量，要求用所给硬币凑出所需金额，并且硬币数量最少。若采用贪婪算法求解，需要6枚（25+5*1）硬币。 若采用动态规划求解，所需3枚（10+10+10）硬币。  --- [贪婪算法](http://www.infocool.net/kb/Arithmetic/201612/251791.html)


```cpp
// 程序通过率为80%，并不能AC
//对于N=676, M=12948情况，贪婪算法求解为13步，而动态规划算法求解为10步。
// 贪婪算法并不一定能得到最优解，但是一个可行的，较好的解。
#include <iostream>
using namespace std;

int stepSearch(int N, int M) {
	if (N > M) {
		return -1;
	}
	if (N == M) {
		return 0;
	}
	int res = 0;
	for (int i = 2; i<N; i++) {
		if (i*(N / i) == N) {
			res++;
			if (stepSearch(N + N/i, M) != -1) {
				res += stepSearch(N + N/i, M);
				return res;
			}
			else {
				res--;
			}
		}
	}
	return -1;
}

int main() {
	int N, M;
	while (cin >> N >> M) {
		cout << stepSearch(N, M) << endl;
	}
	return 0;
}
```

### 求解

* 方法1：动态规划，参照思路1。

```cpp

#include <iostream>
#include <vector>
#include <climits>
#include <cmath>
#include <algorithm>

using namespace std;
int main(){
    int N,M;
    while(cin>>N>>M){
        vector<int> steps(M+1,INT_MAX);
        steps[N] = 0;
        for(int i=N;i<=M;i++){
            if(steps[i] == INT_MAX){
                continue;
            }
            for(int j=2;(j*j)<=i;j++){
                if(i%j == 0){
                    if(i+j <= M){
                        steps[i+j] = min(steps[i]+1,steps[i+j]);
                    }
                    if(i+(i/j) <= M){
                        steps[i+(i/j)] = min(steps[i]+1,steps[i+(i/j)]);
                    }
                }
            }
        }
        if(steps[M] == INT_MAX){
            steps[M] = -1;
        }
        cout<<steps[M]<<endl;
    }
    return 0;
}

```






## 暗黑的字符串

### 题目
一个只包含'A'、'B'和'C'的字符串，如果存在某一段长度为3的连续子串中恰好'A'、'B'和'C'各有一个，那么这个字符串就是纯净的，否则这个字符串就是暗黑的。

例如，BAACAACCBAAA 连续子串"CBA"中包含了'A','B','C'各一个，所以是纯净的字符串
AABBCCAABB 不存在一个长度为3的连续子串包含'A','B','C',所以是暗黑的字符串。

你的任务就是计算出长度为n的字符串(只包含'A'、'B'和'C')，有多少个是暗黑的字符串。 

* 输入描述
输入一个整数n，表示字符串长度(1 ≤ n ≤ 30)

* 输出描述
输出一个整数表示有多少个暗黑字符串

* 输入例子
2
3

* 输出例子
9
21


### 分析（动态规划）
参考[牛客网](https://www.nowcoder.com/test/question/done?tid=7633373&qid=46575#summary)的讨论分析。其中的讨论截图如下图所示。

![dp-wangyi-4.PNG](http://ol3kbaay9.bkt.clouddn.com/dp-wangyi-4.PNG)

用`f(n)`表示长度为n的字符串中暗黑字符串的个数。`f(n)`的个数和前面`n-1`长度的字符串有关，并且和前面`n-1`长度的字符串中的最后两个字母有关。
* 最后两个字母相同
用`S(n-1)`表示长度为n-1的并且最后两个字母相同的个数。若最后两个字母相同，则第n个位置有3种选择，则f(n)中对应的个数为`3*S(n-1)`。例如，最后两个字母相同，均为AA，则第n个位置填A，B，C均可，从而组成AAA，AAB，AAC。

* 最后两个字母不同
用`D(n-1)`表示长度为n-1的并且最后两个字母不同的个数。若最后两个字母不同，则第n个位置2种选择，则f(n)中对应的个数为`2*D(n-1)`。例如，最后两个字母不同，为AB，则第n个位置只能填A，B，从而组成ABA，ABB。

* 综上，有如下推导过程（转移方程）

```
f(n-1) = S(n-1) + D(n-1)
f(n) = 3*S(n-1) + 2*D(n-1) = 2f(n-1) + S(n-1)
```
转移方程中还有一个`S(n-1)`项没有消除。下面考虑如何消除`S(n-1)`项。

* 最后两个字母相同

用`S(n-1)`表示长度为n-1的并且最后两个字母相同的个数。若最后两个字母相同，则第n个位置有3种选择，则f(n)中对应的个数为`3*S(n-1)`。例如，最后两个字母相同，均为AA，则第n个位置填A，B，C均可，从而组成AAA，AAB，AAC。

组成的`3*S(n-1)`项中，有1/3项的最后两个字母仍然是相同的，即`S(n-1)`；有2/3项的最后两个字母是不同的，即`2*S(n-1)`。

* 最后两个字母不同

用`D(n-1)`表示长度为n-1的并且最后两个字母不同的个数。若最后两个字母不同，则第n个位置2种选择，则f(n)中对应的个数为`2*D(n-1)`。例如，最后两个字母不同，为AB，则第n个位置只能填A，B，从而组成ABA，ABB。

组成的`2*D(n-1)`项中，有1/2项的最后两个字母仍然是相同的，即`D(n-1)`；有1/2项的最后两个字母是不同的，即`D(n-1)`。

* 综上，有如下推导

```
S(n) = S(n-1) + D(n-1)
D(n) = 2*S(n-1) + D(n-1)
f(n) = S(n) + D(n)

结合第1个式子和第3个式子，有S(n) = f(n-1)

将其代入转移方程有
f(n-1) = S(n-1) + D(n-1)
f(n) = 3*S(n-1) + 2*D(n-1) = 2*f(n-1) + S(n-1) = 2*f(n-1)+f(n-2)

初始条件： f(1) = 3    f(2) = 9

```

至此，动态规划中的转移方程和初试条件求解完毕。只需编写代码实现该算法即可。




### 求解

```cpp
#include <iostream>
#include <vector>
using namespace std;

typedef long long ll;

int main(){
    int n;
    vector<ll> counts;
    while(cin>>n){
		counts.clear();
        counts.resize(n+1,0);
        counts[1] = 3;
        counts[2] = 9;
        for(int i=3;i<=n;i++){
            counts[i] = 2*counts[i-1]+counts[i-2];
        }
        cout<<counts[n]<<endl;
    }
    return 0;
}
```





## 数字翻转
### 题目
对于一个整数X，定义操作rev(X)为将X按数位翻转过来，并且去除掉前导0。例如
如果 X = 123，则rev(X) = 321;
如果 X = 100，则rev(X) = 1.
现在给出整数x和y,要求rev(rev(x) + rev(y))为多少？ 
输入描述:
输入为一行，x、y(1 ≤ x、y ≤ 1000)，以空格隔开。


* 输出描述
输出rev(rev(x) + rev(y))的值

* 输入例子
123 100

* 输出例子
223


### 分析
题目比较简单，直接求解即可。

### 求解
* 方法1: 逐位取反。

该方法中不需要处理反转之后的前导0。因为0乘以权值仍然是0。
 
```
#include <iostream>
using namespace std;

int rev(int x) {
	int res = 0;
	while (x) {
		res = res * 10 + (x % 10);
		x = x / 10;
	}
	return res;
}

int main() {
	int x, y;
	while (cin >> x >> y) {
		cout << rev(rev(x) + rev(y)) << endl;
	}
	return 0;
}
```

* 方法2：字符串翻转

```cpp
#include <iostream>
#include <string>
#include <cstdlib>
#include <algorithm>
using namespace std;  

int rev(int x){    
    string s = to_string(x);    
    reverse(s.begin(),s.end());    
    return atoi(s.c_str());
}

int main(){    
    int x,y;    
    while(cin>>x>>y){
        cout << rev(rev(x) + rev(y)) << endl;
    }   
    return 0;
}
```

* `to_string()`
	- 将一个数字转化为string类型。数字可以是int，double，float，unsigned long等。 
	-  使用该函数需要包含`<string>`头文件。
	-  该函数在C++11中支持，低版本编译器可能不支持该函数。
	-  参考[std::to_string](http://www.cplusplus.com/reference/string/to_string/)。

* `atoi`
	- `int atoi (const char * str);`: Convert string to integer
	- 该函数将一个string字符串转化为一个数字。
	-  使用该函数需要包含`<cstdlib>`头文件。
	- 参考[atoi](http://www.cplusplus.com/reference/cstdlib/atoi/?kw=atoi)
	- `atof()`: Convert string to double (function )
	- `atol()`: Convert string to long integer (function )
	- `strtol()`: Convert string to long integer (function )
* `c_str()`
	- 生成一个`const char*`指针，指向以空字符终止的数组。
	- 使用该函数需要包含`<string>`头文件。
	- `c_str()`返回一个客户程序可读不可改的指向字符数组的指针，不需要手动释放或删除这个指针。
	- 参考[c_str()](http://www.cplusplus.com/reference/string/string/c_str/)

```cpp
const char* c;
string s="1234";
c = s.c_str(); 
cout<<c<<endl; //输出：1234
s="abcd";
cout<<c<<endl; //输出：abcd
```

需要注意的是，`atoi`函数接收的参数类型是`const char * str`，不能直接传入string类型的变量名，但是可以直接传入字符串。若要传入string类型的变量名，可以先借助`str.c_str()`函数进行转换。

```
atoi("321");   //321
string str = "321";
atoi(str);   //Error!!!
atoi(str.c_str());   //321
```

## 最大的奇约数
小易是一个数论爱好者，并且对于一个数的奇数约数十分感兴趣。一天小易遇到这样一个问题： 定义函数f(x)为x最大的奇数约数，x为正整数。 例如:f(44) = 11.
现在给出一个N，需要求出 f(1) + f(2) + f(3).......f(N)

例如，N = 7 
f(1) + f(2) + f(3) + f(4) + f(5) + f(6) + f(7) = 1 + 1 + 3 + 1 + 5 + 3 + 7 = 21

小易计算这个问题遇到了困难，需要你来设计一个算法帮助他。 

* 输入描述
输入一个整数N (1 ≤ N ≤ 1000000000)

* 输出描述
输出一个整数，即为f(1) + f(2) + f(3).......f(N)

* 输入例子
7

* 输出例子:
21

### 分析

* 思路1： 暴力求解（运行超时，未AC）
采用暴力求解。如下程序所示。很遗憾，程序提交时候显示运行超时，未AC。

```
#include <iostream>
using namespace std;

int maxOdd(int x){
    int res = 0;
    if(x%2 !=0){
        //x为奇数，则最大奇数约束是其本身
        return x;
    }
	int i;
    for(i=x-1;i>=1;i=i-2){
        if(x%i == 0){
            break;
        }
    }  
    return i;
}

int main(){
    int length;
    int res;
    while(cin>>length){
        res = 0;
        for(int i=1;i<=length;i++){
            res += maxOdd(i);
        }
        cout<<res<<endl;
    }
    return 0;
}
```



* 思路2： 奇数偶数分析

情况1：n为奇数
奇数的最大奇约束为其本身，即f(n)=n。

情况2：n为偶数
由于`偶数 = 偶数 * 偶数` 或者`偶数 = 偶数 * 偶数`。因此，采用`偶数/2`的方式可以求解一个偶数的最大奇约数。

```
int maxFactor;
while(n%2 == 0){
	n = n/2;
}
maxFacor = n;  //f(n) = maxFactr
```

由此，可以编写如下程序。但是很遗憾，该算法依旧超时，未AC。因此，还需对算法进行优化。

```cpp
//（运行超时，未AC）
#include <iostream>
using namespace std;

int maxOdd(int x){
    if(x%2 !=0){
        //x为奇数，则最大奇数约束是其本身
        return x;
    }
	while(x%2 == 0){
        x = x/2;
    }
    return x;
}

int main(){
    int length;
    int res;
    while(cin>>length){
        res = 0;
        for(int i=1;i<=length;i++){
            res += maxOdd(i);
        }
        cout<<res<<endl;
    }
    return 0;
}
```


* 思路3： 奇数序列分析
在思路2的基础上——**偶数的最大奇约束可以采用循环除2的方法求解**，对整个序列进行分析。参考[牛客网](https://www.nowcoder.com/questionTerminal/49cb3d0b28954deca7565b8db92c5296)了解该算法思想。

以n=10，n为偶数为例，进行说明。n为偶数时，其中有n/2个奇数。每次遍历，求解其中奇数项的和，采用等差数列求和公式有，奇数项的和为`((n)/2)*((n)/2)`。由于n为偶数，因此`((n)/2)*((n)/2) = ((n+1)/2)*((n+1)/2)`（为了和下文中n为奇数的情况求和公式进行统一）。此时，**序列中剩余的偶数项除以2之后，恰好是原序列的前n/2项**。如此，开始下一次循环即可。每次求解其中奇数项的和，偶数项除以2之后就是原序列的前n/2项。如此循环下去，直到序列长度为0。该过程如下所示。

```
1 2 3 4 5 6 7 8 9 10    n=10

sum(1 3 5 7 9)   rest = 2 4 6 8 10      sum = 25      
rest/2 = 1 2 3 4 5    （原序列的前n/2序列）   n=5

sum(1 3 5)       rest = 2 4         sum =25+9=34
rest/2 = 1 2     （原序列的前n/2序列）   n=2

sum(1)           rest = 2           sum =34+1=35
rest/2 = 1      （原序列的前n/2序列）   n=1

sum(1)           rest = 0           sum =35+1=36
rest/2 = 0      （原序列的前n/2序列）   n=0
```

若n为奇数，其中有(n+1)/2个奇数。每次遍历，求解其中奇数项的和，采用等差数列求和公式有，奇数项的和为`((n+1)/2)*((n+1)/2)`（和上文中n为偶数情况的求和公式统一）。此时，**序列中剩余的偶数项除以2之后，恰好是原序列的前n/2项**。如此，开始下一次循环即可。每次求解其中奇数项的和，偶数项除以2之后就是原序列的前n/2项。如此循环下去，直到序列长度为0。

该算法进行了优化，可以正确AC。

最后，需要注意的是，程序中为了防止溢出，应该采用`long long`类型，不要采用int型。使用`typedef`定义简化书写。

```
typedef long long ll;
```


### 求解

```cpp
#include <iostream>
using namespace std;

typedef long long ll;   //防止溢出
int main(){
    ll length;
    ll res;
    while(cin>>length){
        res = 0;
        while(length>0){
            res += ((length+1)/2)*((length+1)/2);
            length = length/2;
        }
        cout<<res<<endl;
    }
    return 0;
}
```

##  买苹果

### 题目

小易去附近的商店买苹果，奸诈的商贩使用了捆绑交易，只提供6个每袋和8个每袋的包装(包装不可拆分)。 可是小易现在只想购买恰好n个苹果，小易想购买尽量少的袋数方便携带。如果不能购买恰好n个苹果，小易将不会购买。 

* 输入描述
输入一个整数n，表示小易想购买n(1 ≤ n ≤ 100)个苹果

* 输出描述
输出一个整数表示最少需要购买的袋数，如果不能买恰好n个苹果则输出-1

* 输入例子
20

* 输出例子
3

### 分析
* 思路1：动态规划（通用解法）
采用动态规划求解。思路同本文中前面的编程题——跳石板。创建一个vector容器steps，`steps[i]`表示购买i个苹果所需的最小袋数。初始化为steps容器为INT_MAX。从1苹果开始遍历，若`steps[i]`为INT_MAX，表示无法购买该个数的苹果，直接开始下次循环。若`steps[i]`不为INT_MAX，表示该个数的苹果可以购买，进行动态规划求解。动态规划的转移方程为

```
steps[i+j] = min(steps[i]+1,steps[i+j])   //j为6或8
steps[0] = 0
```

动态规划的过程如下图所示。

![dp-wangyi-2.PNG](http://ol3kbaay9.bkt.clouddn.com/dp-wangyi-2.PNG)


* 思路2：贪婪算法
对于金额，优先选取每袋含有8个苹果的包装。若还有余数，则再用6个装的包装去购买。如果不行的话，则将8个装的个数减去1个，进行**回溯**，再用6包装的去购买。如果还不行的话，再次**回溯**，直到购买8包装的个数为0。

**贪婪算法并不一定能得到最优解，但是一个可行的，较好的解。**下面对使用贪婪算法能否得到最优解进行分析。

首先，**6和8都是偶数。因此，能凑出的个数也一定是偶数。程序中若苹果总数是奇数，可以直接返回-1。**

再次，偶数个苹果数对8取模，其结果只可能为0,2,4,6。若余数为6或者0，则可以直接用6包装情况处理，不需要**回溯**购买8包装的情况。若余数为4，只需回溯1次即可，因为`8+4=12, 12%6 = 0`。若余数为2，只需回溯2次即可，因为`8+8+2=18, 18%6 = 0`。

综上，本题情况使用贪婪算法一定能得到最优解。


> 贪婪算法并不一定能得到最优解，但是一个可行的，较好的解。例如，给定硬币coins=[1,2,10,25]，金额总数amounts=30，不限制每种币值的硬币数量，要求用所给硬币凑出所需金额，并且硬币数量最少。若采用贪婪算法求解，需要6枚（25+5*1）硬币。 若采用动态规划求解，所需3枚（10+10+10）硬币。  --- [贪婪算法](http://www.infocool.net/kb/Arithmetic/201612/251791.html)


* 思路3：数字分析求解。`O(1)`算法
对数字特征进行分析。

首先，**6和8都是偶数。因此，能凑出的个数也一定是偶数。程序中若苹果总数是奇数，可以直接返回-1。**

再次，偶数个苹果数对8取模，其结果只可能为0,2,4,6。若余数为6或者0，则可以直接用6包装情况处理，不需要**回溯**购买8包装的情况。若余数为4，只需回溯1次即可，因为`8+4=12, 12%6 = 0`。若余数为2，只需回溯2次即可，因为`8+8+2=18, 18%6 = 0`。

综上，可以采用如下思路进行处理。（**由于数字6和8的特征，本方法只适用于本题**）
* 情况1：若num不是偶数，则直接返回-1
* 情况2：若num%8 = 0，则返回num/8
* 情况3：若num%8 != 0，则只需回溯1次或者2次8包装购买个数，就可以求解。回溯1次，其结果为`n/8-1+2 = n/8+1`；回溯1次，其结果为`n/8-2+3 = n/8+1`。因此，可以情况3下，可以返回`n/8+1`。


### 求解

* 方法1：动态规划。

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

int main(){
    int amounts;
    cin>>amounts;
    vector<int> steps(amounts+1,INT_MAX);
	steps[6] = 1;
    steps[8] = 1;
    for(int i=6;i<=amounts;i++){
        if(steps[i] == INT_MAX){
            continue;
        }
        else{
            if(i+6 <= amounts){
                steps[i+6] = min(steps[i]+1,steps[i+6]);
            }
            if(i+8 <= amounts){
                steps[i+8] = min(steps[i]+1,steps[i+8]);
            }
            
        }
    }
    steps[amounts] = (steps[amounts] == INT_MAX)? -1:steps[amounts];
    
    cout<<steps[amounts]<<endl;
    return 0;
}
```

* 方法2：贪婪算法。


```
#include <iostream>
using namespace std;

int maxPackages(int num) {
	int res = 0;
	int mul, remains;
	if(num%2 != 0){
        return -1;  //非偶数直接返回
    }
    
	if (num % 8 == 0) {
		res += num / 8;
		return res;
	}
	else{
		mul = num / 8;  //倍数
		remains = num % 8;
		res += mul;
		num = num % 8;
		while (mul >= 0) {  //回溯8包装
			if (num % 6 == 0) {
				res += num / 6;
				return res;
			}
			else {
				mul--;  //回溯  8包装购买袋数-1
				res--;
				num = num + 8;
			}
		}
	}
	return -1;

}


int main() {
	int num;
	while (cin >> num) {
		cout << maxPackages(num) << endl;
	}
	return 0;
}
```

* 方法3：数字分析求解。`O(1)`。

```cpp
#include <iostream>
using namespace std;

int main() {
	int num;
	while (cin >> num) {
		if(num%2 != 0){
            cout<<-1<<endl;
        }
        else{
            if(num%8 == 0){
                cout<<num/8<<endl;
            }
            else{
                cout<<1+num/8<<endl;
            }
        }
	}
	return 0;
}
```

## 计算糖果
### 题目
A,B,C三个人是好朋友,每个人手里都有一些糖果,我们不知道他们每个人手上具体有多少个糖果,但是我们知道以下的信息：
A - B, B - C, A + B, B + C. 这四个数值.每个字母代表每个人所拥有的糖果数.
现在需要通过这四个数值计算出每个人手里有多少个糖果,即A,B,C。这里保证最多只有一组整数A,B,C满足所有题设条件。 
输入描述:
输入为一行，一共4个整数，分别为A - B，B - C，A + B，B + C，用空格隔开。
范围均在-30到30之间(闭区间)。


* 输出描述
输出为一行，如果存在满足的整数A，B，C则按顺序输出A，B，C，用空格隔开，行末无空格。
如果不存在这样的整数A，B，C，则输出No

* 输入例子
1 -2 3 4

* 输出例子
2 1 3

### 分析
这道题目的实质是：判断三元一次方程组是否有解及求解。

把题目条件用方程式表示
A-B=Y1;
B-C=Y2;
A+B=Y3;
B+C=Y4;

用消元法求解
A=(Y1+Y3)/2;
B=(Y3-Y1)/2=(Y2+Y4)/2;
C=(Y4-Y2)/2;

由于题目给出的是整数，要求解也是整数，这个约束条件也需要注意下。

不满足约束条件就是没解，就可以输出NO了，满足所有的约束条件那就是有解。

或者也可以直接判断Y1+Y3是否为偶数，Y2+Y4是否为偶数，Y4-Y2是否为偶数，Y3-Y1是否为偶数。

### 求解

```cpp
#include <iostream>
using namespace std;

int main(){
    int Y1,Y2,Y3,Y4;
    while(cin>>Y1>>Y2>>Y3>>Y4){
        if((Y1+Y3)%2 == 0 && (Y4-Y2)%2 == 0 && (Y2+Y4)%2 == 0 && (Y3-Y1)%2 == 0){
            if((Y3-Y1) == (Y2+Y4)){
                cout<<(Y1+Y3)/2<<" ";  //A
                cout<<(Y2+Y4)/2<<" ";   //B
                cout<<(Y4-Y2)/2<<endl;   //C
            }
            else{
                cout<<"No"<<endl;
            }
        }
        else{
            cout<<"No"<<endl;
        }
    }
    return 0;
}

```

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 