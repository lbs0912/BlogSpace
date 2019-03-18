---
title: 美团点评2016研发工程师在线编程题
date: 2017-03-17 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,美团笔试]
keywords: 动态规划,字典排序,校招笔试编程题,美团笔试
---


* 本文主要对美团笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/586807/summary)
* 下载本文中的[美团笔试试题](http://static.nowcoder.com/pdf/paper/586807.pdf)


<!--more-->


## 最大差值
### 题目
* 有一个长为n的数组A，求满足`0≤a≤b<n`的`A[b]-A[a]`的最大值。
* 给定数组A及它的大小n，请返回最大差值。
* 测试样例

```
//Input
[10,5], 2
//Output
0
```

### 分析
常规求解即可
 
### 求解

```cpp
class LongestDistance {
public:
    int getDis(vector<int> A, int n) {//时间复杂度O(n) 空间复杂度O(1)
        // write code here
        int maxDiff=0;//初始化最大差值
        int minNum=A[0];//初始化最小值
        for(int i=1;i<n;++i){//遍历
            if(A[i]<minNum)minNum=A[i];//更新最小值
            if(A[i]-minNum>maxDiff)maxDiff=A[i]-minNum;//更新最大差值             
        }
        return maxDiff; 
    }
};
```

## 棋盘翻转
### 题目
* 在4x4的棋盘上摆满了黑白棋子，黑白两色的位置和数目随机其中左上角坐标为(1,1),右下角坐标为(4,4),现在依次有一些翻转操作，要对一些给定支点坐标为中心的上下左右四个棋子的颜色进行翻转，请计算出翻转后的棋盘颜色。
* 给定两个数组A和f,分别为初始棋盘和翻转位置。其中翻转位置共有3个。请返回翻转后的棋盘。
* 测试样例

```
//Input
[[0,0,1,1],[1,0,1,0],[0,1,1,0],[0,0,1,0]],[[2,2],[3,3],[4,4]]
//Output
[[0,1,1,1],[0,0,1,0],[0,1,1,0],[0,0,1,0]]
```
### 分析
* 注意`f`数组中取值范围从1~4，而`A`数组中取值范围从0~3。
* `num XOR 1`可以将1变成0，0变成1。
 
### 求解

```cpp
class Flip {
public:
    vector<vector<int> > flipChess(vector<vector<int> > A, vector<vector<int> > f) {
        // write code here
        vector<vector<int>> delta = {{-1,0},{1,0},{0,-1},{0,1}};  //上下左右4个临界方向
        int length = f.size();
        int x;
        int y;
        for(int i=0;i<length;i++){ //遍历f数组
            for(int j=0;j<4;j++){  //上下左右4个方向
            	x = f[i][0]+delta[j][0];
              	y = f[i][1]+delta[j][1];
                if((x >= 1 && x <= 4) && (y >= 1 && y <= 4)){  //在棋盘范围内
                	//翻转
                    A[--x][--y] ^= 1;  //异或取反
                }
            }
        }
        return A;
     }
};
```

## 拜访
### 题目
* 现在有一个城市销售经理，需要从公司出发，去拜访市内的商家，已知他的位置以及商家的位置，但是由于城市道路交通的原因，他只能在左右中选择一个方向，在上下中选择一个方向，现在问他有多少种方案到达商家地址。
* 给定一个地图`map`及它的长宽`n`和`m`，其中1代表经理位置，2代表商家位置，-1代表不能经过的地区，0代表可以经过的地区，请返回方案数，保证一定存在合法路径。保证矩阵的长宽都小于等于10。
* 测试样例

```
[[0,1,0],[2,0,0]],2,3
返回：2
```

### 分析
* 思路1：动态规划

参考：[动态规划-美团笔试题-拜访](http://blog.csdn.net/li563868273/article/details/51113121)和[笔试面试题-拜访](http://www.cnblogs.com/BJUT-2010/p/5530966.html)。

![meituan-1.PNG](http://ol3kbaay9.bkt.clouddn.com/meituan-1.PNG)

如上图所示，相当于在地图map大矩形区域中找出一个由经理和客户组成的小矩形。而客户和经理之间的位置关系有三种：
* 主对角线关系
* 副对角线关系
* 位置重合

![meituan-2.PNG](http://ol3kbaay9.bkt.clouddn.com/meituan-2.PNG)


采用动态规划求解，参考如上示意图，其步骤如下所示
* 找出经理和客户的位置，确定小矩形的轮廓。经理位置和客户位置分别用`(x1,y1)`和`(x2,y2)`表示。为了方便起见， `(x1,y1)`表示x坐标较小的数。（从客户到经理的路程方案总数和从经理到客户的路程方案总数相同）
* 设置动态规划二维数组，`dp[i][j]`表示从起点`(x1,y1)`到点`(i,j)`的路程方案总数。
* 初始化小矩形边界。为了防止越界，对小矩形边界进行初始化，对于起点所在的行，`dp[i][j] = (map[i][j] == -1) ? 0:dp[i-1][j]`；对于起点所在的列，`dp[i][j] = (map[i][j] == -1) ? 0:dp[i][j-1]`。遇见`-1`，将其置于0，表示到达该点的路程方案总数为0。
* 动态规划求解，对于主对角线，`dp[i][j] = (map[i][j] == -1) ? 0:dp[i-1][j]+dp[i][j-1]`。对于副对角线，`dp[i][j] = map[i][j] == -1 ? 0 : dp[i - 1][j] + dp[i][j + 1];`。
* 最后返回计算结果，即`dp[x2][y2]`。
 
### 求解

```cpp
class Visit {
public:
	int countPath(vector<vector<int> > map, int n, int m) {
		// write code here
		//寻找二者位置
		int x1, y1, x2, y2;
		for (int i = 0; i<n; i++) {
			for (int j = 0; j<m; j++) {
				if (map[i][j] == 1) {  //经理位置
					x1 = i;
					y1 = j;
				}
				if (map[i][j] == 2) {  //商家位置
					x2 = i;
					y2 = j;
				}
			}
		}
		//二者位置重合，直接返回1
		if ((x1 == x2) && (y1 == y2)) {
			return 1;
		}

		//为了计算方便，保证x1为较小的值
		if (x1 > x2) {
			int temp = x1;
			x1 = x2;
			x2 = temp;
			temp = y1;
			y1 = y2;
			y2 = temp;
		}


		//动态规划数组创建
		vector<vector<int>> dp(n,vector<int> (m));
		dp[x1][y1] = 1;  //初始值

		//二者在主对角线关系或者同一行
		if (y1 <= y2) {
			//动态规划数组边界初始化，防止越界
			for (int i = x1 + 1; i <= x2; i++) {
				dp[i][y1] = (map[i][y1] == -1) ? 0 : dp[i - 1][y1];
			}
			for (int j = y1 + 1; j <= y2; j++) {
				dp[x1][j] = (map[x1][j] == -1) ? 0 : dp[x1][j - 1];
			}
			for (int i = x1 + 1; i <= x2; i++) {
				for (int j = y1 + 1; j <= y2; j++) {
					dp[i][j] = (map[i][j] == -1) ? 0 : dp[i][j - 1] + dp[i - 1][j];
				}
			}
		}
		//二者在副对角线关系
		else {
			//y1 > y2
			//动态规划数组边界初始化，防止越界
			for (int i = x1 + 1; i <= x2; i++) {
				dp[i][y1] = (map[i][y1] == -1) ? 0 : dp[i - 1][y1];
			}
			for (int j = y1 - 1; j >= y2; j--) {
				dp[x1][j] = (map[x1][j] == -1) ? 0 : dp[x1][j + 1];
			}
			for (int i = x1 + 1; i <= x2; i++) {
				for (int j = y1 - 1; j >= y2; j--) {
					dp[i][j] = map[i][j] == -1 ? 0 : dp[i - 1][j] + dp[i][j + 1];
				}
			}
		}
		return dp[x2][y2];

	}
};
```

## 直方图内最大矩形
### 题目
* 有一个直方图，用一个整数数组表示，其中每列的宽度为1，求所给直方图包含的最大矩形面积。比如，对于直方图[2,7,9,4],它所包含的最大矩形的面积为14(即[7,9]包涵的7x2的矩形)。
* 给定一个直方图A及它的总宽度n，请返回最大矩形面积。保证直方图宽度小于等于500。保证结果在int范围内。
* 测试样例

```
[2,7,9,4,1],5
返回：14
```

### 分析
* 该题目同[LeetCode - 84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/#/description)。参见该题的答题笔记和分析过程。
* 思路1：暴力求解+分而治之  
	- 将问题分解，分而治之，先求解包含前1个直方图时候面积最大值，再求解包含前2个直方图时候面积最大值，在求解包含前3个直方图时候面积最大值。
	- 对于每个子问题，每个子问题中求解的最大面积都应该包括新增的矩形边界。例如，求解前2个直方图面积最大值时，需要求解只包含第2个直方图时面积最大值和包含第2个，第1个直方图时面积最大值。求解前3个直方图面积最大值时，需要求解只包含第3个直方图时面积最大值，包含第3个，第2个直方图时面积最大值和包含第3个，第2个，第1个直方图时面积最大值。
	- 很遗憾，该算法虽然正确，在牛客网上运行正确，但是在LeetCode上提交会被作为`Time Limited`。
	- 算法实现过程如下代码所示。

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n  = heights.size();
        int maxArea = 0;
        int minValue = 0;
    	for(int i=0;i<n;i++){
            minValue = INT_MAX;
            for(int j=i;j>=0;j--){
                minValue = min(minValue,heights[j]);
                maxArea = max(maxArea,minValue*(i-j+1));
            }
        }	
        return maxArea;
    }
};
```

* 思路2：使用栈求解。O(n)算法。
	- Reference: [Largest Rectangular Area in a Histogram | Set 2](http://www.geeksforgeeks.org/largest-rectangle-under-histogram/)和[Largest Rectangle in Histogram | Blog](http://www.cnblogs.com/ganganloveu/p/4148303.html)
	- 首先，考虑已知height数组是升序的情况？比如`1,2,5,7,8`，则最大面积为(1*5),  (2*4), (5*3), (7*2), (8*1)中的最大值，即`max(height[i]*(size-i))`。
	- 仿照上述思想，根据heights数组内容，构造一个升序的栈，则最大面积为`max(stk.top()*count)`，`count`为该元素在栈的排序个数（从栈顶计算，从1开始）
	- 遍历数组，如果该元素大于栈中的元素，则将其插入栈中，构造升序的栈。如果该元素小于栈顶元素，则将栈顶元素弹出，计算`stk.top()*count`的值。然后再将弹出的栈顶元素设为和遍历的数组元素相同的值，再将其二者都插入栈中。
	- 遍历数组结束后，会得到一个升序的栈，再计算`max(height[i]*(size-i))`即可。
	
### 求解

* 方法1：参照思路2。(Cost 12ms. Beats 70.58% )

```cpp
class MaxInnerRec {
public:
    int countArea(vector<int> A, int n) {
        // write code here
        stack<int> stk;
		int maxArea = 0;
		int count = 0;
		int length = n;
		for (int i = 0; i<length; i++) {
			if (stk.empty() || stk.top() <= A[i]) {
				stk.push(A[i]);
			}
			else {
				while (!stk.empty() && stk.top() >  A[i]) {
					count++;
					maxArea = max(maxArea, stk.top()*count);
					stk.pop();
				}
				while (count) {
					stk.push(A[i]);
					count--;
				}
				stk.push(A[i]);
			}
		}
		count = 0;
		while (!stk.empty()) {
			count++;
			maxArea = max(maxArea, stk.top()*count);
			stk.pop();
		}
		return maxArea;
    }
};
```

需要注意的是，由于AND条件判断时候的短路效应，下述代码中应该注意两个判断条件的先后顺序。如果采用第2行中的写法，当栈为空的时候，此时`stk.top()`不存在，因此运行时候会产生错误。

```cpp
//注意两个判断条件的顺序
while (!stk.empty() && stk.top() > heights[i])  //OK
while (stk.top() > heights[i] && !stk.empty())  //Error
```

另一点需要注意的是，不推荐使用在条件判断中使用自加或者自减运算。如下程序所示。测试2中，由于是后置减减，因此当count等于0时候，while循环结束，但是count仍然会再执行自减运算，最终值为-1，而不是0。
 
```cpp
//测试1
//循环结束后，count=0
while (count > 0) {
	stk.push(heights[i]);
	count--;
}

//测试1
//循环结束后，count=-1   ！！！
while (count--) {
	stk.push(heights[i]);
	count--;
}

//测试2
//循环结束后，count=0
while (--count) {
	stk.push(heights[i]);
	count--;
}
```


## 字符串和字典排序
### 题目
* 求字典序在`s1`和`s2`之间的，长度在`len1`到`len2`的字符串的个数，结果`mod 1000007`。
* 输入描述：每组数据包涵s1（长度小于100），s2（长度小于100），len1（小于100000），len2（大于len1，小于100000）。
* 输出描述：输出答案。
* 输入例子：ab ce 1 2
* 输出例子：56

### 分析
* 参考资料
	- [牛客网](https://www.nowcoder.com/questionTerminal/f72adfe389b84da7a4986bde2a886ec3)（参考该解答为主）
	- [字符串计数（字典序）--- 美团2016研发工程师在线编程题](http://blog.csdn.net/chengonghao/article/details/52132218)
	- [美团笔试-字符串计数](http://blog.csdn.net/li563868273/article/details/51132278)
* 引言：计算两个10进制数字之间的数字个数
求解本题之前，先引入一个例子：计算两个10进制数字之间的数字个数。
例如，计算在`138`和`652`之间的数字个数。（示例1）
(1) 计算方法1
 `(652-138)-1 = 514-1=513`。`(652-138)`计算的结果中包括652，不包括138，因此，需要再对其减去1。
(2) 计算方法2
  `(6-1)*(10^2)+(5-3)*(10^1)+(2-8)*(10^0)-1 = (500+20-6)-1 = 514-1`
  
同理，（示例2）计算数字`138`和`652`之间的4位数字（按照字典排序，而不是十进制大小排序）。
先将这两个数字补齐为4位数，则得到`1380`和`6520`。则二者之间的4位数有`(6-1)*(10^3)+(5-3)*(10^2)+(2-8)*(10^1)+(0-0)*(10^0) = 5140`。
注意，因为计算结果中需要包括`1380`（字典排序中1380大于138），不包括`6520`，因此，计算结果中并不需要减去1。

比较上面两个例子发现，当两个数字位数相同时候，计算的结果中需要减去1。当两个数字位数不同时，计算结果并不需要减去1。

下面对上述结论进行验证。（示例3）计算数字18和656之间的1到4位数字（字典排序，而非十进制数字的大小比较）。
Step 1： 将数字补齐为4位数字：`1800`和`6560`
Step 2： 1位数的个数为`n1 = 6-1 = 5`。（不包括1，包括6）
Step 3： 2位数的个数为`n2 = n1*10+(5-8) = 47`。（不包括18，包括65）
Step 4： 3位数的个数为`n3 = n2*10+(6-0) = 476`。（不包括656，包括180）
Step 5： 4位数的个数为`n4 = n3*10+(0-0) = 4760`。（不包括6560，包括1800）
（上述示例3中两个数字的位数不同，因此不需要在结果中再减去1）
 
 
 * 字典排序中计算两个字符之间的字符个数

求解该问题中，可以借鉴上述“计算两个10进制数字之间的数字个数”的思想，将字符串作为26进制数处理。下面以计算在`ab`和`ce`之间长度在[1,2]的字符串个数为例进行说明。（示例4）
Step 1： 由于字符已经为2位，因此不需要补齐。
Step 2： 1位数的个数为`n1 = (int)(c-a) = 2`。（不包括a，包括c）
Step 3： 2位数的个数为`n2 = n1*26+(int)(e-b) -1 = 52+3-1 = 55-1 = 54`。（不包括ab，也不包括ce。两个字符长度相同，因此需要在计算结果中减去1）
Step 4： 因此，最终计算结果为`2+54=56`
 
下面以计算在`ab`和`ce`之间长度在[1,3]的字符串个数为例进行说明。（示例4）
Step 1： 将字符串补齐为4位字符串：`aba`和`cea`
Step 2： 1位数的个数为`n1 = (int)(c-a) = 2`。（不包括a，包括c）
Step 3： 2位数的个数为`n2 = n1*26+(int)(e-b) = 52+3 = 55`。（不包括ab，也不包括ce。在**最终的结果**中需要减去多计算的ab，即减去1。注意是在最终的结果中减去1，而不是在中间的计算结果中减去1）
Step 4： 3位数的个数为`n3 = n2*26+(int)(a-a) = 1430`。（不包括cea，包括aba。减1只减1次，因此这次计算结果不用减去1）
Step 5： 因此，最终计算结果为`2+55+1430-1=1487-1=1486`

* 注意事项
	- 当两个字符串长度相同时，求解的结果需要减去1，注意是在最终的结果中减去1，而不是在中间的计算结果中减去1，并且只用减1次。以上述示例4为例，如果在中间计算结果`n2`中减去1（排除`ab`字符串），则后续计算`n3`时候，会少计算26个字符（少计算了`aba`,`abb`,`abc`...`abz`字符）。
	- 题目中限定了`len2>len1`。按照上述思路编写的算法在`len2>len1`情况下可以正常计算。如果`len2=len1`，则求解结果不再正确。例如， 在`a`和`b`之间长度在[2,2]的字符串个数`count22=25`（正确结果应该是26）
 
### 求解
```cpp
# include <iostream>
#include <string>
#include <vector>
using namespace std;

#define M 1000007

int stringCount(string &s1, string &s2, int len1, int len2) {
	int count = 0;
	int temp = 0;
	int length1 = s1.size();
	int length2 = s2.size();
	//将s1, s2看作26进制数，并将其补齐为len2的长度
	if (len2 > length1) {
		s1.append(len2-length1,'a');
	}
	if (len2 > length2) {
		s2.append(len2 - length2, 'a');
	}
	//计算每一位的差值
	vector<int> diff(len2);
	for (int i = 0; i < len2; i++) {
		diff[i] = s2[i] - s1[i];
	}
	//计算len1长度的符合条件的字符串个数
	for (int i = 0; i < len1 - 1; i++) {
		temp = temp * 26 + diff[i];
	}
	//遍历len1至len2长度，计算符合条件的字符串个数
	for (int i = len1-1; i < len2; i++) {
		temp = temp * 26 + diff[i];
		count = count + temp;
	}
	if (length1 == length2) {
		count--;
	}
	return count%M;
}
int main() {
	string s1, s2;
	int len1, len2;
	while (cin >> s1 >> s2 >> len1 >> len2) {
		cout << stringCount(s1, s2, len1, len2) << endl;
	}
	return 0;
}
```


## 平均年龄
### 题目
* 已知某公司总人数为W，平均年龄为Y岁(每年3月末计算，同时每年3月初入职新人)，假设每年离职率为x，x>0&&x<1,每年保持所有员工总数不变进行招聘，新员工平均年龄21岁。 
* 从今年3月末开始，请实现一个算法，可以计算出第N年后公司员工的平均年龄。(最后结果向上取整)。
* 输入描述：输入W Y x N
* 输出描述：输出第N年后的平均年龄
* 输入例子：5 5 0.2 3
* 输出例子 :  15

### 分析
* 易错点
	- 老员工每过一年，年龄会增加一岁
	- 输出的结果是对最后的结果求取`ceil()`，不是每一步都向上取整
	- 题目中规定员工总人数保持不变，说明每年入职的新员工是`W*x`名
* 计算公式推导
	- `Y = (21*W*x+(W-W*x)*(Y+1))/W = 21*x+(1-x)*(Y+1)`
	- 计算公式中和总人数`W`无关
 
### 求解
* 方法1：参照思路1。

```cpp
#include <iostream>
#include <cmath>
 
using namespace std;
void AverageAge()
{
    // 总人数为W，平均年龄为Y岁
    // 每年离职率为x，x>0&&x<1
    double W, Y, x, N;
    while(cin >> W >> Y >> x >> N)
    {
        while(N--)
        {
            Y = 21 * x + (1 - x) * (Y + 1);
        }
        cout << ceil(Y) << endl;
    }
}
 
int main()
{
    AverageAge();
    return 0;
}
```

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 