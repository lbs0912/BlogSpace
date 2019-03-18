
---
title: LeetCode Notes - 9
date: 2017-03-15 23:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---


* [LeetCode](https://leetcode.com)
* 本文包括[LeetCode - 61. Rotate List](https://leetcode.com/problems/rotate-list/#/description)，[LeetCode - 189. Rotate Array](https://leetcode.com/problems/rotate-array/#/description)， [LeetCode - 151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/#/description)，[LeetCode - 186. Reverse Words in a String II ](https://leetcode.com/problems/reverse-words-in-a-string-ii/)和 [LeetCode - 506. Relative Ranks](https://leetcode.com/problems/relative-ranks/#/description)5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。

<!--more-->


## Rotate List(61)
### Description
* [LeetCode - 61. Rotate List](https://leetcode.com/problems/rotate-list/#/description)
* Given a list, rotate the list to the right by k places, where k is non-negative.
* For example

```
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.
```
* Tags: Linked List, Two Pointers

### Analysis
* 易错点：注意k的值大于链表长度的情况。
* 思路1：环形链表 O(n) time complexity， O(1) memory
	- 参考：[LeetCode Solution](https://leetcode.com/problems/rotate-list/#/solutions)
	- 遍历链表，计算出链表长度，并找到链表尾节点。
	- 让尾节点指向头结点，则将线性链表转换为环形链表。此时，需要移动的节点个数为`k%length`，其中length为链表的长度（即环的长度）。
	- 最后，再遍历链表，从头结点开始，遍历`length-k%length`次，找到题目所需的新链表的头节点，将其设为头结点，并断开环形链表。


### Slove
#### Method 1: C++
* 方法1：参照思路1。(Cost 9ms. Beats 82.23%)

```cpp

class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == NULL ||head->next == NULL || k==0){
        	return head;
        }
        //calculate the size of list
        int length = 1;
        ListNode *tail = head; 
        while(tail->next != NULL){
        	tail = tail->next;
        	length++;
        }
        
        //form a circle
        tail->next = head;  //tail为原来的list的尾部节点
        //计算移动位置
        k = k % length;
        //遍历list，找到移动位置
        for(int i=0;i<length-k;k++){
        	tail = tail->next;
        }
        head = tail->next;
        //断开环形链表
        tail->next = NULL;
        return head;

    }
};
```
#### Method 2: JavaScript
* 方法1：参照思路1。(Cost 122ms. Beats 96.61%)

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
 * @param {number} k
 * @return {ListNode}
 */
var rotateRight = function(head, k) {
    if(head === null || head.next === null || k === 0){
        return head;
    }
    //计算链表长度，并获取尾部节点位置
    var tail = new ListNode();
    tail.val = head.val;
    tail.next = head.next;
    
    var length = 1;
    while(tail.next !== null){
        tail = tail.next;
        length++;
    }
    //形成环形链表
    tail.next = head;
    //计算移位长度
    k = k % length;
    //找到新的头结点
    for(var i=0;i<length-k;i++){
        tail = tail.next;
    }
    //断开环形链表
    head = tail.next;
    tail.next = null;
    return head;
};
```
#### Method 3: Java
原理同C++，此处不再赘述。


## Rotate Array(189)
### Description
* [LeetCode - 189. Rotate Array](https://leetcode.com/problems/rotate-array/#/description)
* Rotate an array of n elements to the right by k steps.
* For example, with n = 7 and k = 3, the array `[1,2,3,4,5,6,7]` is rotated to `[5,6,7,1,2,3,4]`.
* Note: Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
* Hint: Could you do it in-place with O(1) extra space?
* Related problem: [LeetCode - 186. Reverse Words in a String II](https://leetcode.com/subscribe/)

### Analysis
* 参考：[1.1 旋转字符串](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/01.01.md)
* 思路1：复制数组解决。时间复杂度O(n)，空间复杂度O(n)
* 思路2：3次翻转数组。时间复杂度O(n)，空间复杂度O(1)
	- 原理：对于字符AB，若要得到BA，记A^(T)表示A的翻转，则有`(A^(T)B^(T))^T=BA`
	- 以`[1,2,3,4,5,6,7]`, `k=4`为例(`k=k%length`)
	- 第1次翻转：将数组整个元素都翻转，`[7,6,5,4,3,2,1]`
	- 第2次翻转：将数组前k个元素翻转，`[5,6,7,4,3,2,1]`
	- 第3次翻转：将数组后`length-k`个元素翻转，`[5,6,7,1,2,3,4]`
* 思路3：调用C++中的`rotate()`方法。间复杂度O(n)，空间复杂度O(1)

> `template <class ForwardIterator>  ForwardIterator rotate (ForwardIterator first, ForwardIterator middle, ForwardIterator last);`
> **Rotate left the elements in range.**
> Rotates the order of the elements in the range `[first,last)`, in such a way that the element pointed by middle becomes the new first element.  
>  --- [CPlusPlus.com](http://www.cplusplus.com/reference/algorithm/rotate/)


### Slove
#### Method 1: C++
* 方法1：参照思路1，时间复杂度O(n)，空间复杂度O(n)。(Cost 23ms. Beats 37.98%)

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
	    if ((n == 0) || (k <= 0))
        {
	         return;
        }
        vector<int> res = nums;
        int length = nums.size();
        k = k % length;
        int j = 0;
        for(int i=length-k;i<length;i++){
            nums[j] = res[i];
            j++;
        }
        for(int x=0;x<length-k;x++){
              nums[j] = res[x];
              j++;
        }
    }
};
```

* 方法2：参照思路2，时间复杂度O(n)，空间复杂度O(1)。(Cost 19ms. Beats 61.48%)

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int length = nums.size();
        k = k % length;
        if(length == 0 || k == 0) return;
        reverse(nums,0,length);
        reverse(nums,0,k);
        reverse(nums,k,length);
    }
    void reverse(vector<int>& nums,int low,int high){
        int temp;
        high = high-1;  //[low high)
        while(low <= high){
           swap(nums[low++],nums[high--]);
        }
    }
};
```
或者直接vector自带的`reverse`方法。(Cost 19ms. Beats 61.48%)

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        reverse(nums.begin(),nums.end());
        reverse(nums.begin(),nums.begin()+k);
        reverse(nums.begin()+k,nums.end());
    }
};
```
 
* 方法3：参照思路3，时间复杂度O(n)，空间复杂度O(1)。(Cost 19ms. Beats 61.48%)

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();
        std::rotate(nums.begin(),nums.end()-k,nums.end());
    }
};
```
#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。
 


## Reverse Words in a String(151)
### Description
* [LeetCode - 151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/#/description)
* Given an input string, reverse the string word by word.
* For example
Given `s = "the sky is blue"`,
return `"blue is sky the"`.
* For C programmers: Try to solve it in-place in O(1) space.
* Clarification:
	- What constitutes a word?
	- A sequence of non-space characters constitutes a word.
	- Could the input string contain leading or trailing spaces?
	- Yes. However, your reversed string should not contain leading or trailing spaces.
	- How about multiple spaces between two words?
	- Reduce them to a single space in the reversed string.

### Analysis
* 思路1: 多步翻转法。时间复杂度O(n)，空间复杂度O(1)。
	- 参考[LeetCode - 189. Rotate Array](https://leetcode.com/problems/rotate-array/#/description)中的思路2。
	- 先将整个字符串都翻转，然后再翻转每一个单词。
	- 注意去除字符串的前导0，后导0和单词之间多余的空格。

### Slove
#### Method 1: C++
* 方法1：参照思路1，时间复杂度O(n)，空间复杂度O(1)。(Cost 6ms. Beats 37.73%)

```cpp
class Solution {
public:
	void reverseWords(string &s) {
		//去除前导0
		int size = s.size();
		int length = 0;
		while (s[length] == ' ' && length < size) {
			length++;
		}
		s.erase(0, length);//删除从第0个元素开始的length个字符
		//去除后导0
		size = s.size();
		length = size - 1;
		int count = 0;
		if (length > 0) {
			while (s[length] == ' ' && length >= 0) {
				length--;
				count++;
			}
			s.erase(length + 1, count); //删除从第length个元素开始的count个字符
		}
		//多个空格中只保留一个
		bool hasBackspace = false;
		for (int i = 0; i < s.size(); i++) {
			if ((s[i] == ' ') && (hasBackspace == false))
			{
				hasBackspace = true;  //保留第1个空格
			}
			else if ((s[i] == ' ') && (hasBackspace == true)) {
				s.erase(i, 1); //删除从第i个元素开始的1个字符
				i--;
			}
			else if (s[i] != ' ') {
				hasBackspace = false;
			}
		}
		//翻转整个字符
		reverse(s.begin(), s.end());
		length = s.size();
		int end = 0;
		int start = 0;
		for (int i = 0; i<length; i++) {
			if (s[i] == ' ' || i == length - 1) {  //结尾处字符
				if (i == length - 1) {
					reverse(s.begin() + start, s.end());
				}
				else {
					reverse(s.begin() + start, s.begin() + start + end);
					start = start + end + 1;
					end = 0;
				}

			}
			else {
				end++;
			}

		}
	}
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。
 
 
## Reverse Words in a String II(186)
### Description
* [LeetCode - 186. Reverse Words in a String II ](https://leetcode.com/problems/reverse-words-in-a-string-ii/)
* Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.
* The input string does not contain leading or trailing spaces and the words are always separated by a single space.
* For example

```
Given s = "the sky is blue",
return "blue is sky the".
```
* Could you do it in-place without allocating extra space?
### Analysis
* 思路1
	- 参考[LeetCode - 151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/#/description)中的思路1。
	- 先将整个字符串都翻转，然后再翻转每一个单词。
	- 该题比`Reverse Words in a String`题目简单，因为不用处理前导0，后导0和单词之间多余的空格。

### Slove
#### Method 1: C++
* 方法1：参照思路1，时间复杂度O(n)，空间复杂度O(1)。

```cpp
class Solution {
public:
	void reverseWords(string &s) {
		//翻转整个字符
		reverse(s.begin(), s.end());
		length = s.size();
		int end = 0;
		int start = 0;
		for (int i = 0; i<length; i++) {
			if (s[i] == ' ' || i == length - 1) {  //结尾处字符
				if (i == length - 1) {
					reverse(s.begin() + start, s.end());
				}
				else {
					reverse(s.begin() + start, s.begin() + start + end);
					start = start + end + 1;
					end = 0;
				}

			}
			else {
				end++;
			}

		}
	}
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
#### Method 3: Java
原理同C++，此处不再赘述。



## Relative Ranks(506)
### Description
* [LeetCode - 506. Relative Ranks](https://leetcode.com/problems/relative-ranks/#/description)
* Given scores of `N` athletes, find their relative ranks and the people with the top three highest scores, who will be awarded medals: `"Gold Medal"`, `"Silver Medal"` and `"Bronze Medal"`.
* Example 1

```
Input: [5, 4, 3, 2, 1]
Output: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]

Explanation: The first three athletes got the top three highest scores, so they got "Gold Medal", "Silver Medal" and "Bronze Medal". 

For the left two athletes, you just need to output their relative ranks according to their scores.
```

* Note
	- N is a positive integer and won't exceed 10,000
	- All the scores of athletes are guaranteed to be unique.

### Analysis

* 思路1: 
	- 参考：[LeetCode Solution](https://leetcode.com/problems/relative-ranks/#/solutions)和[506. Relative Ranks](http://blog.csdn.net/kakitgogogo/article/details/54944897)
	- Time complexity: O(NlgN). Space complexity: O(N). N is the number of scores.
	- 最初会想到对数组进行降序排序，其索引值就是对应的名字。但是，如果直接排序的话，会丢失数组原有的顺序。
	- 因此，采用`pair`数据结构。创建`vector<pair<int,int>> vec;`容器，pair的第1个参数保存原数组的索引顺序，pair的第2个参数保存原数组的元素值，并按照pair的第2个参数进行降序排序。
	- `sort`排序中，自定义比较函数，采用lambda表达式完成。

### Slove
#### Method 1: C++

* 方法1：参照思路1。(Cost 12ms. Beats 80.4%)

```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        vector<pair<int,int>> vec;
        vector<string> res(nums.size());
        string ranks[3] = {"Gold Medal", "Silver Medal", "Bronze Medal"};

        for(int i=0;i<nums.size();i++){
        	vec.push_back(pair<int,int>(i,nums[i]));
        }
        auto comp = [](const pair<int,int>& p1,const pair<int,int>& p2){
        	return p1.second >p2.second;
        };
        sort(vec.begin(),vec.end(),comp);
        for(int i=0;i<nums.size();i++){
        	if(i<3){
        		res[vec[i].first] = ranks[i];
        	}
        	else{
        		res[vec[i].first] = to_string(i+1);
        	}
        }
        return res;
    }
};
```

#### Method 2: JavaScript
* 方法1：参照思路1。(Cost 155ms. Beats 80.53%)
参考[LeetCode Solution](https://leetcode.com/problems/relative-ranks/#/solutions)。创建一个映射数组，保存原数组中元素的排序顺序。
 
 
```javascript
/**
 * @param {number[]} nums
 * @return {string[]}
 */
var findRelativeRanks = function(nums) {
    //复制数组
    var sortedNums = nums.slice(0);
    sortedNums.sort(function(a,b){  //降序
        return b-a; 
    });
    var sortedNumsMapping = {};
    sortedNums.forEach(function(el,index){
        sortedNumsMapping[el] = (index+1).toString();
    });
    
    var res = [];
    res = nums.map(function(el,index){
       if(sortedNumsMapping[el] === '1')        return "Gold Medal";
       else if(sortedNumsMapping[el] === '2' ) return "Silver Medal";
       else if(sortedNumsMapping[el] === '3' ) return "Bronze Medal";
       else return (sortedNumsMapping[el]);
    });
    return res;
}; 
```

#### Method 3: Java
原理同C++，此处不再赘述。










## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
