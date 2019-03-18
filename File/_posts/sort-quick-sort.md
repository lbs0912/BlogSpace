---
title:  排序算法
date: 2017-03-20 15:35:26
categories: Algorithm
tags: [Sort,Algorithm]
keywords: Sort,Algorithm
toc: true
---



* 本文主要对几种场景的排序算法进行介绍和归纳。

<!--more-->

## 快速排序QuickSort
### 基本介绍

* 参考资料
	- [常见排序算法 - 快速排序 (Quick Sort)](http://bubkoo.com/2014/01/12/sort-algorithm/quick-sort/)
	- [快速排序（Quicksort）的Javascript实现](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)
	- [快速排序动画演示](http://jsdo.it/norahiko/oxIy/fullscreen)
	- [快速排序 | 维基百科](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)


![Sorting_quicksort_anim.gif](http://ol3kbaay9.bkt.clouddn.com/Sorting_quicksort_anim.gif)

[QuickSort](https://en.wikipedia.org/wiki/Quicksort)，即快速排序算法，使用的比较广泛，速度比较快。在平均状况下，排序`n`个项目要`O(nlogn)`次比较。在最坏状况下则需要`O(n^2)`次比较，但这种状况并不常见。事实上，快速排序通常明显比其他`O(nlogn)`算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来。

### 算法性能
* 数据结构：不定
* （最差）时间复杂度：	`O(n^2)`
* 最优时间复杂度：	`O(nlogn)`
* 平均时间复杂度：	`O(nlogn)`
* 空间复杂度：	根据实现的方式不同而不同

### 原理
快速排序使用**分治法（Divide and conquer）**策略，把一个序列（list）分为两个子序列（sub-lists）。

快速排序算法的步骤分为三步
* Step 1： 从数列中挑出一个元素，称为**"基准"（pivot）**
* Step 2：重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作。
* Step 3：对”基准”左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。


递归的最底部情形，是数列的大小是零或一，也就是永远都已经被排序好了。虽然一直递归下去，但是这个算法总会结束，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。


阅读[快速排序（Quicksort）的Javascript实现](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)和[常见排序算法 - 快速排序 (Quick Sort)](http://bubkoo.com/2014/01/12/sort-algorithm/quick-sort/)，了解应用实例。
 
### 算法实现
#### 非原地(In-place)分区的版本
阅读[快速排序（Quicksort）的Javascript实现](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)了解更多。

非原地(In-place)分区的版本中，需要额外的存储空间。在实际上的实现，也会极度影响速度和缓存的性能。下面给出JavaScript的实现。

>下面简单版本的缺点是，它需要`Ω(n)`的额外存储空间，也就跟归并排序一样不好。额外需要的存储器空间配置，在实际上的实现，也会极度影响速度和高速缓存的性能。 —— 摘自 [快速排序 | 维基百科](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)


```javascript
//javascript
var quickSort = function(arr) {
	//递归终止
	if (arr.length <= 1) {
		return arr; 
	}
	//确定基准坐标
　　var pivotIndex = Math.floor(arr.length / 2);
　　//基准元素
	var pivot = arr.splice(pivotIndex, 1)[0];
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　return quickSort(left).concat([pivot], quickSort(right));
};
```
#### 原地(In-place)分区的版本
阅读[常见排序算法 - 快速排序 (Quick Sort)](http://bubkoo.com/2014/01/12/sort-algorithm/quick-sort/)和[快速排序 | 维基百科](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)，了解更多。

![enter image description here](http://ol3kbaay9.bkt.clouddn.com/302px-Partition_example.svg.png)

原地(In-place)分区的版本中，不需要额外的存储空间。

分区是快速排序的主要内容，用伪代码可以表示如下

```
function partition(a, left, right, pivotIndex)
     pivotValue := a[pivotIndex]
     swap(a[pivotIndex], a[right]) // 把 pivot 移到結尾
     storeIndex := left
     for i from left to right-1
         if a[i] < pivotValue
             swap(a[storeIndex], a[i])
             storeIndex := storeIndex + 1
     swap(a[right], a[storeIndex]) // 把 pivot 移到它最後的地方
     return storeIndex // 返回 pivot 的最终位置
```
首先，把基准元素移到結尾（如果直接选择最后一个元素为基准元素，那就不用移动），然后从左到右（除了最后的基准元素），循环移动小于等于基准元素的元素到数组的开头，每次移动 storeIndex 自增 1，表示下一个小于基准元素将要移动到的位置。循环结束后 storeIndex 所代表的的位置就是基准元素的所有摆放的位置。所以最后将基准元素所在位置（这里是 right）与 storeIndex 所代表的的位置的元素交换位置。要注意的是，一个元素在到达它的最后位置前，可能会被交换很多次。

一旦我们有了这个分区算法，要写快速排列本身就很容易

```
procedure quicksort(a, left, right)
    if right > left
        select a pivot value a[pivotIndex]
        pivotNewIndex := partition(a, left, right, pivotIndex)
        quicksort(a, left, pivotNewIndex-1)
        quicksort(a, pivotNewIndex+1, right)
```

下面给出JavaScript的实现。

```cpp
//javascript
function quickSort(array) {
	// 交换元素位置
	function swap(array, i, k) {
		var temp = array[i];
		array[i] = array[k];
		array[k] = temp;
	}
	// 数组分区，左小右大
	function partition(array, left, right) {
		var storeIndex = left;        
		var pivot = array[right]; // 直接选最右边的元素为基准元素
		for (var i = left; i < right; i++) {
			if (array[i] < pivot) {
				swap(array, storeIndex, i);
				storeIndex++; // 交换位置后，storeIndex 自增 1，代表下一个可能要交换的位置
			}
		}
		swap(array, right, storeIndex); // 将基准元素放置到最后的正确位置上
		return storeIndex;
	}
	function sort(array, left, right) {
		if (left > right) {
			return;
		}
		var storeIndex = partition(array, left, right);
		sort(array, left, storeIndex - 1);
		sort(array, storeIndex + 1, right);
	}
	sort(array, 0, array.length - 1);
	return array;
}
```

下面给出C++实现的版本。

```cpp
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
```
 
 


## 网易2016笔试 - 寻找第K大
### 题目
* 有一个整数数组，请你根据快速排序的思路，找出数组中第K大的数。
* 给定一个整数数组a,同时给定它的大小n和要找的K(K在1到n之间)，请返回第K大的数，保证答案存在。
* 测试样例
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