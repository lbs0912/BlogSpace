---
title: 网易2016实习研发工程师编程题
date: 2017-03-21 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,网易笔试]
keywords: 校招笔试编程题,网易笔试
---




* 本文主要对网易笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/1429468/summary)


<!--more-->


##  比较重量
### 题目
* 小明陪小红去看钻石，他们从一堆钻石中随机抽取两颗并比较她们的重量。这些钻石的重量各不相同。在他们们比较了一段时间后，它们看中了两颗钻石g1和g2。现在请你根据之前比较的信息判断这两颗钻石的哪颗更重。
给定两颗钻石的编号g1,g2，编号从1开始，同时给定关系数组vector,其中元素为一些二元组，第一个元素为一次比较中较重的钻石的编号，第二个元素为较轻的钻石的编号。最后给定之前的比较次数n。请返回这两颗钻石的关系，若g1更重返回1，g2更重返回-1，无法判断返回0。输入数据保证合法，不会有矛盾情况出现。
* 测试样例：
2,3,[[1,2],[2,4],[1,3],[4,3]],4
返回: 1


### 分析
 构造一个有向图（无权值），则该题目问题可以转换为判断是否有g1到g2的路径。采用Floyd算法求解。由于是无向图，因此，需要对Floyd算法进行必要的修改，若边的权值为1，则表示有边；若为0，则表示没有边。
 
```cpp
if ( dis[i][k] > 0 && dis[k][j] > 0) { 
	//若不等于0，则证明存在边
	dis[i][j] = dis[i][k] + dis[k][j];
}					
```

### 求解

```cpp
class Cmp {
public:
	int cmp(int g1, int g2, vector<vector<int> > records, int n) {
		int res = 0;
		//初始化
		int max_size = records[0][0];
		for (int i = 0; i < n; i++) {
			max_size = max(max_size, records[i][0]);
			max_size = max(max_size, records[i][1]);
		}
		vector<vector<int>> dis(max_size+1,vector<int>(max_size+1,0)); //初始化为0
		//读取边
		for (int i = 0; i < n; i++) {
			dis[records[i][0]][records[i][1]] = 1; //存在边，则赋值为1
		}
		//Floyd算法
		for (int k = 1; k <= max_size; k++) {
			for (int i = 1; i <= max_size; i++) {
				for (int j = 1; j <= max_size; j++) {
					if ( dis[i][k] > 0 && dis[k][j] > 0) { 
						//若不等于0，则证明存在边
						dis[i][j] = dis[i][k] + dis[k][j];
					}
				}
			}
		}
		//结果判断
		if (dis[g1][g2] != 0) {
			res = 1;
		}
		else if (dis[g2][g1] != 0) {
			res = -1;
		}
		return res;
	}
};
```

## 二叉树

### 题目
有一棵二叉树，树上每个点标有权值，权值各不相同，请设计一个算法算出权值最大的叶节点到权值最小的叶节点的距离。二叉树每条边的距离为1，一个节点经过多少条边到达另一个节点为这两个节点之间的距离。
给定二叉树的根节点root，请返回所求距离。

### 分析
* 参考资料：[二叉树求最大最小叶子节点距离](https://my.oschina.net/eager/blog/725020)，[二叉树（12）----查找两个节点最低祖先节点（或最近公共父节点等），递归和非递归](http://blog.csdn.net/beitiandijun/article/details/41970417)和[二叉树中权值最大的叶节点到权值最小的叶节点的距离](http://blog.csdn.net/yang20141109/article/details/51045464)。
* 注意是权值最大的叶子节点和权值最小的叶子节点，而不是根节点或父辈节点。
* 用2个变量标记2个节点的位置，再求出这两个节点的最低祖先节点（LCA）
* 最后求出根节点到它们的路径，相加即可。


### 求解

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/


class Tree {
private:
	TreeNode maxNode = TreeNode(INT_MIN);
	TreeNode minNode = TreeNode(INT_MAX);
public:
	void getMaxMin(TreeNode* root) {
		if (root == NULL) {
			return;
		}
		if (root->left == NULL && root->right == NULL) {
			if (root->val > maxNode.val) {
				maxNode.val = root->val;
			}
			else if (root->val < minNode.val) {
				minNode.val = root->val;
			}
		}
		getMaxMin(root->left);
		getMaxMin(root->right);
	}

	TreeNode* getLCA(TreeNode* root) {
		if (root == NULL) {
			return NULL;
		}
		if (root->val == maxNode.val || root->val == minNode.val) {
			return root;
		}
		TreeNode* leftNode = getLCA(root->left);
		TreeNode* rightNode = getLCA(root->right);
		if (leftNode == NULL) {
			return rightNode;
		}
		else if (rightNode == NULL) {
			return leftNode;
		}
		else {
			//左右子树均找到一个节点，则根节点为最近公共祖先
			return root;
		}
	}

	int getNodeDis(TreeNode* lcaNode, TreeNode node) {
		if (lcaNode == NULL) {
			return -1;
		}
		if (lcaNode->val == node.val) {
			return 0;
		}
		//3种情况，两个均在左子树，两个均在右子树，一左一右
		//所以不能用if...else.. if结构
		int distance = getNodeDis(lcaNode->left, node);
		if (distance == -1) {
			//左子树未查找到，则查找右子树
			distance = getNodeDis(lcaNode->right, node);
		}
		if (distance != -1) {
			return distance + 1;
		}
		return -1;
	}
	int getDis(TreeNode* root) {
		getMaxMin(root);
		TreeNode* lcaNode = getLCA(root);
		int a = getNodeDis(lcaNode, maxNode);
		int b = getNodeDis(lcaNode, minNode);
		return a + b;
	}


};

```


## 寻找第K大
### 题目
* 有一个整数数组，请你根据快速排序的思路，找出数组中第K大的数。
* 给定一个整数数组a,同时给定它的大小n和要找的K(K在1到n之间)，请返回第K大的数，保证答案存在。
* 测试样例s
[1,3,5,2,2],5,3
返回：2

### 分析
采用快速排序对数组排序即可。
 
### 求解 

```cpp
class Finder {
public:
	void swap(int &p, int &q)
	{
		int temp = p;
		p = q;
		q = temp;
	}
	int partition(vector<int> &a, int left, int right)
	{
		int storeIndex = left;
		//直接选择最右边的元素为基准元素
		int pivot = a[right];
		for (int i = left; i<right; i++) {
			if (a[i] < pivot) {
				swap(a[i], a[storeIndex]);
				storeIndex++;
			}
		}
		swap(a[right], a[storeIndex]);
		return storeIndex;
	}
	void quickSort(vector<int> &a, int left, int right) {
		if (left < right)
		{
			int pivotIndex = partition(a, left, right);
			quickSort(a, left, pivotIndex - 1);
			quickSort(a, pivotIndex + 1, right);
		}
	}
	int findKth(vector<int> a, int n, int K) {
		// write code here
		quickSort(a,0,n-1);
		return a[n-K];
	}
};
```

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 