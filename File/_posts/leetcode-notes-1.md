---
title: LeetCode Notes - 1
date: 2017-02-20 15:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---

* [LeetCode](https://leetcode.com)
* 本文包括 [LeetCode - 258. Add Digits](https://leetcode.com/problems/add-digits/?tab=Description)， [LeetCode- 461. Hamming Distance](https://leetcode.com/problems/hamming-distance/)，[LeetCode- 191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)，[LeetCode- 476. Number Complement](https://leetcode.com/problems/number-complement/)和[LeetCode- 485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/?tab=Description)5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。

<!--more -->

## Add Digits(258)
### Description
* [LeetCode - 258. Add Digits](https://leetcode.com/problems/add-digits/?tab=Description)
* Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
* For example
Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.
* Follow up
Could you do it without any loop/recursion in `O(1)` runtime?
* Hint
    - A naive implementation of the above process is trivial. Could you come up with other methods?
    - What are all the possible results?
    - How do they occur, periodically or randomly?
    - You may find this [Wikipedia article](https://en.wikipedia.org/wiki/Digital_root) useful.

### Analysis
* Ref this [Wikipedia article](https://en.wikipedia.org/wiki/Digital_root).

![leetcode-258-add-digits.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-258-add-digits.png)
### Slove
#### Method 1: C++
* 方法1：Ref this [Wikipedia article](https://en.wikipedia.org/wiki/Digital_root). You should contain `<cmath>`head file first if you want to use `floor()`function. (Cost 6ms. Beats 11.98%)

```cpp
class Solution 
{
public:
    int addDigits(int num) 
    {
        return num-9*floor((num-1)/9);
    }
};
```

* 方法2： 利用`num%9`求解。注意判断num=0和num是9的倍数的情况。 (Cost 6ms. Beats 11.98%)

``` cpp
int addDigits(int num) {
    int res = num % 9;
    return (res != 0 || num == 0) ? res : 9;
}
```
#### Method 2: JavaScript
原理同C++方法，此处不再赘述。 (Cost 142ms. Beats 43.97%)
```javascript
/**
 * @param {number} num
 * @return {number}
 */
var addDigits = function(num) {
    var res = num%9;
    return (res !==0 || num===0)? res:9;
};
```
#### Method 3: Java
原理同C++方法，此处不再赘述。

## Hamming Distance(461)
### Description

* [LeetCode- 461. Hamming Distance](https://leetcode.com/problems/hamming-distance/)
* The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.
* Given two integers `x` and `y`, calculate the Hamming distance.
* Note: 0 ≤ x, y < 2^31.
* Example

```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

### Analysis

* In C++, `int __builtin_popcount (unsigned int x)` function can return the number of 1-bits in x. Read [Other Built-in Functions Provided by GCC](http://gcc.gnu.org/onlinedocs/gcc/Other-Builtins.html) and [Blog __builtin_popcount()](http://www.xuebuyuan.com/828691.html) for more information.
* Java provides [Integer.bitCount()](https://www.tutorialspoint.com/java/lang/integer_bitcount.htm) function. The `java.lang.Integer.bitCount()` method returns the number of one-bits in the two's complement binary representation of the specified int value i. This is sometimes referred to as the `population count`.
* In JavaScript, [number.toString(radix)](https://www.w3schools.com/jsref/jsref_tostring_number.asp) method  converts a number to a string.

### Solve
#### Method 1: C++

* 利用`num%2`和`num/2`不断取出数字`x`和`y`的二进制数值，并对其进行比较。 (Cost: 6ms)

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int total = 0; 
        while(x!=0 ||  y!= 0){
            if((x%2)!=(y%2)){
                total++;
            }
            x=x/2;
            y=y/2;
        }  
        return total;
    }
};
```

* 或者直接利用异或XOR运算`^`进行求解。(Cost: 6ms)

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int total = 0;
        int myXor = x^y;
        for(int i=0;i<31;i++){
            total += (myXor>>i) & 1; 
        }
        return total;
    }
};
```

*  或者使用GCC提供的` __builtin_popcount()`函数。(Cost: 3ms)

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return __builtin_popcount(x^y);
    }
};
```

#### Method 2: JavaScript
* Use `number.toString(radix)` method and regular expression. (Cost:  119ms)

```javascript
/**
 * @param {number} x
 * @param {number} y
 * @return {number}
 */
var hammingDistance = function(x, y) {
     return (x ^ y).toString(2).replace(/0/g, '').length;
};
```

* 或者使用异或运算符。(Cost:  119ms)

```javascript
/**
 * @param {number} x
 * @param {number} y
 * @return {number}
 */
var hammingDistance = function(x, y) {
    var total = 0;
    var xor = x^y;
    for(var i=0;i<32;i++){
            total += (xor>>i) & 1; 
    }
    return total;
};
```
 
####  Method 3: Java

* Use `Integer.bitCount` function. (Cost:  15ms)

``` java
public class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x^y);  //XOR
    }
}
```

## Number of 1 Bits(191)
### Description

* [LeetCode- 191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
* Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).
* For example, the 32-bit integer ’11' has binary representation 
00000000000000000000000000001011, so the function should return 3.

### Analysis
* Java provides [Integer.bitCount()](https://www.tutorialspoint.com/java/lang/integer_bitcount.htm) function. The `java.lang.Integer.bitCount()` method returns the number of one-bits in the two's complement binary representation of the specified int value i. This is sometimes referred to as the `population count`.
* In JavaScript, [number.toString(radix)](https://www.w3schools.com/jsref/jsref_tostring_number.asp) method  converts a number to a string.

### Solve

#### Method 1: C++
* 利用`num%2`和`num/2`不断取出数字的二进制数值。 (Cost: 6ms)

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int total = 0;
        while(n){
            total += n%2;
            n /=2;
        }
        return total;
    }
};
```

* 或者使用GCC提供的` __builtin_popcount()`函数。(Cost: 3ms)

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
       return __builtin_popcount(n);        
    }
};
```

#### Method 2: Java
* Use `Integer.bitCount` function. (Cost:  1ms)

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        return Integer.bitCount(n);
    }
}
```

#### Method 3: JavaScript
* Use `number.toString(radix)` method. (Cost:  132ms)

```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    return n.toString(2).replace(/0/g,'').length; 
};
```

## Number Complement(476)
### Description
* [LeetCode- 476. Number Complement](https://leetcode.com/problems/number-complement/)
* Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.
* Note
   - The given integer is guaranteed to fit within the range of a 32-bit signed integer. 
   -  You could assume no leading zero bit in the integer’s binary representation.
* Example 1
```
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.
```
* Example 2
```
Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
```

### Analysis

* Ref: [bit manipulation solution](https://discuss.leetcode.com/topic/74642/java-1-line-bit-manipulation-solution)
* 如果直接对num进行取反操作，会把符号位取反，并且把最高位为1之前的所有位数都取反。因此，需要先找到最高位为1的位置，并设置相应的mask，再将mask和~num进行与操作。

### Solve
#### Method 1: Java 
* 方法1：Use `Integer.highestOneBit()`method.  (Cost:  12ms. Beats 36.24%)
* 如果直接对num按位取反，则会将符号位同时取反，`~5=-6`。因此，需要在对num取反之后，再将其和`11...1`进行与操作。`11...1`的位数和num二进制数值的位数相同。

> The java.lang.Integer.highestOneBit() method returns an int value with at most a single one-bit, in the position of the highest-order ("leftmost") one-bit in the specified int value. It returns zero if the specified value has no one-bits in its two's complement binary representation, that is, if it is equal to zero.  
--- [Java.lang.Integer.highestOneBit() Method](https://www.tutorialspoint.com/java/lang/integer_highestonebit.htm)
>

例如，`Integer.highestOneBit(170)`的返回值是128。170的二进制值是10101010，因此该函数返回值是10000000。`Integer.highestOneBit(num)`返回的值一定是`2^N`的形式。

```java
public class Solution {
    public int findComplement(int num) {
        return ~num & ((Integer.highestOneBit(num)<<1)-1);
    }
}
```

#### Method 2: C++

* 参考资料： [Number Complement 补数](http://www.cnblogs.com/grandyang/p/6275742.html) 
* 方法1：利用异或对位进行翻转   (Cost:  3ms. Beats 20.50%)
分析题目可知，只需对每个位的二进制数值进行翻转即可。但是如果对num进行取反操作，会把符号位也取反。因此，对每位翻转（或取反）的起始位置是从最高位的1开始的，前面的0是不能被翻转的。因此，可以考虑从高位往低位遍历，当遇到第1个1后，设置一个标志位flag为true，标志着翻转操作开始。翻转操作可以通过异或操作实现。

```cpp
class Solution {
public:
    int findComplement(int num) {
        bool flag = false;
        for(int i=31;i>=0;i--){
            if((1<<i) & num){
                flag = true;
            }
            if(flag){
                num = num ^ (1<<i);
            }
        }
        return num;
    }
};
```

* 方法2：利用异或对位进行翻转   (Cost:  3ms. Beats 20.50%)
如果直接取反操作，会把最高位1之前的0也翻转，这是不希望的操作。因此，引入一个mask来标记最高位1之前所有0的位置。然后对mask取反后，再和~num进行与操作。`unsigned ~0`设置为无符号位，将返回`111...111`。然后利用位运算符求解最高位1的位置。若num=5，`unsigned mask = ~0`的值为11111111，while操作之后，mask为11111000，对mask取反之后为00000111，和~num（11111010）进行与操作之后，输出0000010，即2。

 
```cpp
class Solution {
public:
    int findComplement(int num) {
        unsigned mask = ~0;
        while (num & mask){
			mask <<=1;   //位运算求解最高位为1的位置
		} 
        return ~mask & ~num;
    }
};
```

* 方法3：递归函数   (Cost:  3ms. Beats 20.50%)
其思路为将num每次都右移一位，并根据最低位的值先进行翻转，即`(1 - num % 2)`。如果当前值小于等于1了（题目中给定的数都是正整数），停止递归。

```cpp
class Solution {
public:
    int findComplement(int num) {
         return (1 - num % 2) + 2 * (num <= 1 ? 0 : findComplement(num / 2));
    }
};
```


#### Method 3: JavaScript 
* 思路同Method 2（JAVA）。只是此处借助正则表达式查找来代替JAVA中的`Integer.heighestOneBit()`函数。 (Cost:  109ms. Beats 65.42%)
* 先将num转化为二进制字符串，然后借助正则表达式查找，将0替换成1。然后将该字符串值转化成整数Int，最后将该值和~num进行与操作。

```javascript
/**
 * @param {number} num
 * @return {number}
 */
var findComplement = function(num) {
    var str = num.toString(2).replace(/0/g,1);  //转换为11..11
    return (~num) & parseInt(str,2);
};
```

## Max Consecutive Ones (485)
### Description
* [LeetCode- 485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/?tab=Description)
* Given a binary array, find the maximum number of consecutive 1s in this array.
* Example 1

```
Input: [1,1,0,1,1,1]
Output: 3
```

* Explanation
	- The first two digits or the last three digits are consecutive 1s. 
	- The maximum number of consecutive 1s is 3.
* Note
    - The input array will only contain 0 and 1.
	- The length of input array is a positive integer and will not exceed 10,000

### Analysis
该题比较简单，下述C++，JAVA和JavaScript方法中的思路相同。注意JS中数组的长度是`myArray.length`，而C++中`vector`容器的大小是`myVector.size()`。
 

### Solve
#### Method 1: Java 
* 方法1: (Cost: 11ms. Beats 48.96%)

```java
public class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int max = 0;
        int count = 0;
        int length = nums.length;
        int i = 0;
        for(;i<length;i++){
            if(nums[i]==0){
                count = 0;   //计数值归零
            }
            else{
                count++;
                if(count>max){
                    max = count;  //记录最大值
                }
            }
        }
        return max; 
    }
}
```

#### Method 2: C++
* 方法1: (Cost: 39ms. Beats 46.33%)

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max = 0;
        int count = 0;
        int length = nums.size();
        int i = 0;
        for(;i<length;i++){
            if(nums[i]==0){
                count = 0;   //计数值归零
            }
            else{
                count++;
                if(count>max){
                    max = count;  //记录最大值
                }
            }
        }
        return max; 
    }
};
```

#### Method 3: JavaScript  
* 方法1: (Cost: 105ms. Beats 92.74%)

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function(nums) {
    var max = 0;
    var count = 0;
    var length = nums.length;
    var i = 0;
    for(;i<length;i++){
        if(nums[i]===0){
            count = 0;   //计数值归零
        }
        else{
            count++;
            if(count>max){
                max = count;  //记录最大值
            }
        }
    }
    return max; 
};
```

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
