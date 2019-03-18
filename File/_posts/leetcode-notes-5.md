---
title: LeetCode Notes - 5
date: 2017-03-01 15:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---





* [LeetCode](https://leetcode.com)
* 本文包括[LeetCode -  260. Single Number III](https://leetcode.com/problems/single-number-iii/)，  [LeetCode -  318. Maximum Product of Word Lengths](https://leetcode.com/problems/maximum-product-of-word-lengths/?tab=Description)，  [LeetCode - 463. Island Perimeter](https://leetcode.com/problems/island-perimeter/?tab=Description)，[LeetCode - 412. Fizz Buzz](https://leetcode.com/problems/fizz-buzz/?tab=Description) 和 [LeetCode - 419. Battleships in a Board](https://leetcode.com/problems/battleships-in-a-board/?tab=Description) 5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。


<!--more-->





## Single Number III(260)
### Description
* [LeetCode -  260. Single Number III](https://leetcode.com/problems/single-number-iii/)
* Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.
* For example: Given `nums = [1, 2, 1, 3, 2, 5]`, return `[3, 5]`.
* Note
	- The order of the result is not important. So in the above example, [5, 3] is also correct.
	- Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?
* Tags: Bit Manipulation

### Analysis
* 思路1（位运算）
	- 参考 [LeetCode -  136. Single Number](https://leetcode.com/problems/single-number/?tab=Description)中的解法，解决本题。[LeetCode -  136. Single Number](https://leetcode.com/problems/single-number/?tab=Description)题目中，借助异或运算可以求得数组中只出现1次的数值。
	- 假设本题中只出现1次的两个数字时`A`和`B`，则对数组进行异或遍历之后（第1次遍历），其结果为`temp = A ^ B`。`temp`二进制值中为1的位表明，该位上，`A`和`B`的值不同。由此，可以区分开`A`和`B`。
	- 求取只包含`temp`中最低位的1，其余位都是0的数字，将其设为`bitFlag`。比如，若`temp=0110`，则`bitFlag=0010`。
	- 对数组进行第2次异或遍历，将数组中的值和`bitFlag`相与，会自动将数组分成两组，并且能保证A和B不在同一组中，并且能保证相同的数字出现在同一组中。据此，可以求解出数字A和B。
	- 如何求取只包含`temp`中最低位的1，其余位都是0的数字`bitFlag`？  
	**将一个数字和它的相反数（补码）行与操作，即可得到只包含该数最低位1，其余位都是0的数字，即`bitFlag = temp & (-temp)`。**
	
### Slove
#### Method 1: C++
* 方法1： 位运算。(Cost 16ms. Beats 42.19%)

```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        vector<int> res(2);
        int temp = 0;
        int size = nums.size(); 
        for(int i=0;i<size;i++){
            temp ^= nums[i];
        }
        temp &= -temp;   // get bitFlag
        for(int i=0;i<size;i++){
            if(nums[i] & temp){
                res[0] ^= nums[i];
            }
            else{
                res[1] ^= nums[i];
            }
        }
        return res;
    }
};
```



#### Method 2: JavaScript

原理同C++，此处不再赘述。(Cost 92ms. Beats 95.24%)
 

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var singleNumber = function(nums) {
    var length = nums.length;
    var res = [];
    var temp = 0;
    for(var i=0;i<length;i++){
            temp ^= nums[i];
        }
    temp &= -temp;   // get bitFlag
    for(i=0;i<length;i++){
        if(nums[i] & temp){
            res[0] ^= nums[i];
        }
        else{
            res[1] ^= nums[i];
        }
    }
    return res;
};
```

#### Method 3: Java
原理同C++，此处不再赘述。
 


##  Maximum Product of Word Lengths(318)
### Description
* [LeetCode -  318. Maximum Product of Word Lengths](https://leetcode.com/problems/maximum-product-of-word-lengths/?tab=Description)
* Given a string array `words`, find the maximum value of `length(word[i]) * length(word[j])` where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.
* Example 1:
Given `["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]`
Return `16`
The two words can be `"abcw"`, `"xtfn"`.
* Example 2:
Given `["a", "ab", "abc", "d", "cd", "bcd", "abcd"]`
Return `4`
The two words can be `"ab"`, `"cd"`.
* Example 3:
Given `["a", "aa", "aaa", "aaaa"]`
Return `0`
No such pair of words.

* Tags: Bit Manipulation

### Analysis
* 思路1（位操作）
	- 将每个字符串的每个字母转换成ascii值，进行比较。在转换过程中，将每个字符和`'a'`作差，其差值只占1位，大大节省了存储空间。转换过程中，使用移位操作。
	- C++中，`.size()`函数返回的结果是`size_type`类型。针对`string`而言，其返回值类型是`unsigned int`类型。而`max()`函数中，要求两个参数的类型值 必须相同。因此，在调用`max()`时，莫忘记强制类型转换。

```cpp
//C++
//强制类型转换  unsigned int 转化成 int
 res = max(res, (int)(words[i].size()*words[j].size()));
```

	
### Slove
#### Method 1: C++
* 方法1： 位操作。 (Cost 52ms. Beats 52.74%)

```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int res = 0;
        int size = words.size();
        vector<int> myAscii(size);  // ascii
        for(int i=0;i<size;i++){
            for(char c: words[i]){
                myAscii[i] |= 1<<(c-'a');  
                //ascii值转换，以'a'为标准，若出现字母，则对应的位设为1
            }
            for(int j=0;j<i;j++){
                if(!(myAscii[i] & myAscii[j])){
                    res = max(res, (int)(words[i].size()*words[j].size()));
                }
            }
        }
        return res;
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。

```javascript
/**
 * @param {string[]} words
 * @return {number}
 */
var maxProduct = function(words) {
    var res = 0;
    var size = words.length;
    var myAscii = [];
    for(var i=0;i<size;i++){
        for(var k=0;k<words[i].length;k++){
        	var bit = words[i].charCodeAt(k)-"a".charCodeAt(0);
            myAscii[i] |= 1<<(bit);  
            //ascii值转换，以'a'为标准，若出现字母，则对应的位设为1
        }
        for(var j=0;j<i;j++){
            if(!(myAscii[i] & myAscii[j])){
                res = Math.max(res, (words[i].length * words[j].length));
            }
        }
    }
    return res;
};
```
需要注意的是
 * JavaScript中`for..in`循环遍历的是元素的索引值，而非元素值。

```javascript
for(var c in words[i])
```
上述程序中，`c`的值是0,1,2...
 
* JavaScript中要使用如下语句代替C++中的ascii作差语句。

```javascript
var bit = words[i].charCodeAt(k)-"a".charCodeAt(0);
```

#### Method 3: Java
原理同C++，此处不再赘述。



## Island Perimeter(463)
### Description
* [LeetCode - 463. Island Perimeter](https://leetcode.com/problems/island-perimeter/?tab=Description)
* You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.
* Example:

```
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
 
Answer: 16
```


* Explanation: The perimeter is the 16 yellow stripes in the image below:

![island.png](https://leetcode.com/static/images/problemset/island.png)

* Tags: Hash Table


### Analysis
* 思路1
	- Reference: [LeetCode -  C++ solution with explanation](https://leetcode.com/problems/island-perimeter/?tab=Solutions)
	- Step 1: find how many `1` in the map. If without the consideration of surrounding cells, the total perimeter should be the total amount of 1 times 4.
	- Step 2: find how many cell walls that connect with both lands. We need to deduct twice of those lines from total perimeter.
	- 对于所有的正方形方格，只考虑其左侧的边和顶部的边这两条边。这样所有的方格都采用该处理方法，可以简化求解过程。

### Slove
#### Method 1: C++
* 方法1：参照思路1。   (Cost 229ms. Beats 10.04%)

```cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
    	int count = 0, repeat = 0;
    	for (int i = 0; i<grid.size(); i++)
    	{
    		for (int j = 0; j<grid[i].size(); j++)
    		{
    			if (grid[i][j] == 1)
    			{
    				count++;
    				if (i != 0 && grid[i - 1][j] == 1) repeat++;
    				if (j != 0 && grid[i][j - 1] == 1) repeat++;
    			}
    		}
    	}
    	return 4 * count - repeat * 2;
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。 (Cost 292ms. Beats 63.53%)

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var islandPerimeter = function(grid) {
    var count=0;
    var repeat=0;
    for(var i=0;i<grid.length;i++){
        for(var j=0;j<grid[i].length;j++){
            if(grid[i][j] === 1){
                count++;
                if((i!==0) && (grid[i-1][j]===1)){
                    repeat++;
                }
                if((j!==0) && (grid[i][j-1]===1)){
                    repeat++;
                }
            }
        }
    }
    return 4*count-2*repeat;
};
```

#### Method 3: Java
原理同C++，此处不再赘述。

## Fizz Buzz(412)
### Description
* [LeetCode - 412. Fizz Buzz](https://leetcode.com/problems/fizz-buzz/?tab=Description)
* Write a program that outputs the string representation of numbers from 1 to n.
* But for multiples of three it should output `“Fizz”` instead of the number and for the multiples of five output `“Buzz”`. For numbers which are multiples of both three and five output `“FizzBuzz”`.
* Example:

```
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```



### Analysis
该题较为简单，直接处理即可。

> In C++, `to_string()` method can convert numerical value to string. You should contain `<string>`  head file first  if you want to use it. --- [Cplusplus.com](http://www.cplusplus.com/reference/string/to_string/)
 
### Slove
#### Method 1: C++
* 方法1： 基本法  (Cost 3ms. Beats 48.17%)


```cpp
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> ret_vec(n);
        for(int i=1; i<=n; ++i)
        {
            if(i%3 == 0)
            {
                ret_vec[i-1] = "Fizz";
                if(i%5 == 0){
                    ret_vec[i-1] = "FizzBuzz";
                }
            }
            else if(i%5 == 0)
            {
               
                ret_vec[i-1] = "Buzz";
            }
            else
            {
                ret_vec[i-1] = to_string(i);
            }
        }
        return ret_vec;
    }
};
```

或者  (Cost 3ms. Beats 48.17%)

```cpp
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> ret_vec(n);
        for(int i=1; i<=n; ++i)
        {
            if(i%3 == 0)
            {
                ret_vec[i-1] += string("Fizz");
            }
            if(i%5 == 0)
            {
                ret_vec[i-1] += string("Buzz");
            }
            if(ret_vec[i-1] == "")
            {
                ret_vec[i-1] += to_string(i);
            }
        }
        return ret_vec;
    }
};
```
 
#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。

## Battleships in a Board(419)
### Description
* [LeetCode - 419. Battleships in a Board](https://leetcode.com/problems/battleships-in-a-board/?tab=Description)
* Given an 2D board, count how many battleships are in it. The battleships are represented with `'X'`s, empty slots are represented with `'.'`s. You may assume the following rules:
	- You receive a valid board, made of only battleships or empty slots.
	- Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape `1xN` (1 row, N columns) or `Nx1` (N rows, 1 column), where N can be of any size.
	- At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.
* Example:

```
X..X
...X
...X
```
In the above board there are 2 battleships.

* Invalid Example:

```
...X
XXXX
...X
```
This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

* Follow up: Could you do it in one-pass, using only `O(1)` extra memory and without modifying the value of the board?

### Analysis
* 思路1
	- Reference: [LeetCode 419. Battleships in a Board 解题报告](http://blog.csdn.net/camellhf/article/details/52871104)
	- 由于船只能在水平方向或是垂直方向上延伸，因此，船的最左边（水平方向）或是最上边（垂直方向）的元素就很特殊。对于船头的元素，其左边（水平方向）或是上边（垂直方向）的元素会是’`.’`，而对于船中部的元素，其左边或是上边的元素肯定有一个会是`’X’`。
	- 根据元素左侧或者顶部元素的类型，可以对船只个数进行计数。
	- 该思路同[LeetCode - 463. Island Perimeter](https://leetcode.com/problems/island-perimeter/?tab=Description)中的解题思路。

* 思路2（深度优先DFS搜索算法）
	- 参考资料：[九章算法 -   Battleships in a Board](http://www.jiuzhang.com/solutions/battleships-in-a-board/)
	- 遍历数组元素，若遇到`X`元素且为第1次访问，则计数值加1。设置一个`vector`容器，标记元素是否被访问。
	- 之后进行深度优先DFS搜索，函数递归实现。若为相邻的`X`，则将其标记为已经访问过了，计数值不改变。

* 思路3（广度优先BFS搜索算法）

### Slove
#### Method 1: C++
* 方法1： 思路1. (Cost 3ms. Beats 26.88%)

```cpp
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int row = board.size();
        int list = board[0].size();
        int count = 0;
        for(int i=0;i<row;i++){
            for(int j=0;j<list;j++){
                if(board[i][j] == '.' || ((i!=0) && (board[i-1][j]=='X')) || ((j!=0) && (board[i][j-1]=='X'))  ){
                
                    continue;
                }
                count++;
            }
        }
        return count;
    }
};
```

* 方法2： DFS深度优先搜索算法。 (Cost 3ms. Beats 26.88%)

```cpp
class Solution {
public:
    int rsize;
    int csize;
    vector<vector<bool>> flag;
    int go[4][2] = {{1,0},{-1,0},{0,1},{0,-1}};  //深度优先DFS搜索周围4个点
    
    //DFS
    void dfs(vector<vector<char>> & board,int i,int j){
        if(i<0 || i>=rsize || j<0 || j>= csize || board[i][j] == '.' || flag[i][j]) {
            return;
        }
        flag[i][j] = true;  //标记已经被访问
        for(int d=0;d<4;d++){
            dfs(board,i+go[d][0],j+go[d][1]); //访问(i,j)右侧的点，左侧的点，下方的点，上方的点
        }
    }
    
    int countBattleships(vector<vector<char>>& board) {
        if(board.empty()){
            return 0;
        }
        rsize = board.size();
        csize = board[0].size();
        flag.resize(rsize,vector<bool>(csize,false));  //初始化为未访问
        int result = 0;
        for(int i=0;i<rsize;i++){
            for(int j=0;j<csize;j++){
                if(board[i][j] == 'X' && !flag[i][j]){
                    result++;
                    dfs(board,i,j);
                }
            }
        }
        return result;
    }
    
};
```

* 方法3： BFS广度优先搜索算法。 (Cost 3ms. Beats 26.88%)

```cpp
class Solution {
public:
    int go[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int countBattleships(vector<vector<char>>& board) {
        if (board.empty()) return 0;
        int m = board.size(), n = board[0].size();
        vector<vector<bool>> flag(m, vector<bool>(n, false));
        int result = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 'X' && !flag[i][j]) {
                    ++result;

                    queue<pair<int, int>> q;
                    q.push({i, j});
                    while (!q.empty()) {
                        auto t = q.front(); q.pop();
                        flag[t.first][t.second] = true;
                        for (int d = 0; d < 4; ++d) {
                            int ni = t.first+go[d][0], nj = t.second+go[d][1];
                            if (ni < 0 || ni >= m || nj < 0 || nj >= n || board[ni][nj] == '.' || flag[ni][nj]) continue;
                            q.push({ni, nj});
                        }
                    }
                }
            }
        }
        return result;
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。

#### Method 3: Java
原理同C++，此处不再赘述。

 


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
