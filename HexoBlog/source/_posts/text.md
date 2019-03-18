---
title: LeetCode Notes - 8
date: 2017-03-10 14:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
password: 123456
comments: false
---


* [LeetCode](https://leetcode.com)
* 本文包括 [LeetCode - 268. Missing Number](https://leetcode.com/problems/missing-number/?tab=Description)，[LeetCode - 41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)，[LeetCode- 141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/?tab=Description)， [LeetCode- 142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)和 [LeetCode - 287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/?tab=Description) 5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。


<!--more-->

##   Missing Number(268)
### Description
* [LeetCode - 268. Missing Number](https://leetcode.com/problems/missing-number/?tab=Description)
* Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.
* Example
Given `nums = [0, 1, 3]`,  return `2`.
* Note
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?
* Tags: Bit Manipulation

### Analysis
* 思路1：位操作
	- 使用异或进行处理。`num XOR num = 0`
	- 使用for循环，将`num`和循环制`i`都进行异或处理。

### Slove

#### Method 1: C++
* 方法1：参考思路1。(Cost 29ms. Beats 32.82%)

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int res = nums.size();
        int size = res;
        for(int i=0;i<size;i++){
            res ^= nums[i];
            res ^= i;
        }
        return res;
    }
};

```

#### Method 2: JavaScript

原理同C++，此处不再赘述。
 
#### Method 3: Java
原理同C++，此处不再赘述。



## First Missing Positive(41)
### Description
* [LeetCode - 41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
* Given an unsorted integer array, find the first missing positive integer.
* For example

```
Given [1,2,0] return 3,
and [3,4,-1,1] return 2.
```
* Your algorithm should run in `O(n)` time and uses constant space.

### Analysis
* 思路1
	- 对于正整数A[i]，如果将它放在数组中满足A[i]=i+1的位置，那么如果当某个位置不满足A[i]==i+1时，则i为第一个不出现的正整数。
	- 遍历数组。当遇到小于n（n为数组大小）的正整数，如果它满足A[i]=i+1,则跳过，i++；如果不满足则将它交换它属于它的位置，即swap(A[i],A[A[i]-1])；
	- 当遇到小于0或者大于n的数，或者需交换的位置已经有了满足条件的值即A[i]==A[A[i]-1]（数组中有重复数字的时候会有这种情况），则跳过，i++。因为没有合适的位置可以跟它们交换。
	- 再次遍历数组，如果A[i]！=i+1，则i为第一个不出现的正整数。
	


### Slove

#### Method 1: C++
* 方法1：参照思路1，使用快慢两个指针。(Cost 6ms. Beats 16.14%)

```cpp
class Solution {
public:
    //引用值传递 ！！
    inline void swap(int &a,int &b){
        int tmp;
        tmp=a;
        a=b;
        b=tmp;
    }
    int firstMissingPositive(vector<int>& nums) {
        int n=nums.size();
        //if(n==0) return 1;
        int i=0;
        while(i<n){
            if(nums[i]==i+1 || nums[i]==nums[nums[i]-1] || nums[i]<=0 || nums[i]>n){
                i++;
            }
            else{
                swap(nums[i],nums[nums[i]-1]);
            }
        }
        
        for(i=0;i<n;i++){
            if(nums[i]!=i+1)
                break;
        }
        return i+1;
    }
};
```
#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。
 
##  Linked List Cycle(141)
### Description
* [LeetCode- 141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/?tab=Description)
* Given a linked list, determine if it has a cycle in it.
* Follow up: Can you solve it without using extra space?
* Tags: Linked List, Two Pointers

### Analysis
* 思路1: 快慢指针，O(1)空间复杂度，O(N)时间复杂度
	- 参考资料：[O(1) Space Solution](https://leetcode.com/problems/linked-list-cycle/?tab=Solutions)，[LeetCode 141/142 - Linked List Cycle](http://www.cnblogs.com/AndyJee/p/4463998.html)和[CSDN Blog - Linked List Cycle](http://blog.csdn.net/ebowtang/article/details/50507131)
	- Use two pointers, walker and runner.
	- walker moves step by step. runner moves two steps at time.
	- If the Linked List has a cycle walker and runner will meet at some point.

### Slove
#### Method 1: C++
* 方法1：参照思路1，使用快慢两个指针。(Cost 9ms. Beats 44.72%)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head == NULL){
            return false;
        }
        ListNode *walker = head; //moves one step each time
        ListNode *runner = head; //moves two step each time
        while(runner->next != NULL && runner->next->next != NULL){
            walker = walker->next;
            runner = runner->next->next;
            if(walker == runner){
                return true;
            }
        }
        return false;
    }
};
```
#### Method 2: JavaScript
* 方法1：参照思路1，使用快慢两个指针。(Cost 109ms. Beats 39.49%)

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    if(head === null) {
        return false;
    }
    var walker = new ListNode();
    var runner = new ListNode();
    walker = head;
    runner = head;
    while(runner.next!==null && runner.next.next!==null) {
        walker = walker.next;
        runner = runner.next.next;
        if(walker === runner) return true;
    }
    return false;
};
```
#### Method 3: Java
* 方法1：参照思路1，使用快慢两个指针。(Cost 1ms. Beats 8.3%)

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        ListNode walker = head;
        ListNode runner = head;
        while(runner.next!=null && runner.next.next!=null) {
            walker = walker.next;
            runner = runner.next.next;
            if(walker==runner) return true;
        }
        return false;
    }
}
```

##  Linked List Cycle II(142)
### Description
* [LeetCode- 142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
* Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
* Note: Do not modify the linked list.
* Follow up: Can you solve it without using extra space?
### Analysis
* 思路1
在 [LeetCode- 141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/?tab=Description) 的基础上完成本题。`141. Linked List Cycle `题目中已经实现了对是否有环的判断，下面需要实现的是对环起点的判断和环长度的计算。
* 参考资料： [LeetCode 141/142 - Linked List Cycle](http://www.cnblogs.com/AndyJee/p/4463998.html)
	- 设链表起点距离环的起点距离为`a`，圈长为`n`，当walker和runner相遇时，相遇点距离环起点距离为`b`，此时b已绕环走了`k`圈，则
	- walker走的距离为`a+b`，步数为`a+b`；
	- runner速度为walker的两倍，runner走的距离为`2*(a+b)`，步数为a+b;
	- runner走的距离为`a+b+k*n=2*(a+b)`，从而`a+b=k*n`，`a=k*n-b`；
	- 因此有，当walker走a步，runner走(k*n-b)步。当k=1时，则为(n-b)步；

![leetcode-142.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-142.png)
* 环的起点
	-  令walker返回链表初始头结点，runner仍在相遇点。此时，令walker和runner每次都走一步距离。
	-  当walker和runner相遇时，二者所在位置即环的起点。
	-  walker走a步，到达环的起点；runner初始位置为2(a+b)，走了a步之后，即kn-b步之后，所在位置为`2(a+b)+kn-b=2a+b+kn= a+(a+b)+kn=a+2kn`。因此，runner位置是环的起点。
* 环的长度
	-  在上述判断环的起点的基础上，求解环的长度。
	-  当walker和runner相遇时，二者所在位置即环的起点。此后，再让walker每次运动一步。
	-  walker走n步之后，walker和runner再次相遇。walker所走的步数即是环的长度。

### Slove
#### Method 1: C++
* 方法1：参照思路1。(Cost 9ms. Beats 46.33%)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
       if(head == NULL){
            return NULL;
        }
        bool hasCycle = false;
        ListNode *walker = head; //moves one step each time
        ListNode *runner = head; //moves two step each time
        while(runner->next != NULL && runner->next->next != NULL){
            walker = walker->next;
            runner = runner->next->next;
            if(walker == runner){
                hasCycle = true;
                break;  //跳出循环
            }
        }
        if(hasCycle == true){
            walker = head;
            while(walker != runner){
                walker = walker->next;
                runner = runner->next;
            }
            return walker;
        }
        return NULL;
        
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。
 


##  Find the Duplicate Number(287)
### Description
* [LeetCode - 287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/?tab=Description)
* Given an array nums containing `n + 1` integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.
* Note
	- You must not modify the array (assume the array is read only).
	- You must use only constant, `O(1)` extra space.
	- Your runtime complexity should be less than `O(n2)`.
	- There is only one duplicate number in the array, but it could be repeated more than once.
* Tags: Binary Search
### Analysis
* 参考资料：[LeetCode 287. Find the Duplicate Number | 智商被碾压！](https://boweihe.me/2016/03/30/leetcode-287-find-the-duplicate-number-%E6%99%BA%E5%95%86%E8%A2%AB%E7%A2%BE%E5%8E%8B%EF%BC%81/)
* 思路1
	- 假设有n=10的一个数组（大小是n+1），目前的搜索范围是[1,10]。先找中间的数mid=5。现在，遍历数组的所有元素并统计<=5的元素个数，记作n_lteq5好了，那么有：
	- 如果n_lteq5>5，那就有6个数字占了本来只有5个的坑位，那目标数字肯定是在[1,5]的范围内了；
	- 如果n_lteq5<=5，那前面的元素都挺守规矩的，得看看[6,10]里头哪个数字在作怪；
	- 这样每一次判断，问题的规模都会缩小一半，一共n+1个数字，时间复杂度是O(nlogn)
	- [鸽巢原理](https://zh.wikipedia.org/wiki/%E9%B4%BF%E5%B7%A2%E5%8E%9F%E7%90%86)
* 思路2: 映射找环法。时间复杂度O(N)，空间复杂度O(1)。
	- 参考资料：[［Leetcode］287. Find the Duplicate Number简单解法及解释 双指针复杂度O(n)](http://blog.csdn.net/monkeyduck/article/details/50439840)和[Find the Duplicate Number](https://segmentfault.com/a/1190000003817671)
 	- 假设数组中没有重复，那我们可以做到这么一点，就是将数组的下标和1到n每一个数一对一的映射起来。比如数组是`2 1 3`,则映射关系为`0->2`, `1->1`, `2->3`。假设这个一对一映射关系是一个函数`f(n)`，其中n是下标，f(n)是映射到的数。如果我们从下标为0出发，根据这个函数计算出一个值，以这个值为新的下标，再用这个函数计算，以此类推，直到下标超界。实际上可以产生一个类似链表一样的序列。比如在这个例子中有两个下标的序列，`0->2->3`。
 	- 但如果有重复的话，这中间就会产生多对一的映射，比如数组`2 1 3 1`，则映射关系为`0->2`, `{1，3}->1`, `2->3`。这样，我们推演的序列就一定会有环路了，这里下标的序列是`0->2->3->1->1->1->1->...`，而环的起点就是重复的数。
 	- 所以该题实际上就是找环路起点的题，和[LeetCode- 142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)一样。我们先用快慢两个下标都从0开始，快下标每轮映射两次，慢下标每轮映射一次，直到两个下标再次相同。这时候保持慢下标位置不变，再用一个新的下标从0开始，这两个下标都继续每轮映射一次，当这两个下标相遇时，就是环的起点，也就是重复的数。对这个找环起点算法不懂的，请参考[Floyd's Algorithm](https://en.wikipedia.org/wiki/Cycle_detection#Tortoise_and_hare)。
 	
### Slove
#### Method 1: C++
* 方法1：参照思路1，二分法查找，时间复杂度是O(nlogn)。(Cost 13ms. Beats 39.69%)

```cpp
class Solution {
public:
    int countLessValue(vector<int>& nums,int value){
        int res = 0;
        for(int i=0;i<nums.size();i++){
            if(nums[i] <= value){
                res++;
            }
        }
        return res;
    }
    
    int findDuplicate(vector<int>& nums) {
        int size = nums.size();
        int min = 1;
        int max = size-1;
        int mid;
        int temp;
        while(min<max){
            mid = (min+max)/2;
            temp = countLessValue(nums,mid);
            if(temp > mid){
                max= mid;
            }
            else{
                min = mid+1;
            }
        }
        return min;  //min = max
    }
    
};
```

* 方法2：参照思路2，映射找环法。时间复杂度O(N)，空间复杂度O(1)。(Cost 9ms. Beats 70.67%)

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int walker = 0;
        int runner = 0;
        do{
            walker = nums[walker];  //move one step
            runner = nums[nums[runner]]; //move two step
        }while(walker != runner);
        walker = 0;
        while(walker != runner){
            walker = nums[walker];
            runner = nums[runner];
        }
        return walker;
    }
};
```
#### Method 2: JavaScript
原理同C++，此处不再赘述。
 
#### Method 3: Java
原理同C++，此处不再赘述。
 
### Extension
* 参考资料：[LeetCode 287. Find the Duplicate Number | 智商被碾压！](https://boweihe.me/2016/03/30/leetcode-287-find-the-duplicate-number-%E6%99%BA%E5%95%86%E8%A2%AB%E7%A2%BE%E5%8E%8B%EF%BC%81/)
* 对该题进行变形，如果可以改变数组元素呢？
* 如果可以改变数组元素的话，可以按照下面的思路进行，时间复杂度是O(n)。
* 基本思想是，一个萝卜一个坑，把搜索到的数放到自己应该在的位置上，比如1应该放在第一个位置上，如果后面再找到一个1，我们就会发现1这个位置的坑被前一个1占了，那就冲突了。
* 方便起见，数组的位置按1,2,3,…描述（而非0,1,2…）。假设有数组 [5,4,2,3,1,5,6] ，当前工作的位置pos=1
* 先看第pos=1个数字5，在设想的排好序的序列里，5应该就在第5个位置上，因此我们把5和第5个位置上现有的数字1进行调换，结果就变成了 [1,4,2,3,5,5,6] ；
* 看换回来的数字1，由于已经在1号位上，这次处理就结束了，往前挪一下，pos=2，看下一个数字；
* pos=2的数字是4，本来应该在4号位上，因此把4和4号位上的 3调换，数组变成 [1,3,2,4,5,5,6] ；
* 这时候事情没完，因为换回来的3不在应该有的3号位上，因此把3和3号位上占着的2调换，数组变成 [1,2,3,4,5,5,6] ；
* 换回的数字2是正确的位置，因此pos++，变成pos=3，类似地检查pos=4, 5发现都符合要求；
* 当pos=6时，此时位置上的元素是5，于是我们想把这个5归为到5号位，但是检查5号位的时候我们发现，上面已经有一个5了，这就说明5是重复数值了，搞定
* 可能有问，那如果原来的数组是 [5,4,2,3,1,6,5] 呢？很显然，如果前面找到pos=6的时候都没发生冲突，那罪魁祸首肯定在最后个元素上面了。
* 这种方法只要遍历一次数组并做相应替换就行，除了会改变数组内容外，其他条件都满足，并且时间复杂度更小。


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
