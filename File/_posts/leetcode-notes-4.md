---
title: LeetCode Notes - 4
date: 2017-02-28 15:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---



* [LeetCode](https://leetcode.com)
* 本文包括 [LeetCode -  201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/?tab=Description)， [LeetCode -  342. Power of Four](https://leetcode.com/problems/power-of-four/?tab=Description)， [LeetCode -  169. Majority Element](https://leetcode.com/problems/majority-element/)，[LeetCode - 229. Majority Element II](https://leetcode.com/problems/majority-element-ii/) 和 [LeetCode - 187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/?tab=Description) 5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。



<!--more-->





## Bitwise AND of Numbers Range(201)
### Description
* [LeetCode -  201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/?tab=Description)
* Given a range `[m, n]` where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
* For example, given the range [5, 7], you should return 4.

### Analysis
* 思路1：使用移位操作实现。
	- 以[5,7]为例，`5=0101，6=0110，7=0111`，最后返回的数是0100，即最后的结果是该数字范围内所有数的左边共同的部分。
	- 再以[26,30]为例，其中的数字为`11010, 11011, 11100, 11101, 11110`，最左边的公共部分是`11`，故返回的结果是`11000`。
	- 算法实现思路：直接平移`m`和`n`，每次向右移一位，直到`m`和`n`相等，记录下所有平移的次数`i`，然后再把`m`左移`i`位即为最终结果。
 

### Slove
#### Method 1: C++

* 方法1：使用移位操作。（Cost 32ms. Beats 39.47%）

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int count = 0;
        while(m!=n){
            m = m >> 1;
            n = n >> 1;
            count = count + 1;
        }
        return m << count;
    }
};
```

* 方法2：原理同方法1，但是采用递归实现。（Cost 29ms. Beats 56.55%）

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        return (n > m) ? (rangeBitwiseAnd(m/2, n/2) << 1) : m;
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。 （Cost 305ms. Beats 40.91%）
 
```javascript
 /**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var rangeBitwiseAnd = function(m, n) {
    var count = 0;
    while(m!=n){
        m = m >> 1;
        n = n >> 1;
        count = count + 1;
    }
    return m << count;
};
```

 
#### Method 3: Java
原理同C++，此处不再赘述。


##  Power of Four(342)
### Description
* [LeetCode -  342. Power of Four](https://leetcode.com/problems/power-of-four/?tab=Description)
* Given an integer (signed 32 bits), write a function to check whether it is a power of 4.
* Example
Given `num = 16`, return true. Given `num = 5`, return false.
* Follow up: Could you solve it without loops/recursion?

### Analysis
* 思路1
	- 在`Power of four`的基础上进行分析，首先判断该数n是否是2的次幂数（`n&(n-1)==0`），然后再从中筛选出4的次幂数。
	- 分析如下二进制数形式。2的次幂数的二进制形式，相当于将`1`逐次向左移动1位，而4的次幂数的二进制形式，相当于将`1`逐次向左移动2位。并且4的次幂数的二进制形式中，数字`1`只出现在第1位（1也是4的次幂数），第3位，第5位，第7位……上（从低位数起）。
	-  因此，将数字n和一个可以在奇数位上为1的数字`0101 0101……`进行与操作，就可以判断该数字是否是4的次幂数。
	-  C++中，`int`为32位，则`0101 0101……`的16进制表达形式为`0x55555555`。因此，可以使用`(num & 0x55555555) !=0`来判断该数是否是4的次幂数。故最终的判断语句为`num > 0 && (num & (num - 1)) ==0  && (num & 0x55555555) !=0;`


```cpp
1 => 1
10 => 2
100 => 4
1000 => 8
10000 => 16
100000 => 32
1000000 => 64
10000000 => 128
100000000 => 256
1000000000 => 512
10000000000 => 1024
100000000000 => 2048
1000000000000 => 4096

```







 
### Slove
#### Method 1: C++
* 方法1：位运算实现。先筛选出2的次幂数，再从中筛选出4的次幂数。（Cost 3ms. Beats 34.77%）

```cpp
class Solution {
public:
    bool isPowerOfFour(int num) {
        return (num>=1) && ((num & (num-1))==0) && ((num & 0x55555555) != 0);
    }
};
```

#### Method 2: JavaScript

* 方法1：同C++中方法1，位运算实现。先筛选出2的次幂数，再从中筛选出4的次幂数。（Cost 142ms. Beats 40.00%）

```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPowerOfFour = function(num) {
    return (num>=1) && ((num & (num-1))===0) && ((num & 0x55555555) !== 0);
};
```
> Use `!==` instead of `!=` in JavaScript.



#### Method 3: Java
原理同C++，此处不再赘述。


## Majority Element(169)
### Description
* [LeetCode -  169. Majority Element](https://leetcode.com/problems/majority-element/)
* Given an array of size n, find the majority element. The majority element is the element that appears more than `⌊ n/2 ⌋` times.
* You may assume that the array is non-empty and the majority element always exist in the array.

### Analysis
* 说明：题目中限制了数组非空，并且一定存在majority element。若给定的数组不存在majority element，则返回任意一个元素即可。例如，`arr = [1,2,3,2]`中是不存在majority element的，此时程序返回1或2或3均可。

* 思路1
	- 遍历数组，比较majority element的计数值
	- 由于majority element出现的次数大于`⌊ n/2 ⌋`，超过了数组总元素次数的一半。因此，若遍历数组时，遇见一个不等于majority element的元素后将majority element的count值减去1，遍历完数组后其count值仍能大于0。
* 思路2
	- 为每一个数组元素都设定一个计数值，最后根据最大的计数值，返回majority element。
	- 该方法在实现上比较适合采用JavaScript语言。原因分析见Slove部分。
	- 阅读 [Majority Voting Algorithm](https://gregable.com/2013/10/majority-vote-algorithm-find-majority.html)，了解该算法的思想。

* 思路3
	- 采用位操作实现。
	- 数组中的majority element元素是出现次数最多的元素，那也就意味着该元素的二进制位上的每一个1或者0，都是整个数组元素的该位的众数。
	- 因此，可以逐位求取每一位的众数，这样，由众数组成的数值一定就是majority element。
	
### Slove

#### Method 1:  JavaScript
* 方法1：为每一个数组元素都设定一个计数值，最后根据最大的计数值，返回majority element。(Cost 122ms. Beats 37.91%)

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    var map = {};  //对象，不是数组
    if(nums.length ===1) {return nums[0]}

    for(var i = 0 ; i < nums.length ; i++){
        // 第一次出现的元素
        if(!map[nums[i]]) {
            map[nums[i]] = 1;
        } else {
            // 已经出现过的元素，计数值增加1
            map[nums[i]]++;
            if(map[nums[i]] >= nums.length/2){
                return nums[i];
            }
        }
    }
};
```

> 题目中限定了一定存在majority element元素。如果给定的数组不存在majority element元素，上述程序会没有返回值。

需要指出的是，该方法比较适合用JS中的对象进行计数，不适合用C++语言实现。因为在C++中，数组需要在初始化时就指定长度，vector容器虽然可以动态改变长度，但是需要索引值为非负数。

例如下述C++程序，创建vector容器用来存储计数值。需要先获取num数组的最大值，从而指定vector容器的长度，操作十分耗时。另一方面，若num数组中有负数出现，比如`nums=[-1,1,1,1,1,2]`，则访问`map[nums[0]]`的的时候会有语法错误（索引值为非负数）。采用JavaScript中的对象进行计数值的存储，可以很好地解决上述两个问题。JavaScript中的对象采用`key:value`存储属性值，并不要求`key`一定为非负数。
 
``` cpp
//cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int length = nums.size();
        int halfLength = (int)floor(length/2);
        sort(nums.begin(),nums.end());  //数组排序
        int max = nums[length-1];
        if(length < 3){
            return nums[0];
        }
        vector<int> map(max,0);  //创建一个计数容器
        for(int i=0;i<length;i++){
            if(map[nums[i]] == 0){  //该元素第1次出现
                map[nums[i]] = 1;
            }
            else{
                //该元素已经出现过了
                map[nums[i]]++;
                if(map[nums[i]] > halfLength){
                    return nums[i];
                }
            }
        }
    }
};
```
 
 


#### Method 2: C++
* 方法1：遍历数组，比较majority element的计数值。（Cost 26ms. Beats 35.10%）
初试设定`nums[0]`为majority element，若下一个元素不等于majority，则二者可以相互**“抵消”**，计数值count等于0，重新寻找majority element。由于majority element出现的次数大于`⌊ n/2 ⌋`，超过了数组总元素次数的一半。因此，若遍历数组时，遇见一个不等于majority element的元素后将majority element的count值减去1，遍历完数组后其count值仍能大于0。
 

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int length = nums.size();
        if(length < 3) return nums[0];
        int majority = nums[0];
        int count = 1;
        for(int i=1;i<length;i++){
            if(count == 0){
                majority = nums[i];
                count++;
            }
            else{
                if(nums[i] == majority){
                    count++;
                }
                else{
                    count--;
                }
            }
        }
        return majority;
    }
};
```

* 方法2：采用位操作。求取二进制位上每一位的众数，最后获取majority element。（Cost 23ms. Beats 49.08%）

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int size = nums.size();
        //int halfSize = (int)floor(size/2);
        int length = sizeof(int)*8; //每个int占4个字节，共32位
        int count = 0;
        int mask = 1;
        int ret = 0;
        for(int i=0;i<length;i++){  //共有32个位需要判断
            count = 0;  //清空计数值
            for(int j=0;j<size;j++){
                if(mask & nums[j]){  //该位为1
                    count++;
                }
            }
            if(count > size/2){  //该位众数为1
                ret = ret | mask; 
            }
            mask <<= 1;  //向左移位，判断高位的众数
        }
        return ret;
    }
};
```

#### Method 3: Java
原理同C++，此处不再赘述。


## Majority Element II(229)
### Description
* [LeetCode - 229. Majority Element II](https://leetcode.com/problems/majority-element-ii/)
* Given an integer array of size n, find all elements that appear more than `⌊ n/3 ⌋` times. The algorithm should run in linear time and in `O(1)` space.
* Hint
	- How many majority elements could it possibly have?
	-  Do you have a better hint? Suggest it!

### Analysis
* 参照*LeetCode -  169.Majority Element*中的思路1，解决该问题。
* 求解过程中采用了 [The Boyer-Moore Majority Vote Algorithm](http://www.cs.rug.nl/~wim/pub/whh348.pdf)算法，参考下列链接了解更多。
	- [Boyer-Moore Majority Vote algorithm - LeetCode](https://leetcode.com/problems/majority-element-ii/?tab=Solutions)
	- [Majority Voting Algorithm](https://gregable.com/2013/10/majority-vote-algorithm-find-majority.html)
	- [The Boyer-Moore Majority Vote Algorithm](http://www.cs.rug.nl/~wim/pub/whh348.pdf)

* 根据题意可知，majority element元素最多有两个。根据[The Boyer-Moore Majority Vote Algorithm](http://www.cs.rug.nl/~wim/pub/whh348.pdf)算法可以求解该问题。

### Slove

#### Method 1: C++

* 方法1： 采用了 [The Boyer-Moore Majority Vote Algorithm](http://www.cs.rug.nl/~wim/pub/whh348.pdf)算法。（Cost 16ms. Beats 45.19%）

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> result;
        if(nums.empty()){
            return result;
        }
            
        int candidate1 = nums[0];
        int candidate2 = nums[1];
        int count1 = 0;
        int count2 = 0;
        int length = nums.size();
       
        for(int i=0;i<length;i++){
            if(nums[i] == candidate1){
                count1++;
            }
            else if(nums[i] == candidate2){
                count2++;
            }
            else if(count1 == 0){
                count1 = 1;
                candidate1 = nums[i];
            }
            else if(count2 == 0){
                count2 = 1;
                candidate2 = nums[i];
            }
            else{
                count1--;
                count2--;
            }
        }
        
        count1 = 0;
        count2 = 0;
        
        for(int i=0;i<length;i++){
            if(nums[i] == candidate1){
                count1++;
            }
            else if(nums[i] == candidate2){
                 count2++;
            }
        }
        
        if(count1 > (length/3)){
             result.push_back(candidate1);
        }
        if(count2 > (length/3)){
            result.push_back(candidate2);
        }
      
        return result;
    }
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。（Cost 106ms. Beats 70.73%）

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var majorityElement = function(nums) {
    var result =  [];
    var length = nums.length;
    if(length === 0){
        return result;
    }
    var candidate1 = nums[0];
    var candidate2 = nums[1];
    var count1 = 0;
    var count2 = 0;
   
    for(var i=0;i<length;i++){
        if(nums[i] === candidate1){
            count1++;
        }
        else if(nums[i] === candidate2){
            count2++;
        }
        else if(count1 === 0){
            count1 = 1;
            candidate1 = nums[i];
        }
        else if(count2 === 0){
            count2 = 1;
            candidate2 = nums[i];
        }
        else{
            count1--;
            count2--;
        }
    }
 
    count1 = 0;
    count2 = 0;
    
    for(var i=0;i<length;i++){
        if(nums[i] === candidate1){
            count1++;
        }
        else if(nums[i] === candidate2){
             count2++;
        }
    }
        
    if(count1 > (length/3)){
        result.push(candidate1);
    }
    if(count2 > (length/3)){
        result.push(candidate2);
    }
     
    return result;
    
};
```

#### Method 3: Java
原理同C++，此处不再赘述。


## Repeated DNA sequences(187)
### Description
* [LeetCode - 187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/?tab=Description)
* All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
* Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
* For example

```
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return: ["AAAAACCCCC", "CCCCCAAAAA"].
```

### Analysis
* 参考阅读
	- [CSDN - Repeated DNA Sequences](http://blog.csdn.net/nk_test/article/details/48383103#)
	- [CNBlog - Repeated DNA Sequences](http://www.cnblogs.com/yrbbest/p/4491657.html)
* 思路1
	- 使用C++中的关联容器`map`实现。将字符串中每10个字符串作为一个`map`容器的关键字，并记录其出现的次数。若出现的次数大于1次，则输出该字符串。
	- 关于关联容器的介绍，参见*《C++ Primer》*教材。
* 思路2
	- 将A,C,G,T编码为00,01,02,03，从而将每10个字符组成的字符串转换成一个20位的二进制数。由于`int`是32位的，因此可以将该结果保存在一个int值中。这样，可以节省存储空间。
	- 转换成二进制编码值之后，后续操作同思路1。二进制编码转换过程中，采用位操作实现。
	
* 思路3
	- 采用哈希表实现。该方法原理同思路1，只是采用`HashMap()`数据结构实现。


### Slove
#### Method 1: C++
* 方法1：使用关联容器map。(Cost 185ms. Beats 11.49%)

```cpp
class Solution {  
public:  
    vector<string> findRepeatedDnaSequences(string s) {  
        vector <string>  ans;  
        map<string,int> mp;  
        int length = s.length();
        if(length<10){
            return ans;    //长度小于10，返回空vector
        }  
     
        for(int i=0;i<=(length-10);i++){
            mp[s.substr(i,10)]++;  //更新关联容器值，每10个字符串作为一个关键字
        }  
        
        //迭代器声明
        //也可以使用auto关键字  auto it = mp.begin()
        map<string,int>::iterator it;  
        
        for( it=mp.begin();it!=mp.end();++it)  
        {  
            if(it->second>1){ //出现次数大于1，则保存该结果
                ans.push_back(it->first);
            }  
        }  
        return ans;  
    }  
};  
```

* 方法2：使用位操作实现。(Cost 105ms. Beats 33.41%)
```cpp
class Solution {
public:
	vector<string> findRepeatedDnaSequences(string s) {
		map<int, int> mp;    //遍历字符串string
		map<char, int> cur;  //将A，G，C，T分别映射为0,1,2,3
		vector<string> ans;  //返回的结果值
		set<string> st;

		int size = s.size();
		if (size < 10) {
			return ans;
		}

		//将A，G，C，T分别映射为00,01,10,11
		cur['A'] = 0;
		cur['C'] = 1;
		cur['G'] = 2;
		cur['T'] = 3;
		
		//求解前10个字符串的编码值,采用移位操作实现
		int temp = 0;
		for (int i = 0; i < 10; i++) {
		    temp <<= 2;  //A，G，C，T每个编码占2位
			temp |= cur[s[i]];
		
		}
		mp[temp]++;  //保存第1个子字符串的编码值

		//遍历字符串s，对子字符串出现次数计数
		for (int i = 10; i < size; i++) {
			temp <<= 2;  //移位之后temp为22位
			temp &= ~(0x300000);  //去除前2位
			temp |= cur[s[i]];   // 获取新的子字符串的编码值
			mp[temp]++; 
			if (mp[temp] > 1) { //出现次数大于1
				st.insert(s.substr(i-9,10));
				//由于set容器不允许关键字重复，
				//因此可以将该语句放在for循环中
				//重复的子字符串并不会被重复插入
				//如果不使用set容器，需要将该语句放在一个单独的循环中，这样耗时会增加
			}
		}

		set<string>::iterator it;
		for (it = st.begin(); it != st.end(); it++) {
			ans.push_back(*it);
		}
		return ans;
	}
};
```

#### Method 2: JavaScript
原理同C++，此处不再赘述。
 

#### Method 3: Java
* 方法1：使用哈希表，`HashMap()`数据结构实现。(Cost 65ms. Beats 31.12%)

```java
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new ArrayList<String>();
        if(s == null || s.length() < 10)
            return res;
        Map<String, Integer> map = new HashMap<>();
        
        for(int i = 0; i < s.length() - 9; i++) {
            String subStr = s.substring(i, i + 10);
            if(map.containsKey(subStr)) {
                if(map.get(subStr) == 1){
					res.add(subStr);  //已经出现过1次
				}
                map.put(subStr, map.get(subStr) + 1);
            } else{
				map.put(subStr, 1);
			}   
        }
        return res;
    }
}
```

* 方法2：使用哈希表，`HashMap()`数据结构实现。对方法1进行优化。(Cost 53ms. Beats 63.99%)

```java
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new ArrayList<>();
        if (s == null) return res;
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i + 10 <= s.length(); i++) {
            String str = s.substring(i, i + 10);
            if (!map.containsKey(str)) {
                map.put(str, 1);
            } else {
                if (map.get(str) == 1) res.add(str);
                map.put(str, 2);
            }
        }
        return res;
    }
}
```
 


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
