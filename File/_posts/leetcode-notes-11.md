---
title: LeetCode Notes - 11
date: 2017-05-20 18:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---



* [LeetCode](https://leetcode.com)
* 本文包括[LeetCode - 48. Rotate Image](https://leetcode.com/problems/rotate-image/#/description) ，`微软 - Image Encryption`，`美团 - 矩阵旋转45度`，[LeetCode - 172. Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/#/description)和[LeetCode - 233. Number of Digit One](https://leetcode.com/problems/number-of-digit-one/#/description) 5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。


<!--more-->




## Rotate Image(48)
### Description
* [LeetCode - 48. Rotate Image](https://leetcode.com/problems/rotate-image/#/description)
* You are given an `n x n` 2D matrix representing an image.
* Rotate the image by 90 degrees (clockwise).
* Follow up
Could you do this in-place?


### Analysis

参考[LeetCode - Solution](https://leetcode.com/problems/rotate-image/#/solutions)了解更多。
 
 
 * 思路1: 引入额外的存储空间。
 
若矩阵左上角坐标为(0,0)，即相对原点进行旋转，

旋转90度对应的公式为`data[i][j] ---> data[j][n-1-i]`。逆推则有，`ddata[n-1-j][i] ---> data[i][j]`。

旋转180度（即在旋转90度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-i][n-1-j]`。

旋转270度（即在旋转180度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-j][i]`。

若矩阵左上角坐标为(xA,yA)，旋转过程中先将矩阵整体平移到原点（即`i=i-xA，j=j-yA`），再套用相对原点的矩阵旋转公式，最后再对将矩阵平移到参考点(xA,yA)（即将横坐标值加上xA，纵坐标值加上yA）

旋转90度对应的公式为`data[i][j] ---> data[j-yA+xA][n-1-i+xA+yA]`。逆推则有，` data[n-1+xA+yA-j][i+yA-xA] ---> data[i][j]`。

旋转180度（即在旋转90度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-i+2*xA][n-1-j+2*yA]`。逆推则有，`ddata[n-1+2*xA-i][n-1+2*yA-j] ---> data[i][j]`。

旋转270度（即在旋转180度的基础上再旋转90度）对应的公式为`data[i][j] ---> data[n-1-j+xA+yA][i-xA+yA]`。逆推则有，`data[j+xA-yA][n-1+yA+xA-i] ---> data[i][j]`。
 

 * 思路2: 不引入额外的存储空间。

若顺时针旋转矩阵，则先将矩阵进行上下翻转，然后对矩阵求转置即可。

若逆时针旋转矩阵，则先将矩阵进行左右翻转，然后对矩阵求转置即可。

 对于一个二维数组，调用`reverse()`函数，会将二维数组作为一维数组进行翻转。其效果为将数组元素上下颠倒。

掌握下述程序中求矩阵转置的方法。
 
```
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/
void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

### Slove
#### Method 1: C++

* 方法1：参照思路1，引入额外的存储空间。Cost 6ms. Beats 12.77%

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        vector<vector<int>> temp(matrix);
        int size = matrix.size();
        //Rotate 90 degree
        // data[i][j] ---> data[j][size-1-i]
        // data[size-1-j][i] ---> data[i][j]
        for(int i=0;i<size;i++){
            for(int j=0;j<size;j++){
                matrix[i][j] = temp[size-1-j][i];
            }
        }
    }
};
```

* 方法2：参考思路2。Cost 3ms. Beats 63.84%


```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
       reverse(matrix.begin(),matrix.end());
       int size = matrix.size();
       for(int i=0;i<size;i++){
           for(int j=i+1;j<size;j++){
               swap(matrix[i][j],matrix[j][i]);
           }
       }
    }
};
```




#### Method 2: JavaScript
原理同C++，此处不再赘述。


#### Method 3: Java
原理同C++，此处不再赘述。


 
## 微软 - Image Encryption

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

## 美团 - 矩阵旋转45度
### 题目

将二维数组逆时针旋转45度输出。

* 测试样例

```
1 2 3
4 5 6
7 8 9

//逆时针旋转45度之后
3
2 6
1 5 9
4 8
7

```

### 分析
逆时针旋转45度，如下图所示。在矩阵元素上绘制逆时针旋转45的标记线，每条线上的元素为旋转后矩阵的一行。

![matrix-rotate-45.PNG](http://ol3kbaay9.bkt.clouddn.com/matrix-rotate-45.PNG)

若初始矩阵维数为`n*n`，则有如下规律
 
* 旋转之后矩阵的列的长度为n。
 从图中可以看出，标记线所包括最多元素的情况出现在矩阵的主对角线上。而主对角线上包括的元素个数为n。因此，旋转后的矩阵的列的长度为n。
 
* 旋转之后矩阵的行的长度为`2*n-1`。
观察上图中标记线，要满足旋转前后矩阵元素的个数相等，而旋转后的矩阵每行的元素为`1,2,3...n,(n-1)...2,1`。该式子中共有`n+n-1 = 2*n-1`项。所以旋转之后矩阵的行的长度为`2*n-1`。

```
//旋转前后，元素个数不变
1+2+3+...+n+(n-1)+...+2+1 = n*(n+1)/2 + n*(n-1)/2 = n*n
```
* 综上有，旋转后矩阵的维数为`(2*n-1)*n`。

 
 




### 求解

```cpp
#include <iostream>
#include <vector>
using namespace std;


vector<vector<int>> rotate45(vector<vector<int>> matrix) {
	int size = matrix.size();
	vector<vector<int>> res(2 * size - 1, vector<int>(size, 0));
	
	//循环开始条件 （右上角）
	int i = 0;
	int j = size - 1; 
	int tempi, tempj;
	//新矩阵的坐标
	int x = 0;
	int y = 0;
	//bool changeRow;
	bool changeRow = true;  //顶点先遍历行

	//循环终止条件
	while(!(i == (size) && j == 0)){
		
		res[x][y] = matrix[i][j];
		//保存当前i和j的值
		tempi = i;
		tempj = j; 
		while ((tempi+1) < size && (tempj+1) < size) {
			res[x][++y] = matrix[++tempi][++tempj];
		}
		if (i == 0 && j == 0) {
			//顶点行遍历结束，下面顶点开始列遍历
			changeRow = false;
		}
		if (changeRow == true) {
			//顶点遍历行
			j--;
		}
		else if (changeRow == false) {
			//顶点遍历列
			i++;
		}
		x++;
		y = 0;
	}
	
	return res;
}

int main() {
	vector<vector<int>> matrix = { { 1,2,3 },{ 4,5,6 },{ 7,8,9 } };

	int size = matrix.size();
	vector<vector<int>> rotated;
	rotated = rotate45(matrix);
	int rows = rotated.size();
	int cols = rotated[0].size();
	for (int i = 0; i<rows; i++) {
		for (int j = 0; j<cols; j++) {
			if ((i + 1) <= cols) {
				//1 2 3 ... n
				if (j < (i + 1)) {
					cout << rotated[i][j] << " ";
				}
			}
			else {
				//(i + 1) > cols
				//n-1,n-2 ... 2, 1  
				if (j < cols - (i + 1) % cols) {
					cout << rotated[i][j] << " ";
				}
			}
			
		}
		cout << endl;
	}
	system("pause");
	return 0;
}
```





## Factorial Trailing Zeroes(172)  
### Description
* [LeetCode - 172. Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/#/description)
* Given an integer n, return the number of trailing zeroes in `n!`.
* Note: Your solution should be in logarithmic time complexity.

### Analysis
参考[LeetCode Solution](https://leetcode.com/problems/factorial-trailing-zeroes/#/solutions)了解更多。

>`10` is the product of `2` and `5`. In `n!`, we need to know how many 2 and 5, and **the number of zeros is the minimum of the number of 2 and the number of 5.**
>
>Since multiple of 2 is more than multiple of 5, **the number of zeros is dominant by the number of 5.**

Sometimes one number may have several 5 factors, for example, 25 have two 5 factors, 125 have three 5 factors. In the n! operation, factors 2 is always ample. So we just count how many 5 factors in all number from 1 to n.

* Example One

How many multiples of 5 are between 1 and 23 ? 

There is 5, 10, 15, and 20, for four multiples of 5. Paired with 2's from the even factors, this makes for four factors of 10, so: 23! has 4 zeros.

5,10,15,20除以5之后，是1,2,3,4，不含有因子5。

* Example Two

How many multiples of 5 are there in the numbers from 1 to 100?

Because 100 ÷ 5 = 20, so, there are twenty multiples of 5 between 1 and 100.

but wait, actually 25 is 5×5, so each multiple of 25 has an extra factor of 5, e.g. 25 × 4 = 100，which introduces extra of zero.

So, we need know how many multiples of 25 are between 1 and 100? Since 100 ÷ 25 = 4, there are four multiples of 25 between 1 and 100.

Finally, we get 20 + 4 = 24 trailing zeroes in 100!

The above example tell us, we need care about 5, 5×5, 5×5×5, 5×5×5×5 ....

* Example Three

By given number 4617.

5^1 : 4617 ÷ 5 = 923.4, so we get 923 factors of 5

5^2 : 4617 ÷ 25 = 184.68, so we get 184 additional factors of 5

5^3 : 4617 ÷ 125 = 36.936, so we get 36 additional factors of 5

5^4 : 4617 ÷ 625 = 7.3872, so we get 7 additional factors of 5

5^5 : 4617 ÷ 3125 = 1.47744, so we get 1 more factor of 5

5^6 : 4617 ÷ 15625 = 0.295488, which is less than 1, so stop here.

Then 4617! has 923 + 184 + 36 + 7 + 1 = 1151 trailing zeroes.



### Slove
#### Method 1: C++
* 方法1: Cost 3ms. Beats 18.34%. 

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int result = 0;
        //注意使用long long类型，防止数据溢出
        //for循环终止时，i大于n
        for(long long i=5;(n/i)>0;i*=5){
            result += n/i;
        }
        return result;
    }
};
```

* 方法2: Cost 3ms. Beats 18.34%. 

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int result = 0;
        while(n>0){
            n /= 5;
            result += n; 
        }
    
        return result;
    }
};
```


* 方法3: 函数递归。Cost 3ms. Beats 18.34%. 

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
       return (n==0)? 0:n/5 + trailingZeroes(n/5);
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。



## Number of Digit One(233)   
### Description
* [LeetCode - 233. Number of Digit One](https://leetcode.com/problems/number-of-digit-one/#/description)
* Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.
* For example
Given n = 13,
Return 6, because digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.
* Hint
Beware of overflow.

### Analysis
* 参考资料
	- [从1到n整数中1出现的次数 - O(logn)算法](http://blog.csdn.net/yi_afly/article/details/52012593)
	- [程序员面试题精选100题(25)-在从1到n的正数中1出现的次数](http://zhedahht.blog.163.com/blog/static/25411174200732494452636/)
	- [LeetCode Solution](https://discuss.leetcode.com/topic/18054/4-lines-o-log-n-c-java-python)

* 思路1
	- 参考 [从1到n整数中1出现的次数 - O(logn)算法](http://blog.csdn.net/yi_afly/article/details/52012593)。
	- 将数字n的位数分为各位和其他位两大类情况。
	- 记每一位的权值为base，位值为weight，该位之前的数是former，如下图所示。
	- 对于其他位来说，若weight为0，则1出现次数为`round*base`；若weight为1，则1出现次数为`round*base+former+1`；若weight大于1，则1出现次数为`rount*base+base`。
	- 对于个位，若weight为0，则1出现次数为`round*1`；若个位等于0，则1出现的次数为`round*1+1`。
	
 ![leetcode-233.PNG](http://ol3kbaay9.bkt.clouddn.com/leetcode-233.PNG)


* 思路2


```cpp
int countDigitOne(int n) {
    int ones = 0;
    for (long long m = 1; m <= n; m *= 10) {
        int a = n/m, b = n%m;
        ones += (a + 8) / 10 * m + (a % 10 == 1) * (b + 1);
    }
    return ones;
}
```
参考[LeetCode Solution](https://discuss.leetcode.com/topic/18054/4-lines-o-log-n-c-java-python)

Let me use variables `a` and `b` to make the explanation a bit nicer.


Go through the digit positions by using position multiplier m with values 1, 10, 100, 1000, etc.

For each position, split the decimal representation into two parts, for example split n=3141592 into a=31415 and b=92 when we're at m=100 for analyzing the hundreds-digit. And then we know that the hundreds-digit of n is 1 for prefixes "" to "3141", i.e., 3142 times. Each of those times is a streak, though. Because it's the hundreds-digit, each streak is 100 long. So `(a / 10 + 1) * 100` times, the hundreds-digit is 1.

Consider the thousands-digit, i.e., when m=1000. Then a=3141 and b=592. The thousands-digit is 1 for prefixes "" to "314", so 315 times. And each time is a streak of 1000 numbers. However, since the thousands-digit is a 1, the very last streak isn't 1000 numbers but only 593 numbers, for the suffixes "000" to "592". So `(a / 10 * 1000) + (b + 1)` times, the thousands-digit is 1.

**The case distincton between the current digit/position being 0, 1 and >=2 can easily be done in one expression. With (a + 8) / 10 you get the number of full streaks, and a % 10 == 1 tells you whether to add a partial streak.**

如果a大于2，则根据上述分析，求解表达式为`a/10+1`，这个`(a+8)/10`是相同的。如a小于2，则`(a+8)/10`和`a/10`效果相同。

表达式`(n/m%10 == 1)*(n%m+1)`中，若`(n/m%10 == 1)`为真，则返回1，否则返回0。

### Slove
#### Method 1: C++

* 方法1：参照思路1，O(logn)算法。Cost 0ms. Beats 45.97%。

```cpp
class Solution {
public:
    int countDigitOne(int n) {
        if(n<1) return 0;
        int count = 0;
        int base = 1;
        int round = n;
        int weight;
        while(round>0){
            weight = round%10;
            round /= 10;
            count += round*base;
            if(weight==1){
                count += (n%base)+1;
            }
            else if(weight > 1){
                count += base;
            }
            base *= 10;
        }
        return count;
    }
};
```


**为了防止数据溢出，注意使用`long long`数据类型。因为for循环最后终止时，m值是大于n的。**

* 方法2：参考思路2。O(logn)算法。Cost 0ms. Beats 45.97%。


```cpp
class Solution {
public:
    int countDigitOne(int n) {
        int result = 0;
        for(long long m=1;m<=n;m*=10){
            result += (n/m+8)/10*m + (n/m%10 == 1)*(n%m+1);
        }
        return result;
    }
};
```


**为了防止数据溢出，注意使用`long long`数据类型。因为for循环最后终止时，m值是大于n的。**

#### Method 2: JavaScript

* 方法1：参考思路2。O(logn)算法。Cost 0ms. Beats 45.97%。

**注意，在JS中使用`parseInt()`保证数据为整数。**

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var countDigitOne = function(n) {
    var result = 0;
    var a,b,m;
    for(m=1;m<=n;m*=10){
        a = parseInt(n/m);
        b = n%m;
        if(a%10 > 1){
            result += (parseInt(a/10)+1)*m;
        }
        else if(a%10 === 1){
            result += (parseInt(a/10))*m +(b+1);
        }
        else {
            result += (parseInt(a/10))*m;
        }
    }
    return result;
};
```
#### Method 3: Java
原理同C++，此处不再赘述。





## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
