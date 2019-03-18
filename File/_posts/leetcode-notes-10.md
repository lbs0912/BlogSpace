
---
title: LeetCode Notes - 10
date: 2017-05-16 16:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---






* [LeetCode](https://leetcode.com)
* 本文包括 [LeetCode - 84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/#/description)，[LeetCode - 307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/#/description)，[LeetCode - 303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/#/description)， [LeetCode - 200. Number of Islands](https://leetcode.com/problems/number-of-islands/#/description)和  [LeetCode - 130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/#/description)  5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。
* 本文中题目涉及线段树，二叉搜索树BST，二叉索引树BIT等数据结构，以及DFS，Union-Find算法。


<!--more-->


##  Largest Rectangle in Histogram(84)
### Description
* [LeetCode - 84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/#/description)
* Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![histogram.png](http://ol3kbaay9.bkt.clouddn.com/histogram.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![histogram_area.png](http://ol3kbaay9.bkt.clouddn.com/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.

* For example
Given heights = `[2,1,5,6,2,3]`,
return `10`.

* Tags: Stack, Array

### Analysis
* 思路1：暴力求解+分而治之  
	- 将问题分解，分而治之，先求解包含前1个直方图时候面积最大值，再求解包含前2个直方图时候面积最大值，在求解包含前3个直方图时候面积最大值。
	- 对于每个子问题，每个子问题中求解的最大面积都应该包括新增的矩形边界。例如，求解前2个直方图面积最大值时，需要求解只包含第2个直方图时面积最大值和包含第2个，第1个直方图时面积最大值。求解前3个直方图面积最大值时，需要求解只包含第3个直方图时面积最大值，包含第3个，第2个直方图时面积最大值和包含第3个，第2个，第1个直方图时面积最大值。
	- 很遗憾，该算法虽然正确，但是在LeetCode上提交会被作为`Time Limited`。
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


### Slove
#### Method 1: C++
* 方法1：参照思路2。(Cost 12ms. Beats 70.58% )

```cpp
class Solution {
public:
	int largestRectangleArea(vector<int>& heights) {
		stack<int> stk;
		int maxArea = 0;
		int count = 0;
		int length = heights.size();
		for (int i = 0; i<length; i++) {
			if (stk.empty() || stk.top() <= heights[i]) {
				stk.push(heights[i]);
			}
			else {
				while (!stk.empty() && stk.top() > heights[i]) {
					count++;
					maxArea = max(maxArea, stk.top()*count);
					stk.pop();
				}
				while (count) {
					stk.push(heights[i]);
					count--;
				}
				stk.push(heights[i]);
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
  
#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。
 





## Range Sum Query - Mutable(307)
### Description
* [LeetCode - 307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/#/description)
* Given an integer array `nums`, find the sum of the elements between indices `i` and `j` (i ≤ j), inclusive.
* The `update(i, val)` function modifies nums by updating the element at index i to val.
* Example

```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

* Note
	- The array is only modifiable by the update function.
	- You may assume the number of calls to update and sumRange function is distributed evenly.

### Analysis
* 思路1：暴力求解。
遍历数组，进行求和。求和操作`O(n)`，更新操作`O(1)`。
* 思路2：改进数组
创建另外一个数组来存储从下标`i`开始的元素之和。这样一来，给定区间之和可以用`O(1)`的时间计算，但是更新需要花费`O(n)`的时间。这种方法适用于需要大量查询而更新操作较少的场景。

* 思路3：线段树。`O(log n)`
	- 参考资料：[Segment Tree | Set 1 (Sum of given range)](http://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/) 和 [线段树 | 第1讲 给定区间求和](http://bookshadow.com/weblog/2015/08/13/segment-tree-set-1-sum-of-given-range/)
	- 使用线段树，更新操作和求和操所的复杂度都是`O(log n)`。
	- 线段树构建的时间复杂度为`O(n)`。总计有`2n-1`个节点，每一个节点在树构建过程中只被运算一次。
	- 查询的时间复杂度为`O(logn)`。在每一层至多处理4个节点，并且层的总数为`O(logn)`。
	- 更新的时间复杂度也是`O(logn)`。要更新一个叶子节点，我们每一层处理一个节点，并且层的总数为O(log n)。

* 思路4：二叉搜索树（树状数组）
	- 参考资料：[Binary Indexed Tree or Fenwick Tree](http://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/)
	- 树状数组可以处理的问题，都可以用线段树来处理。但是线段树处理的问题，树状数组并不一定能处理，即线段树的功能更强大。但树状数组消耗的空间更小，更易于编程实现。

> Using Binary Indexed Tree, we can do both tasks in `O(Logn)` time. **The advantages of Binary Indexed Tree over Segment are, requires less space and very easy to implement.**   --- [Binary Indexed Tree or Fenwick Tree](http://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/)

### Slove
#### Method 1: C++
*  方法1：使用二叉搜索树（树状数组）。Cost 93ms.   Beats 83.77%

```
// Note NumArray query index i, and BIT index j has i + 1 = j; So be clear about it.
// Cost 93ms.   Beats 83.77%

class NumArray {
private:
	vector<int> bit;
	vector<int> arr;
	int n;
public:
	int lowbit(int x) {
		return x&(-x);
	}
	void build()
	{
		for (int i = 1; i <= n; i++)
		{
			bit[i] = arr[i-1];
			for (int j = i - 1; j>i - lowbit(i); j--)
				bit[i] += arr[j-1];
		}
	}
	NumArray(vector<int> nums) {
		n = nums.size();
		for (int i = 0; i < n; i++) {
			arr.push_back(nums[i]);
		}
		bit.resize(n+1);
		build();
	}	
	// delata = new_val - arr[i] 
	void edit(int i, int delta)
	{
		for (int j = i; j <= n; j += lowbit(j)) {
			bit[j] += delta;
		}
	}
	void update(int i, int val) {
		int delta = val - arr[i];
		arr[i] = val;
		edit(i + 1, delta);
	}
	int getSum(int k)
	{
		int ans = 0;
		for (int i = k; i > 0; i -= lowbit(i)) {
			ans += bit[i];
		}
		return ans;
	}
	int sumRange(int i, int j) {
		return getSum(j+1) - getSum(i);
	}
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```



*  方法2：使用线段树（数组实现线段树）。树状数组可以处理的问题，都可以用线段树来处理。但是线段树处理的问题，树状数组并不一定能处理，即线段树的功能更强大。但树状数组消耗的空间更小，更易于编程实现。Cost 129ms.   Beats 48.27%

> Using Binary Indexed Tree, we can do both tasks in `O(Logn)` time. **The advantages of Binary Indexed Tree over Segment are, requires less space and very easy to implement.**   --- [Binary Indexed Tree or Fenwick Tree](http://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/)
> 

```
class NumArray {
private:
	vector<int> arr;
	vector<int> st;
	int n;
public:
	int getMid(int left, int right) {
		return left + (right - left) / 2;
	}
	int constructST(int left,int right,int index) {
		if (left == right) {
			st[index] = arr[left];
		}
		else {
			int mid = getMid(left,right);
			st[index] = constructST(left,mid,2*index+1) +
					constructST(mid+1,right,2*index+2);
		}
		return st[index];
	}
	NumArray(vector<int> nums) {
		n = nums.size();
		if (n == 0) {
			return;
		}
		for (int i = 0; i < n; i++) {
			arr.push_back(nums[i]);
		}
		int depth = (int)ceil(log2(n));
		int max_size = (2 << depth) - 1;
		st.resize(max_size);
		constructST(0,n-1,0);
	}
	void updateUtil(int i,int diff,int left,int right,int index) {
		if (left > i || right < i) {
			return;
		}
		else {
			st[index] += diff;
			if (left != right) {
				int mid = getMid(left, right);
				updateUtil(i, diff, left, mid, 2 * index + 1);
				updateUtil(i, diff, mid + 1, right, 2 * index + 2);
			}
		}
	}
	void update(int i, int val) {
		if(i < 0 || i >= n){
			return;
		}
		else {
			int diff = val - arr[i];
			arr[i] = val;
			updateUtil(i,diff,0,n-1,0);
		}
	}
	int getSum(int lower,int upper,int left,int right,int index) {
		int res = 0;
		if (right < lower || left > upper) {
			res = 0;
		}
		else if (lower <= left && right <= upper) {
			res = st[index];
		}
		else {
			int mid = getMid(left,right);
			res = getSum(lower, upper, left, mid, 2 * index + 1) +
					getSum(lower,upper,mid+1,right,2*index+2);
		}
		return res;
	}
	int sumRange(int i, int j) {
		if (i > j) {
			return -1;  //区间错误
		}
		else {
			return getSum(i, j, 0, n - 1, 0);
		}
	}
};


/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```


*  方法3：使用线段树（结构体实现）。树状数组可以处理的问题，都可以用线段树来处理。但是线段树处理的问题，树状数组并不一定能处理，即线段树的功能更强大。但树状数组消耗的空间更小，更易于编程实现。Cost 165ms.   Beats 28.71%


```cpp
struct SegmentTreeNode {
	int start, end, sum;
	SegmentTreeNode* left;
	SegmentTreeNode* right;
	SegmentTreeNode(int a, int b) :start(a), end(b), sum(0), left(nullptr), right(nullptr) {}
};
class NumArray {
	SegmentTreeNode* root;
public:
	NumArray(const vector<int> &nums) {
		int n = nums.size();
		root = buildTree(nums, 0, n - 1);
	}
	void update(int i, int val) {
		modifyTree(i, val, root);
	}
	int sumRange(int i, int j) {
		return queryTree(i, j, root);
	}
	SegmentTreeNode* buildTree(const vector<int> &nums, int start, int end) {
		if (start > end) return nullptr;
		SegmentTreeNode* root = new SegmentTreeNode(start, end);
		if (start == end) {
			root->sum = nums[start];
			return root;
		}
		int mid = start + (end - start) / 2;
		root->left = buildTree(nums, start, mid);
		root->right = buildTree(nums, mid + 1, end);
		root->sum = root->left->sum + root->right->sum;
		return root;
	}
	int modifyTree(int i, int val, SegmentTreeNode* root) {
		if (root == nullptr) return 0;
		int diff;
		if (root->start == i && root->end == i) {
			diff = val - root->sum;
			root->sum = val;
			return diff;
		}
		int mid = (root->start + root->end) / 2;
		if (i > mid) {
			diff = modifyTree(i, val, root->right);
		}
		else {
			diff = modifyTree(i, val, root->left);
		}
		root->sum = root->sum + diff;
		return diff;
	}
	int queryTree(int i, int j, SegmentTreeNode* root) {
		if (root == nullptr) return 0;
		if (root->start == i && root->end == j) return root->sum;
		int mid = (root->start + root->end) / 2;
		if (i > mid) return queryTree(i, j, root->right);
		if (j <= mid) return queryTree(i, j, root->left);
		return queryTree(i, mid, root->left) + queryTree(mid + 1, j, root->right);
	}
};


/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */

```

#### Method 2: JavaScript
原理同C++，此处不再赘述。

#### Method 3: Java
原理同C++，此处不再赘述。
 








##  Range Sum Query - Immutable(303)
### Description
* [LeetCode - 303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/#/description)
* Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.
* Example:
Given nums = [-2, 0, 3, -5, 2, -1]
sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
* Note
	- You may assume that the array does not change.
	- There are many calls to sumRange function.

### Analysis
* 思路1：采用树状数组（二叉索引树）
 参考[LeetCode - 307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/#/description)中的树状数组分析。
* 思路2：采用线段树。
  参考[LeetCode - 307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/#/description)中的树状数组分析。
 * 思路3： 改进数组
The idea is fairly straightforward: create an array `accu` that stores the accumulated sum for nums such that `accu[i] = nums[0] + ... + nums[i - 1]` in the initializer of NumArray. Then just return `accu[j + 1]` - `accu[i]` in sumRange. 

### Slove
#### Method 1: C++
* 方法1：参照思路1，采用树状数组（二叉索引树）。Cost 182ms. Beats 66.26%

```cpp
class NumArray {
private:
vector<int> arr;
vector<int> bit;
int n;
public:
    int lowbit(int x){
        return x&(-x);
    }
    void buildBIT(){
        for(int i=1;i<n+1;i++){
            bit[i] = arr[i-1];
            for(int j=i-1;j>i-lowbit(i);j--){
                bit[i] += arr[j-1];
            }
        }
    }
    NumArray(vector<int> nums) {
      n=nums.size();
      if(n == 0){
          return;
      }
      for(int i=0;i<n;i++){
          arr.push_back(nums[i]);
      }
      bit.resize(n+1,0);
      buildBIT();
    }
    
    int getSum(int end){
        int res = 0;
        while(end){
            res += bit[end];
            end -= lowbit(end);
        }
        return res;
    }
    
    int sumRange(int i, int j) {
        return getSum(j+1)- getSum(i);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```


* 方法2：参照思路2，采用线段树。Cost 236ms. Beats 18.25%

```cpp
class NumArray {
private:
	vector<int> arr;
	vector<int> st;
	int n;
public:
	int getMid(int left, int right) {
		return left + (right - left) / 2;
	}
	int constructST(int left,int right,int index) {
		if (left == right) {
			st[index] = arr[left];
		}
		else {
			int mid = getMid(left,right);
			st[index] = constructST(left,mid,2*index+1) +
					constructST(mid+1,right,2*index+2);
		}
		return st[index];
	}
	NumArray(vector<int> nums) {
		n = nums.size();
		if (n == 0) {
			return;
		}
		for (int i = 0; i < n; i++) {
			arr.push_back(nums[i]);
		}
		int depth = (int)ceil(log2(n));
		int max_size = (2 << depth) - 1;
		st.resize(max_size);
		constructST(0,n-1,0);
	}


	int getSum(int lower,int upper,int left,int right,int index) {
		int res = 0;
		if (right < lower || left > upper) {
			res = 0;
		}
		else if (lower <= left && right <= upper) {
			res = st[index];
		}
		else {
			int mid = getMid(left,right);
			res = getSum(lower, upper, left, mid, 2 * index + 1) +
					getSum(lower,upper,mid+1,right,2*index+2);
		}
		return res;
	}

	int sumRange(int i, int j) {
		if (i > j) {
			return -1;  //区间错误
		}
		else {
			return getSum(i, j, 0, n - 1, 0);
		}

	}
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```


* 方法3：参照思路3，采用树状数组（二叉索引树）。Cost 265ms. Beats 13.06%

```cpp
class NumArray {
public:
    NumArray(vector<int> nums) {
        accu.push_back(0);
        for (int num : nums)
            accu.push_back(accu.back() + num);
    }

    int sumRange(int i, int j) {
        return accu[j + 1] - accu[i];
    }
private:
    vector<int> accu;
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```
#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。


## Number of Islands(200)
### Description
* [LeetCode - 200. Number of Islands](https://leetcode.com/problems/number-of-islands/#/description)
* Given a 2d grid map of `1`s (land) and `0`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
* Example 1

```
//input
11110
11010
11000
00000

Answer: 1
```

* Example 2:

```
//input
11000
11000
00100
00011

Answer: 3
```

* Tags: Depth-first Search, Breadth-first Search, Union Find

### Analysis
* 思路1：DFS

参考[LeetCode Solution](https://discuss.leetcode.com/topic/13248/very-concise-java-ac-solution)了解更多。

遍历grid数组，遇到`1`则将小岛计数值count加1，同时进行深度优先搜索DFS。

DFS过程中，先将该小岛的值置为0，在遍历该小岛周围的4个小岛的情况，遇到值为1的情况，则将值置于0，并函数递归，再次遍历该小岛周围4个岛屿的情况，直至递归终止。

DFS过程中，注意将遍历过的小岛的值置于0，避免下次遍历时候重复计数。也可以引入一个visited数组，标记是否被访问，但这样会引入额外的存储空间。
 
### Slove
#### Method 1: C++

* 方法1：参考思路1，DFS解决。Cost 6ms. Beats 35.23%.

**注意对测试用例为空的判断，否则程序会报错。并且要先判断row是否为0，再创建col变量。row=0时，无法创建col变量。**

```
int row = grid.size();
if(row == 0){
    return 0;
}
int col = grid[0].size();
```
 
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int row = grid.size();
        if(row == 0){
            return 0;
        }
        int col = grid[0].size();
        int count = 0;
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(grid[i][j] == '1'){
                    count++;
                    DFSSearch(grid,i,j,row,col);
                }
            }
        }
        return count;
    }
    void DFSSearch(vector<vector<char>>& grid,int i,int j,int row, int col){
        if(i<0 || j <0 || i >= row || j >= col || grid[i][j] != '1'){
            return;
        }
        grid[i][j] = '0';
        DFSSearch(grid,i-1,j,row,col);
        DFSSearch(grid,i,j-1,row,col);
        DFSSearch(grid,i+1,j,row,col);
        DFSSearch(grid,i,j+1,row,col);
    }
};
```
#### Method 2: JavaScript

* 方法1：参考思路1，DFS解决，原理同C++。Cost 109ms. Beats 84.26%.

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    var row = grid.length;
    if(row == 0){
        return 0;
    }
    var col = grid[0].length;
    var count = 0;
    for(var i=0;i<row;i++){
        for(var j=0;j<col;j++){
            if(grid[i][j] == '1'){
                count++;
                DFSSearch(grid,i,j);
            }
        }
    }
    return count;
};


var  DFSSearch = function(grid,i,j){
    if(i<0 || j<0 || i>=grid.length || j>= grid[0].length || grid[i][j] != '1'){
        return;
    }  
    grid[i][j] = '0';
    DFSSearch(grid,i-1,j);
    DFSSearch(grid,i,j-1);
    DFSSearch(grid,i+1,j);
    DFSSearch(grid,i,j+1);
    
};
```

#### Method 3: Java
原理同C++，此处不再赘述。



## Surrounded Regions(130)
### Description
* [LeetCode - 130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/#/description) 
* Given a 2D board containing `X` and `O` (the letter O), capture all regions surrounded by `X`.
* A region is captured by flipping all `O`s into `X`s in that surrounded region.
* For example

```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be

```
X X X X
X X X X
X X X X
X O X X
``` 

* Tags: Depth-first Search, Breadth-first Search, Union Find

### Analysis
* 思路1： DFS

参考[LeetCode Solution](https://discuss.leetcode.com/topic/45119/concise-12ms-c-dfs-solution)了解更多。

第1步，遍历board矩阵的边界，若发现`O`值，则将其替换为一个标记值，如`D`。

第2步，进行DFS。搜索该`O`值的四周元素，若发现`O`值，仍替换为为`D`。函数递归实现DFS。

第3步，遍历board矩阵，若发现`O`值，直接替换为`X`。若发现`D`值，则替换为`O`值。

```cmake
X X X X          X X X X            X X X X
O X O O          D X D D            O X O O
X O X X  --->    X O X X    --->    X X X X
O X O X          D X D X            O X O X
```


* 思路2：Union-Find算法

参考[LeetCode Solution](https://discuss.leetcode.com/topic/1944/solve-it-using-union-find/4)了解更多。
 
 使用Union-Find算法，初始化元素个数为board的元素个数+1，即`row*col+1`个。多出来的一个元素（下标为`row*col`）用来存储边界为`O`的集合，此处称为虚拟节点(dummy)。

遍历board矩阵，若为边界元素且值为O，则将其归并到dummy虚拟节点。若不为边界元素且值为0，则将其归并到周围为O的集合中。

再次遍历board矩阵，若元素值为O且属于dummy虚拟节点，不做处理。若元素值为O且不属于dummy虚拟节点，则将其替换为X。
 
 
 
 
### Slove
#### Method 1: C++
* 方法1：参考思路1，DFS。Cost 6ms. Beats 93.57%.

```cpp
class Solution {
public:
    void solve(vector<vector<char>>& board) {
		int row = board.size();
		if (row == 0) return;
		int col = board[0].size();
		if (col == 0) return;

		for (int i = 0; i<row; i++) {
			//第1列
			if (board[i][0] == 'O') {
				DFSSearch(board, i, 0, row, col);
			}
			//最后1列
			if (board[i][col - 1] == 'O') {
				DFSSearch(board, i, col - 1, row, col);
			}
		}
		for (int j = 0; j<col; j++) {
			//第1行
			if (board[0][j] == 'O') {
				DFSSearch(board, 0, j, row, col);
			}
			//最后1行
			if (board[row - 1][j] == 'O') {
				DFSSearch(board, row - 1, j, row, col);
			}
		}
		for (int i = 0; i<row; i++) {
			for (int j = 0; j<col; j++) {
				if (board[i][j] == 'O') {
					board[i][j] = 'X';
				}
				else if (board[i][j] == 'D') {
					board[i][j] = 'O';
				}
			}
		}
	}
	
	void DFSSearch(vector<vector<char>>& board, int i, int j, int row, int col) {
		//注意递归的实现
	    if(board[i][j] == 'O'){
	        board[i][j] = 'D';
	        if(i>1){
	            DFSSearch(board, i - 1, j, row, col);
	        }
	        if(j>1){
	            DFSSearch(board, i, j - 1, row, col);
	        }
	        if(i+1<board.size()){
	            DFSSearch(board, i + 1, j, row, col);
	        }
	        if(j+1<board[0].size()){
	            DFSSearch(board, i, j + 1, row, col);
	        }
	   }
	}
};
```

**需要注意的是，由于函数递归会消耗栈空间，因此，应该尽可能的减少函数递归。**

```cpp
void DFSSearch(vector<vector<char>>& grid,int i,int j){
	if(i<0 || j <0 || i >= board.size() || j >= board[0].size() || board[i][j] != 'O'){
	    return;
    }
    board[i][j] = 'D';
    DFSSearch(grid,i-1,j);
    DFSSearch(grid,i,j-1);
    DFSSearch(grid,i+1,j);
    DFSSearch(grid,i,j+1);
}
```
上述递归函数中，先进行递归，再判断递归条件是否合理。虽然上述递归函数可以正确实现功能，在本地运行正确，但是在LeetCode上运行会显示运行超时（当测试数据很大时，多次递归，会运行超时）。**正确的处理方法是，先判断递归条件是否合理，然后再进行递归，减少不必要的递归。**

```cpp
void DFSSearch(vector<vector<char>>& board, int i, int j, int row, int col) {
	//注意递归的实现
	if(board[i][j] == 'O'){
		board[i][j] = 'D';
	    if(i>1){
		    DFSSearch(board, i - 1, j, row, col);
	    }
	    if(j>1){
	        DFSSearch(board, i, j - 1, row, col);
	    }
	    if(i+1<board.size()){
	        DFSSearch(board, i + 1, j, row, col);
	    }
	    if(j+1<board[0].size()){
	        DFSSearch(board, i, j + 1, row, col);
	    }
	}
}
```

 * 方法2：参考思路2，Union-Find算法。Cost 9ms. Beats 39.06%.

```cpp
class UF{
private:
    int* id;  //父辈元素 id[i] = parent of i
    //子树层级（高度） 
    //rank[i] = rank of subtree rooted at i (cannot be more than 31)
    int* rank; 
    int count;    // 元素的总数
public:
    UF(int N)
    {
        count = N;
        id = new int[N];
        rank = new int[N];
        for (int i = 0; i < N; i++) {
            id[i] = i;
            rank[i] = 0;
        }
    }
    ~UF()
    {
        delete [] id;
        delete [] rank;
    }

    int find(int p) {
        if(p == id[p]){
           return p;
        }
        else{
           return find(id[p]);
        }
    }
    /*
    int getCount() {
        return count;
    }
    */
    bool connected(int p, int q) {
        return find(p) == find(q);
    }
    void connect(int p, int q) {
        int i = find(p);
        int j = find(q);
        if (i == j) return;
        //将矮的树合并到高的树中
        if (rank[i] < rank[j]) id[i] = j;
        else if (rank[i] > rank[j]) id[j] = i;
        else {
            //子树层级相等
            id[j] = i;
            rank[i]++;
        }
        count--;
    }
};
class Solution{
public:
    void solve(vector<vector<char>> &board){
        int row = board.size();
        if(row == 0) return;  //board为空的情况
        int col = board[0].size();
        //创建并查集，包含row*col+1个元素
        //最后一个元素（下标为row*col）用于存放边界O元素的集合
        UF uf = UF(row*col+1); 

        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                //board边界出现的O值，将其合并到第row*col节点处
                if(((i==0) || (j==0) || (i==row-1) || (j==col-1)) 
                    && (board[i][j]=='O')){
                    uf.connect(i*col+j,row*col);

                }
                else if(board[i][j]=='O'){
                    //非边界出现的O元素，将其合并到周围的O元素中
                    if(board[i-1][j] == 'O'){
                        uf.connect(i*col+j,(i-1)*col+j);
                    }
                    if(board[i+1][j] == 'O'){
                        uf.connect(i*col+j,(i+1)*col+j);
                    }
                    if(board[i][j-1] == 'O'){
                        uf.connect(i*col+j,i*col+(j-1));
                    }
                    if(board[i][j+1] == 'O'){
                        uf.connect(i*col+j,i*col+(j+1));
                    }
                }
            }
        }

        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(!uf.connected(i*col+j,row*col)){ 
                    //如果节点不是和边界O节点一个集合
                    board[i][j] = 'X';
                }
            }
        }

    }
};
```

 
#### Method 2: JavaScript
#### Method 3: Java

## Union Find算法
* 并查集：参见[维基百科](https://zh.wikipedia.org/wiki/%E5%B9%B6%E6%9F%A5%E9%9B%86)了解更多。
* Union-Find Algorithm 联合-查找算法
* 参考资料
	- [普林斯顿算法 - 并查集（union-find算法）](https://libhappy.com/2016/03/algs-1.3/)
	- [Union-find | 简书](http://www.jianshu.com/p/b5b8d488266e)
	- [有一种算法叫做“Union-Find”？](http://www.cnblogs.com/SeaSky0606/p/4752941.html)
	- [Union-find算法介绍 | CSDN](http://blog.csdn.net/dm_vincent/article/details/7655764)

### 算法基本介绍
并查集(Union Find Set)，也称为不相交集数据结构(Disjointed Set Data Structure)。简单地讲，并查集维护了一列互不相交的集合S1、S2、S3、…，支持查找(Find)与合并(Union)两种操作。
* find
找到元素所在的集合，通常返回该集合的代表元(representative)。元素a与元素b是否属于同一个集合，只要判断find(a)与find(b)是否相等。
* union
将两个集合合并为一个集合。将元素a与元素b所在的集合合并为一个集合，使用`union(a,b)`。

### 原理实现
**`UnionFindSet`用树表示集合，所谓维护一列互不相交的集合，也就是有许多棵树。每棵树的元素属于一个集合，树根的元素就是集合的代表元。所以，`UnionFindSet`实际上维护了一个多棵树构成的森林。**
* `find(x)`就是找`x`所在的树的树根
* `union(x, y)`就是合并`x`和`y`所在的树

#### 算法实现 - UnionFindSet定义

```cpp
class UnionFindSet {
public:
	UnionFindSet(int n);
    int Find(int x);
    void Union(int x, int y);
private:
    std::vector<int> parent;
};
```
与常见的树形数据结构不同，并查集的链接关系不是从parent指向child，而是**从child指向parent，这个链接信息保存在parent数组里。**
* 若`parent[x] == x`，则x就是x所在树的树根
* 若parent[x] != x，则x是parent[x]的子节点


#### 算法实现 - Find方法

```cpp
int UnionFindSet::Find(int x) {
	if (parent[x] == x)
	    return x;
    else
        return Find(parent[x]);
}
```
#### 算法实现 - 构造函数实现
构造函数`UnionFindSet(int n)`表示最初有n个互不相交的集合，代表元分别为`0、1、2、…(n-1)`。

```cpp
UnionFindSet::UnionFindSet(int n) {
	parent.resize(n);
    for (int i = 0; i < n; ++i) {
        parent[i] = i;
    }
}
```

#### 算法实现 - Union方法实现

```cpp
void UnionFindSet::Union(int x, int y) {
    int root_x = Find(x);
    int root_y = Find(y);
    if (root_x == root_y)
        return;
    parent[root_y] = root_x;
}
```
要合并两棵树，只要把一棵根节点的parent设为另一棵树的根节点。
### 算法进阶
关于优化合并，压缩路径等进阶使用，参考上述资料。
 




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
