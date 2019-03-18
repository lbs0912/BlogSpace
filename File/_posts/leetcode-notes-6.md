---
title: LeetCode Notes - 6
date: 2017-03-06 18:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---




* [LeetCode](https://leetcode.com)
* 本文包括 [Leetcode - 482. License Key Formatting](https://leetcode.com/problems/license-key-formatting/?tab=Description)， [LeetCode - 322. Coin Change ](https://leetcode.com/problems/coin-change/?tab=Description)， [LeetCode - 518. Coin Change 2 ](https://leetcode.com/problems/coin-change-2/?tab=Description)，[LeetCode - 500. Keyboard Row](https://leetcode.com/problems/keyboard-row/?tab=Description) 和[LeetCode - 448. Find All Numbers Disappeared in an Array ](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/?tab=Description)5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。


<!--more-->

## License Key Formatting(482)
### Description
* [Leetcode - 482. License Key Formatting](https://leetcode.com/problems/license-key-formatting/?tab=Description)
* Now you are given a string S, which represents a software license key which we would like to format. The string S is composed of alphanumerical characters and dashes. The dashes split the alphanumerical characters within the string into groups. (i.e. if there are M dashes, the string is split into M+1 groups). The dashes in the given string are possibly misplaced.
* We want each group of characters to be of length K (except for possibly the first group, which could be shorter, but still must contain at least one character). To satisfy this requirement, we will reinsert dashes. Additionally, all the lower case letters in the string must be converted to upper case.
* So, you are given a non-empty string S, representing a license key to format, and an integer K. And you need to return the license key formatted according to the description above.

* Example 1

```
Input: S = "2-4A0r7-4k", K = 4

Output: "24A0-R74K"
```

Explanation: The string S has been split into two parts, each part has 4 characters.

* Example 2

```
Input: S = "2-4A0r7-4k", K = 3

Output: "24-A0R-74K"
```
Explanation: The string S has been split into three parts, each part has 3 characters except the first part as it could be shorter as said above.

* Note
	- The length of string S will not exceed 12,000, and K is a positive integer.
	- String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).
	- String S is non-empty.
	
### Analysis
* 思路1
	- 常规做法。首先确定第1部分的长度。
	- 使用JavaScript中提供的`join()`，`slice()`等函数求解。
* 思路2
	- 先不考虑第1部分的长度，而是先从字符串的尾部考虑，依次取出尾部部分，则剩下的字符串即为第1部分的长度。
	- 借助C++容器的`rbegin()`和`rend()`函数，可以很方便实现该算法。
* 大小写字母转换部分，JavaScript中的函数为`toUpperCase()`和`toLowerCase()`。而C++中对应的函数为`toupper()`和`tolower`，且用于单个字符`char`。
* `char ^ 0x20`也可以将小写字母转换成大写字母。

### Slove
#### Method 1: JavaScript
* 方法1：借助`toUpperCase()`, `join()`，`slice()`等函数实现。(Cost 89ms. Beats 85.95%)

```javascript
/**
 * @param {string} S
 * @param {number} K
 * @return {string}
 */
var licenseKeyFormatting = function(S, K) {
    var res = []; //Array
	//转换成大写字母，并删除所有的下划线
	var myS = S.toUpperCase().replace(/-/g,""); 
	var length = myS.length;
    var firstLength = (length%K===0)? K:length%K;
	res[0]=myS.slice(0,firstLength);
	for(var i=firstLength,j=1;i<length;i+=K,j++){
	    res[j]=myS.slice(i,i+K);
	}
	return res.join("-");
};
```

#### Method 2: C++
* 方法1： 借助容器的`rbegin()`和`rend()`函数。(Cost 9ms. Beats 40.95%)

```cpp
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string res;
        for (auto i = S.rbegin(); i < S.rend(); i++){
             if (*i != '-'){
                 (res.size()%(K+1)-K? res : res+='-') += toupper(*i);
             } 
        }
        return reverse(res.begin(), res.end()),res; //逗号表达式 reverse函数无返回值
    }
};
```
需要注意的是，`reverse()`函数无返回值，是`void`类的函数，因此`return`语句中采用了逗号表达式。
 

#### Method 3: Java
* 方法1： 思路同C++。(Cost 33ms. Beats 35.62%)

```java
public class Solution {
    public String licenseKeyFormatting(String s, int k) {
        StringBuilder sb = new StringBuilder();
        for (int i = s.length() - 1; i >= 0; i--)
            if (s.charAt(i) != '-'){
                sb.append(sb.length() % (k + 1) == k ? '-' : "").append(s.charAt(i));
            }
               
        return sb.reverse().toString().toUpperCase();
    } 
}
```
需要注意的是，Java中不能直接对`String`进行修改，需要创建`StringBuilder`，最后再调用`toString()`函数进行转换。
 
## Coin Change(322)
### Description
* [LeetCode - 322. Coin Change ](https://leetcode.com/problems/coin-change/?tab=Description)
* You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.
* Example 1

```
coins = [1, 2, 5], amount = 11
return 3 (11 = 5 + 5 + 1)
```

* Example 2

```
coins = [2], amount = 3
return -1.
```
* Note: You may assume that you have an infinite number of each kind of coin.

### Analysis
* 思路1（动态规划）
	- 参考：[动态规划：从新手到专家](http://www.hawstein.com/posts/dp-novice-to-advanced.html)
	- 使用动态规划DP算法。设定状态`dp[i]`表示钱数为`i`时候所需的最少硬币个数，转移方程为`dp[i]=min(dp[i],dp[i-coins[j]]+1)`，其中`coins[j]`表示给定的某一面值的硬币。
 
### Slove
#### Method 1: C++
* 方法1：动态规划。  (Cost 25ms. Beats 90.88%)

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int max = amount+1;
        vector<int> dp(max,max);  //初始化为最大值
        dp[0]=0;
        int size = coins.size();
        for(int i=1;i<=amount;i++){
            for(int j=0;j<size;j++){
                if(coins[j]<=i){
                     dp[i]=min(dp[i],dp[i-coins[j]]+1);
                }
            }
        }
        return (dp[amount]>amount) ? -1:dp[amount];
    }
};
```
注意，程序中需要初始化`dp`容器为最大值，如`0x7fffffff`或者`INT_MAX`。程序最后对`dp[amount]`值进行判断，若大于拼凑成功的最大值(`amount`)，则表示无法用这些硬币凑出`amount`值的金额。但实际情况中，由于最小硬币为1，因此若可以用这些硬币拼凑成功，`dp[amount]`的最大值为`amount/1=amount`。综上，只要将`dp`容器的初始值初始化为大于`amount`即可。
 
>Note:  **贪婪法不一定能得到最优解，但是一个可行的，较好的解。**例如，针对本题，若`coins=[1,2,5,6]`，`amount=11`，使用动态规划求解的最优解为2（硬币5和6），若采用贪婪算法，则求解结果为3（硬币6, 6和1）。


#### Method 2: JavaScript
原理同C++，此处不再赘述。
 
#### Method 3: Java
原理同C++，此处不再赘述。


## Coin Change 2(518)
### Description
* [LeetCode - 518. Coin Change 2 ](https://leetcode.com/problems/coin-change-2/?tab=Description)
* You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.
* Note: You can assume that
	- 0 <= amount <= 5000
	- 1 <= coin <= 5000
	- the number of coins is less than 500
	- the answer is guaranteed to fit into signed 32-bit integer

* Example 1

```
Input: amount = 5, coins = [1, 2, 5]
Output: 4

Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```
* Example 2

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

* Example 3

```
Input: amount = 10, coins = [10] 
Output: 1
```


### Analysis
* 思路1（动态规划 - 01背包问题）

该题目属于动态规划中的01背包问题。
 

参考[kickoff leetcode - Coin Change 2](http://startleetcode.blogspot.jp/2017/02/leetcode-518-coin-change-2.html)了解该动态规划思路。

设定状态`dp[i][j]`表示用硬币数组中的`coins[0]~coins[i]`硬币凑出总额`j`的方案数。则转移方程为`dp[i][j]=dp[i][j]+dp[i+1][j-coins[i+1]]`。该转移方程中，`dp[i+1][j-coins[i+1]]`表示使用`coins[i+1]`硬币拼凑总额`j`时的方案数（条件为`coins[i+1]<=j`），`dp[i][j]`表示不使用`coins[i+1]`硬币拼凑总额`j`时的方案数。边界条件为`dp[i][0]=1`，表示当总额为0时，只有一种拼凑方案。

上面的状态和转移方程可以简化为

```cmake
dp[j] = dp[j]+dp[j-coins[i]]   (coins[i]<1)
dp[0] = 1
```
 
 
  

### Slove
#### Method 1: C++
* 方法1：动态规划。(Cost 3ms. Beats 68.81%)

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int max = amount+1;
        vector<int> dp(max,0);
        dp[0]=1;
        for(auto coin : coins){
            for(int j=1;j<=amount;j++){
                if(coin<=j){
                    dp[j] = dp[j]+dp[j-coin];
                }
            }
        }
        return dp[amount];
    }
};
```

需要注意的是，2层循环的关系。
* 转移方程中，`dp[j]`表示使用`coins[i]`硬币拼凑总额`j`时的方案数（条件为`coins[i+1]<=j`），`dp[j]`表示不使用`coins[i]`硬币拼凑总额`j`时的方案数。


```cmake
dp[j] = dp[j]+dp[j-coins[i]]   (coins[i]<1)
dp[0] = 1
```
因此，需要保证`dp[j] = dp[j]+dp[j-coins[i]]`中等式后面的`dp[j]`的值中不包括使用`coins[i]`的情况。因此，需要将`auto coin : coins`循环放在外层。

如果用如下循环

```cpp
for(int j=1;j<=amount;j++){
	for(auto coin : coins){
	    if(coin<=j){
	        dp[j] = dp[j]+dp[j-coin];
        }
    }
}
```
则在计算过程中， `dp[j] = dp[j]+dp[j-coins[i]]`中等式后面的`dp[j]`的值中会已经包括使用`coins[i]`的情况，最终造成方案计数值过大。

```cmake
dp[0] = 1
dp[1] = dp[1]+dp[1-1] = 1                (1=1)
dp[2] = dp[2]+dp[2-1] = 0+1=1            (2=1+1)
dp[2] = dp[2]+dp[2-2] = 1+1=2            (2=1+1=2)
dp[3] = dp[3]+dp[3-1] = 0+2=2            (3=2+1=1+1+1)
dp[3] = dp[3]+dp[3-2] = 2+1=3            !!Error
```
 在计算`dp[3]`的第1个式子中`dp[3] = dp[3]+dp[3-1]`中的`dp[3-1]`已经包含了使用`coins[1]=2`的情况。因此，计算`dp[3]`的第2个式子中`dp[3] = dp[3]+dp[3-2]`中会造成重复计数。

错误原因就是2层循环的位置搞错了。应该将硬币数组值循环放在外层，金额总数循环放在内层，先计算出只是用`coins[i]`的所有不同金额数的方案，然后再将硬币种类数增加，遍历所有的金额值。
 
 
#### Method 2: JavaScript
原理同C++，此处不再赘述。

#### Method 3: Java
原理同C++，此处不再赘述。





## Keyboard Row(500)
### Description
* [LeetCode - 500. Keyboard Row](https://leetcode.com/problems/keyboard-row/?tab=Description)
* Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.

![keyboard.png](https://leetcode.com/static/images/problemset/keyboard.png)

* Example 1

```
Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]
```

* Note
	- You may use one character in the keyboard more than once.
	- You may assume the input string will only contain letters of alphabet.

### Analysis
* 思路1
	- 创建3个表，分别保存每行的字母。利用C++中的关联容器`set`进行查找。
	- set容器可是进行高速查找。
* 思路2
	- 利用正则表达式实现。


### Slove
#### Method 1: C++
* 方法1：参照思路1，使用set关联容器。 (Cost 3ms. Beats 2.75%)
 
```cpp
class Solution {
public:
	vector<string> findWords(vector<string>& words) {
		unordered_set<char> row1{ 'q', 'w', 'e', 'r', 't', 'y','u', 'i', 'o', 'p' }; //使用关联容器
		unordered_set<char> row2{ 'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l' };
		unordered_set<char> row3{ 'z', 'x', 'c', 'v', 'b' ,'n', 'm' };
		vector<unordered_set<char>> rows{ row1, row2, row3 };
		vector<string> res;
		const int size = words.size();
		int subSize = 0; //记录words中每个单词的长度
		int rowNum = 0;
		bool matchFlag = true;  //匹配标志位
		for (int i = 0; i<size; i++) {
			//初始化
			rowNum = 0;
			matchFlag = true;

			for (int j = 0; j < 3; j++) {
				if (rows[j].count((char)tolower(words[i][0])) > 0) {
					rowNum = j;  //确定行数
					break;   //确定后直接跳出循环
				}
			}
			subSize = words[i].size();
			for (int k = 1; k < subSize; k++) {
				if (rows[rowNum].count((char)tolower(words[i][k])) == 0) {  //元素不在确定的行内
					matchFlag = false;
					break;
				}
			}
			if (matchFlag) {
				res.push_back(words[i]);
			}
		}
		return res;
	}
};
```

#### Method 2: JavaScript
* 方法1：参照思路2，使用正则表达式。 (Cost 129ms. Beats 0.65%)

#### Method 3: Java
* 方法1：参照思路2，使用正则表达式。 (Cost 129ms. Beats 0.65%)

```java
public class Solution {
    public String[] findWords(String[] words) {
        return Stream.of(words).filter(s -> s.toLowerCase().matches("[qwertyuiop]*|[asdfghjkl]*|[zxcvbnm]*")).toArray(String[]::new);
    }
}
```

##  Find All Numbers Disappeared in an Array(448)
### Description
* [LeetCode - 448. Find All Numbers Disappeared in an Array ](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/?tab=Description)
* Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.
* Find all the elements of [1, n] inclusive that do not appear in this array.
* Could you do it without extra space and in `O(n)` runtime? You may assume the returned list does not count as extra space.
* Example

```cpp
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

* Tags:  Array

### Analysis
* 思路1
	- 数组中的元素值的范围在[1,n]之间，数组index索引值的范围在[1,n-1]之间。利用二者之间的对应关系求解本题。即任意一个元素值k，将索引值(k-1)与之对应。
	- 两次变量数组。第1次遍历时，将元素值k对应的索引值的元素置为负数，即`nums[abs(nums[k])] = nums[abs(nums[k])]<0 ? nums[abs(nums[k])]:-nums[abs(nums[k])]`
	- 第2次遍历数组，若元素值为正数，则说明该索引值index对应的元素值index+1没有出现，返回该元素值即可。
	
### Slove
#### Method 1: C++
* 方法1：参照思路1分析。(Cost 122ms. Beats 99.39%)

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        int size = nums.size();
        int i;
        int temp;
        for(i=0;i<size;i++){
            temp = abs(nums[i])-1;
            nums[temp] = nums[temp]<0 ? nums[temp]:-nums[temp];
        }
        for(i=0;i<size;i++){
            if(nums[i]>0){
                res.push_back(i+1);
            }
        }
        return res;
    }
};
```
#### Method 2: JavaScript
* 方法1：原理同C++，此处不再赘述。参照思路1分析。(Cost 245ms. Beats 64.81%)

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var findDisappearedNumbers = function(nums) {
     var res = [];
     var size = nums.length;
     var i;
     var temp;
     for(i=0;i<size;i++){
        temp = Math.abs(nums[i])-1;
        nums[temp] = nums[temp]<0 ? nums[temp]:-nums[temp];
     }
     var index = 0;
     for(i=0;i<size;i++){
        if(nums[i]>0){
           res[index] = (i+1);
           index++;
        }
     }
     return res;
};
```
#### Method 3: Java
原理同C++，此处不再赘述。



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
