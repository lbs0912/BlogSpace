---
title: LeetCode Notes - 7
date: 2017-03-09 19:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---



* [LeetCode](https://leetcode.com)
* 本文包括 [LeetCode - 492. Construct the Rectangle](https://leetcode.com/problems/construct-the-rectangle/?tab=Description)，[LeetCode - 133. Clone Graph](https://leetcode.com/problems/clone-graph/?tab=Description)， [LeetCode - 452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/?tab=Description)， [LeetCode - 435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/?tab=Description) 和 [LeetCode -  138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/?tab=Description)5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。



<!--more-->

##  Construct the Rectangle(492)
### Description
* [LeetCode - 492. Construct the Rectangle](https://leetcode.com/problems/construct-the-rectangle/?tab=Description)
* For a web developer, it is very important to know how to design a web page's size. So, given a specific rectangular web page’s area, your job by now is to design a rectangular web page, whose length L and width W satisfy the following requirements:
	- The area of the rectangular web page you designed must equal to the given target area.
	- The width W should not be larger than the length L, which means L >= W.
	- The difference between length L and width W should be as small as possible.
* You need to output the length L and the width W of the web page you designed in sequence.
* Example

```
Input: 4
Output: [2, 2]
Explanation: The target area is 4, and all the possible ways to construct it are [1,4], [2,2], [4,1]. But according to requirement 2, [1,4] is illegal; according to requirement 3,  [4,1] is not optimal compared to [2,2]. So the length L is 2, and the width W is 2.
```
* Note
	- The given area won't exceed 10,000,000 and is a positive integer
	- The web page's width and length you designed must be positive integers.

### Analysis
* 思路1
若L和W相差最小，则L和W应该在`sqrt(num)`的附近。

  
 
### Slove
#### Method 1: C++
* 方法1：参照思路1。 (Cost 6ms.Beats 32.88%)

```cpp
class Solution {
public:
    vector<int> constructRectangle(int area) {
        vector<int> res(2);
        res[1] = (int)sqrt(area);  // w
        while(area%res[1]){
            res[1]--;
        }
        res[0]=area/res[1];  //L
        return res;
    }
};
```

* 方法2：参照思路1。计算`sqrt()`耗时较大，采用循环代替。 (Cost 3ms.Beats 37.03%)

```cpp
class Solution {
public:
    vector<int> constructRectangle(int area) {
        if (area <= 0) return vector<int> {};
        vector<int> res;
        int w = area;
        for (int i = 1; i * i <= area; ++i) {
            if (area % i == 0) w = i;
        }
        return vector<int> {area / w, w};
    }
};
```
#### Method 2: JavaScript
* 方法1：参照思路1。 (Cost 239ms.Beats 18.71%)

```javascript
/**
 * @param {number} area
 * @return {number[]}
 */
var constructRectangle = function(area) {
    var res= [];
    res[0] = Math.ceil(Math.sqrt(area));
    while((area%res[0])){
        res[0]++;  //L
    }
    res[1] = area/res[0];
    return res;
};
```
#### Method 3: Java
原理同C++，此处不再赘述。
 


##  Clone Graph(133)
### Description
* [LeetCode - 133. Clone Graph](https://leetcode.com/problems/clone-graph/?tab=Description)
* Clone an undirected graph. Each node in the graph contains a `label` and a list of its `neighbors`.

* OJ's undirected graph serialization:

Nodes are labeled uniquely.

We use `#` as a separator for each node, and `,` as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph `{0,1,2#1,2#2,2}`.

The graph has a total of three nodes, and therefore contains three parts as separated by `#`.
* First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
* Second node is labeled as 1. Connect node 1 to node 2.
* Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.

Visually, the graph looks like the following

```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```

* Tags: Depth-first Search, Graph

### Analysis
* 思路1：DFS
	- 首先，这道题目不能直接返回node。因为需要克隆，也就意味着要为所有的节点新分配空间。
	- 其次，分析节点的元素，包括`label`和`neighbors`两项。这意味着完成一个节点需要克隆一个`int`和一个`vector<UndirectedGraphNode *>`。
	- 根据参数中的node提取出其label值，然后用构造方法生成一个节点，这样就完成了一个节点的生成。
	- 然后，根据参数中的node提取其neighbors中的元素。只需要把这些元素加入刚生成的节点中，便完成了第一个节点的克隆。
	- 再次，neighbor元素怎么生成呢？同样需要克隆。这样我们就可以总结出递归的思路。
	- 最后，由于克隆要求每个节点只能有一个备份（其实也只能做到一个备份），所以，对于已经有开辟空间的节点，我们只需要返回已经存在的节点即可。那怎么做到不重复呢？HashMap！只要我们为每个节点开辟一个空间的时候把这个空间指针保存在HashMap中，便可以通过查找HashMap来确定当前要克隆的节点是否已经存在，不存在再新开辟空间。

### Slove
#### Method 1: C++
* 方法1：参照思路1，采用DFS。 (Cost 29ms. Beats 69.29%)

```cpp
class Solution {
public:
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        //labeled uniquely so label can be used as the key of map
        map<int, UndirectedGraphNode *> hashTable;
        return clone(node, hashTable);
    }
    UndirectedGraphNode * clone(UndirectedGraphNode *node, map<int, UndirectedGraphNode *> & hashTable){
        if(node == NULL){
            return NULL;
        }
        //表明这个节点已经创建了，所以没有必要重复创建，只要把已经创建的节点返回即可
        if(hashTable.find(node->label) != hashTable.end()){
            return hashTable[node->label];
        }
        //如果没有创建参数中节点，则创建，并添加到hashtable中
        UndirectedGraphNode * result = new UndirectedGraphNode(node->label);
        hashTable[node->label] = result;
        //clone所有的邻节点，并添加到刚clone好的节点的第二个元素vector中
        for(int i=0; i<node->neighbors.size();i++){
            UndirectedGraphNode * newnode = clone(node->neighbors[i], hashTable);
            result->neighbors.push_back(newnode);
        }    
        return hashTable[node->label];
        //或  return result;
    }
};
```

* 方法2：同方法1，在算法上进行优化。  (Cost 29ms. Beats 69.29%)

```cpp
class Solution {
public:
    unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> hash;
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
       if (!node) return node;
       if(hash.find(node) == hash.end()) { //未找到
           hash[node] = new UndirectedGraphNode(node -> label);
           for (auto x : node -> neighbors) {
                (hash[node] -> neighbors).push_back( cloneGraph(x) );
           }
       }
       return hash[node]; //若找到，直接返回
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。


##  Minimum Number of Arrows to Burst Balloons(452)
### Description
* [LeetCode - 452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/?tab=Description)
* There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.
* An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with `xstart` and `xend` bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.
* Example:

```
Input: [[10,16], [2,8], [1,6], [7,12]]
Output: 2
Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```
* Tags: Greedy

### Analysis
* 思路1 （排序+贪婪算法，子问题的最优解就是全局最优解）
	- 首先对数据进行排序，按照`pair`的第1个数字（气球起点坐标）升序排序；若相同，则按照第2个数字（气球终点坐标）升序排序。
	- 将打枪的位置设定在第1个气球的终点坐标处，标记为`end`值。
	- 若下一个气球的起点坐标小于end值，则修改end为当前end值和下一个气球的终点坐标中的较小值，即`end = min(end,points[i].second)`。
	- 若下一个气球的起点坐标大于end值，则需要重新打一枪，更新end值为下一个气球的终点坐标值。
 
### Slove
#### Method 1: C++
* 方法1：参照思路1。 (Cost 85ms.Beats 97.42%)

```cpp
class Solution {
public:
    int findMinArrowShots(vector<pair<int, int>>& points) {
        if(points.empty()){
            return 0;
        }
        int res = 0;
        int size = points.size();
        //数组排序
        sort(points.begin(),points.end());
        //贪婪算法
        int end = points[0].second; // 第1枪
        res++;
        for(int i=1;i<size;i++){
           
            if(end >= points[i].first){
                end = min(end,points[i].second); //修正打枪的位置
            }
            else{
                end = points[i].second;  //更新end值
                res++;
            }
        }
        return res;
    }
};
```

#### Method 2: JavaScript
* 方法1：参照思路1。 (Cost 179ms.Beats 100.00%)

注意JavaScript中数组的`sort()`方法是默认按照ASCII排序的。此种排序下，`10`是小于`2`的。因此，需要自己指定排序函数。
  
```javascript
/**
 * @param {number[][]} points
 * @return {number}
 */
var findMinArrowShots = function(points) {
    var res;
    var length = points.length;
    if(length === 0){
        return 0;
    }
    points.sort(function(a,b){
      if(a[0] === b[0]){
        return a[1]-b[1];
      }
      else{
        return a[0]-b[0];
      }
    });
    var end = points[0][1];  //第1枪
    res = 1;
    for(var i=1;i<length;i++){
        if(points[i][0]<= end){
            //修正end值
            end = Math.min(end,points[i][1]);
        }
        else{
            res++;
            end = points[i][1];  //下一枪位置 
        }
    }
    return res;
};
```

#### Method 3: Java
原理同C++，此处不再赘述。

##  Non-overlapping Intervals(435)
### Description
*  [LeetCode - 435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/?tab=Description)
*  Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
*  You may assume the interval's end point is always bigger than its start point.
*  Intervals like [1,2] and [2,3] have borders "touching" but they don't overlap each other.
*  Example 1

```
Input: [ [1,2], [2,3], [3,4], [1,3] ]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.
```
* Example 2:

```
Input: [ [1,2], [1,2], [1,2] ]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.

```
* Example 3

```
Input: [ [1,2], [2,3] ]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```
* Tags: Greedy

### Analysis
* 思路1（排序+贪婪算法）
	- 思路同[LeetCode - 452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/?tab=Description)中的思路1。
	- 首先对数据进行升序排序。排序时若调用`sort()`方法记得指定比较函数。
	- 遍历数组，采用贪婪算法思想，每次若要删除数据，都删除跨度最大的数据。
	- 指定一个end标志值，初始值为第1个数组的结束坐标。若end值小于下一个数组的起点坐标，则将end值更新为end和下一个数组终点坐标中的较小值，删除最大值对应的数组（贪婪思想）。若end值大于下一个数组的起点坐标，则更新end值为下一个数组的终点坐标。
	
### Slove
#### Method 1: C++
* 方法1：参照思路1。 (Cost 9ms. Beasts 56.29%)

```cpp
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
	int eraseOverlapIntervals(vector<Interval>& intervals) {
		int size = intervals.size();
		//匿名函数表达式
        auto comp = [](const Interval& i1, const Interval& i2) {
            if(i1.start == i2.start){
                return i1.start < i2.start;
            }
            else{
                  return i1.start < i2.start;
            }
        };
		sort(intervals.begin(), intervals.end(), comp);
		int res = 0, pre = 0;
		for (int i = 1; i < size; i++) {
			if (intervals[i].start < intervals[pre].end) {
				res++;
				if (intervals[i].end < intervals[pre].end) {
					pre = i; 
				} 
			}
			else {
				pre = i; 
			} 
		}
		return res;
	}
};
```

需要注意的是，`sort()`方法中的比较函数采用了[匿名函数表达式（lambda表达式）](https://zh.wikipedia.org/wiki/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0#C.2B.2B)。

C++中不允许嵌套定义函数（但允许匿名函数在某个函数内定义），只允许嵌套调用函数。因此，比较函数不能定义在`eraseOverlapIntervals`函数体内。比较函数不能是类的成员函数。因此，可以考虑将比较函数定义为匿名比较函数。匿名函数允许定义在某个函数体内。

> 比较函数不能是类的成员函数。 ——[C++ 定义实用比较函数注意点](http://www.sigmainfy.com/blog/c-compare-functions-notes.html)
 
 

#### Method 2: JavaScript
* 方法1：参照思路1。 (Cost 138ms. Beasts 34.38%)

```javascript
/**
 * Definition for an interval.
 * function Interval(start, end) {
 *     this.start = start;
 *     this.end = end;
 * }
 */
/**
 * @param {Interval[]} intervals
 * @return {number}
*/

var eraseOverlapIntervals = function(intervals) {
    var size = intervals.length;
    if(size === 0){
        return 0;
    }
    intervals.sort(function(a,b){
        if(a.start === b.start){
            return a.end - b.end;
        }
        else{
            return a.start - b.start;
        }
    });
    var res = 0;
    var pre = 0;
    var end = intervals[0].end;
    for (var i = 1; i < size; i++) {
        if (intervals[i].start < end) {
           if (intervals[i].end < end) {
                end = intervals[i].end; 
           }
           res++;
        }
        else {
           end = intervals[i].end; 
        } 
    }
    return res;
};
```

> JavaScript中数组排序为`myArr.sort(compareFunction)`。其中，`compareFunction(a,b)`根据返回值大于0或小于0的数字。例如，`return a-b;`。若值小于0，则将a排在b之前。
> 
> C++中数组排序为`sort(myArr,myArr+length,compareFunction)`。其中，`compareFunction(a,b)`根据返回值为布尔值，例如，`return a<b;`。若值为真，则a排在b之前。

#### Method 3: Java
原理同C++，此处不再赘述。

##   Copy List with Random Pointers(138)
### Description
* [LeetCode -  138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/?tab=Description)
* A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
* Return a deep copy of the list.

### Analysis
* 参考资料
	- [LeetCode 138 - Copy List with Random Pointer - 博客](http://yuanhsh.iteye.com/blog/2185974)

* 思路1
	- 解决思路同[LeetCode - 133. Clone Graph](https://leetcode.com/problems/clone-graph/?tab=Description)中的C++实现中的方法2。
	- 创建一个哈希表，若该链表已经存在，则直接返回；若不存在，则新建一个链表节点，并函数递归实现对`label`，`next`和`random`的复制。

* 思路2

	- 参考：[Copy List with Random Pointer 复制带有随机指针的链表](http://blog.csdn.net/tmylzq187/article/details/50913211)
	- Step 1：复制节点，并将拷贝后的节点放到原节点的后面。
		- Step 2：更新所有拷贝节点的random节点：`h.next.random = h.random.next`
		-  将原链表与复制链表断开。

<div align="center">
　　<img src="http://ol3kbaay9.bkt.clouddn.com/leetcode-138-2.png" />
</div>


![leetcode-138.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-138.png)

![leetcode-138-1.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-138-1.png)

### Slove

#### Method 1: C++
* 方法1：参考思路1，函数递归实现。 (Cost 65ms. Beasts 40.64%)

```cpp
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
	unordered_map<RandomListNode *, RandomListNode*> hashTable;
	RandomListNode *copyRandomList(RandomListNode *head) {
		if (head == NULL) {
			return NULL;
		}
		if (hashTable.find(head) == hashTable.end()) {
			hashTable[head] = new RandomListNode(head->label);  //创建新节点
			hashTable[head]->next = copyRandomList(head->next);
			hashTable[head]->random = copyRandomList(head->random);
		}
		return hashTable[head]; //若已经存在，则直接返回
	}
};
```

* 方法2：参考思路2。 (Cost 43ms. Beasts 98.45%)

```cpp
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
 
 class Solution {
public:
	RandomListNode *copyRandomList(RandomListNode *head) {
		if (head == NULL) return NULL;
		RandomListNode* current = NULL;
		RandomListNode* head_preserve = head;
		// step1 create add new node into original list  
		while (head != NULL) {
			current = new RandomListNode(head->label);
			current->next = head->next;
			head->next = current;
			head = head->next->next;
		}
		//step 2 populate random pointer of new list  
		head = head_preserve;
		while (head != NULL) {
			current = head->next;
			if (head->random != NULL) {
				current->random = head->random->next;
			}
			head = head->next->next;
		}
		// step3 seperate old list and new list  
		head = head_preserve;
		head_preserve = head->next;
		while (head != NULL) {
			current = head->next;
			head->next = head->next->next;
			head = head->next;
			if (head != NULL)
				current->next = head->next;
		}
		return head_preserve;
	}
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
* 方法1：参考思路2。 (Cost 2ms. Beasts 72.6%)

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
public RandomListNode copyRandomList(RandomListNode head) {  
    if(head == null) return null;  
    RandomListNode h = head;  
    while(h != null) {  
        RandomListNode copy = new RandomListNode(h.label);  
        RandomListNode next = h.next;  
        h.next = copy;  
        copy.next = next;  
        h = next;  
    }  
    h = head;  
    while(h != null) {  
        if(h.random != null)  
            h.next.random = h.random.next;  
        h = h.next.next;  
    }  
    h = head;  
    RandomListNode newHead = h.next;  
    while(h != null) {  
        RandomListNode copy = h.next;  
        h.next = copy.next;  
        h = h.next;  
        copy.next = h != null ? h.next : null;  
    }  
    return newHead;  
} 
}
```




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
