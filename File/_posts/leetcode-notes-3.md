---
title: LeetCode Notes - 3
date: 2017-02-24 15:35:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---



* [LeetCode](https://leetcode.com)
* 本文包括[LeetCode - 223. Rectangle Area](https://leetcode.com/problems/rectangle-area/?tab=Description)， [LeetCode -  204. Count Primes](https://leetcode.com/problems/count-primes/?tab=Description)，[LeetCode -  344.  Reverse String](https://leetcode.com/problems/reverse-string/?tab=Description)，[LeetCode -  197. Rising Temperature](https://leetcode.com/problems/rising-temperature/?tab=Description) 和[LeetCode - 136. Single Number](https://leetcode.com/problems/single-number/?tab=Description) 5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。

<!--more -->



## Rectangle Area(223)
### Description
* [LeetCode - 223. Rectangle Area](https://leetcode.com/problems/rectangle-area/?tab=Description)
* Find the total area covered by two rectilinear rectangles in a 2D plane.
* Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.
![rectangle_area.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-223-rectangle-area.png)
* Assume that the total area is never beyond the maximum possible value of int.

### Analysis
* 根据上图，可以确定计算的面积值为两个矩形面积的和再减去重合部分的面积，即`resultArea = recArea1+recArea2-repetitionArea`
* 重合部分定点坐标的确定
	- Bottom Left Point Coordinate: `(max(A,E), max(B,F))`
	- Top Right Point Coordinate: `(min(C,G), min(D,H))`
* 易忽略点
> Due to the total bits of “int” is 32, we prefer to use ”w2<=w1 ” to ”(int) w2-w1” to judge the return value. The data of ”(int) w2-w1” may overflow the range of integer.(For example, w2>0 and w1<0)

### Slove
#### Method 1: C++
* 方法1：利用`resultArea = recArea1+recArea2-repetitionArea`计算。 (Cost 29ms. Beats 7.47%)


```cpp
class Solution {
public:
	int computeArea(int A, int B, int C, int D, int E, int F, int G, int H)
	{
		int area_1 = (C - A)*(D - B);
		int area_2 = (G - E)*(H - F);
		//Calculate the overlap area
		int w1 = max(A, E);
		int h1 = max(B, F);
		int w2 = min(C, G);
		int h2 = min(D, H);
	
		if ((w2<=w1) || (h2<=h1)) //没有重合部分
		{
			return area_1 + area_2;
		}
		else
		{
			return area_1 + area_2 - ((h2-h1)*(w2-w1));
		}
	}
};
```

* 方法2： 原理同方法1，但在计算过程上进行优化。(Cost 16ms. Beats 45.83%)

```cpp
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int left = max(A,E), right = max(min(C,G), left);
        int bottom = max(B,F), top = max(min(D,H), bottom);
        return (C-A)*(D-B) - (right-left)*(top-bottom) + (G-E)*(H-F);
    }
};
```
#### Method 2: JavaScript
* 原理同C++，此处不再赘述。(Cost 185ms. Beats 46.88%)
* C++中求最大最小值函数为`max(num1,num2)`和`min(num1,num2)`。JavaScript中对应的函数为`Math.max(num1,num2)`和`Math.min(num1,num2)`。

```javascript
var computeArea = function(A, B, C, D, E, F, G, H) {
    var left = Math.max(A,E), right = Math.max(Math.min(C,G), left);
    var bottom = Math.max(B,F), top = Math.max(Math.min(D,H), bottom);
    return (C-A)*(D-B) - (right-left)*(top-bottom) + (G-E)*(H-F);
};
```
#### Method 3: Java
* 原理同C++，此处不再赘述。
* C++中求最大最小值函数为`max(num1,num2)`和`min(num1,num2)`。Java中对应的函数为`Math.max(num1,num2)`和`Math.min(num1,num2)`。 (Cost 8ms. Beats 3.71%)
 
 ```java
 public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int left = Math.max(A,E), right = Math.max(Math.min(C,G), left);
        int bottom = Math.max(B,F), top = Math.max(Math.min(D,H), bottom);
        return (C-A)*(D-B) - (right-left)*(top-bottom) + (G-E)*(H-F);
    }
}
 ```
 
## Count Primes(204)
### Description
* [LeetCode -  204. Count Primes](https://leetcode.com/problems/count-primes/?tab=Description)
* Count the number of prime numbers less than a non-negative number, n.
* Let's start with a `isPrime` function. To determine if a number is prime, we need to check if it is not divisible by any number less than n. The runtime complexity of isPrime function would be O(n) and hence counting the total prime numbers up to n would be O(n^2). Could we do better?
* As we know the number must not be divisible by any number > n / 2, we can immediately cut the total iterations half by dividing only up to n / 2. Could we still do better?
* Let's write down all of 12's factors

```cmake
2 × 6 = 12
3 × 4 = 12
4 × 3 = 12
6 × 2 = 12
```
* As you can see, calculations of `4 × 3` and `6 × 2` are not necessary. Therefore, we only need to consider factors up to `√n` because, if `n` is divisible by some number `p`, then n = p × q and since p ≤ q, we could derive that `p ≤ √n`.
* Our total runtime has now improved to O(n^1.5), which is slightly better. Is there a faster approach?

```java
public int countPrimes(int n) {
   int count = 0;
   for (int i = 1; i < n; i++) {
      if (isPrime(i)) count++;
   }
   return count;
}

private boolean isPrime(int num) {
   if (num <= 1) return false;
   // Loop's ending condition is i * i <= num instead of i <= sqrt(num)
   // to avoid repeatedly calling an expensive function sqrt().
   for (int i = 2; i * i <= num; i++) {
      if (num % i == 0) return false;
   }
   return true;
}
```

* The [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) is one of the most efficient ways to find all prime numbers up to n. But don't let that name scare you, I promise that the concept is surprisingly simple.

![Sieve_of_Eratosthenes_animation.gif](http://ol3kbaay9.bkt.clouddn.com/Sieve_of_Eratosthenes_animation.gif)

* We start off with a table of n numbers. Let's look at the first number, 2. We know all multiples of 2 must not be primes, so we mark them off as non-primes. Then we look at the next number, 3. Similarly, all multiples of 3 such as 3 × 2 = 6, 3 × 3 = 9, ... must not be primes, so we mark them off as well. Now we look at the next number, 4, which was already marked off. What does this tell you? Should you mark off all multiples of 4 as well?
* 4 is not a prime because it is divisible by 2, which means all multiples of 4 must also be divisible by 2 and were already marked off. So we can skip 4 immediately and go to the next number, 5. Now, all multiples of 5 such as 5 × 2 = 10, 5 × 3 = 15, 5 × 4 = 20, 5 × 5 = 25, ... can be marked off. There is a slight optimization here, we do not need to start from 5 × 2 = 10. Where should we start marking off?
* In fact, we can mark off multiples of 5 starting at 5 × 5 = 25, because 5 × 2 = 10 was already marked off by multiple of 2, similarly 5 × 3 = 15 was already marked off by multiple of 3. Therefore, if the current number is p, we can always mark off multiples of p starting at `p^2`, then in increments of p: p^2 + p, p^2 + 2p, ... Now what should be the terminating loop condition?
* It is easy to say that the terminating loop condition is `p < n`, which is certainly correct but not efficient. 
* Yes, the terminating loop condition can be `p < √n`, as all `non-primes ≥ √n` must have already been marked off. When the loop terminates, all the numbers in the table that are non-marked are prime.
* The Sieve of Eratosthenes uses an extra `O(n)` memory and its runtime complexity is O(n log log n). For the more mathematically inclined readers, you can read more about its algorithm complexity on [Wikipedia](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Algorithm_complexity).

```java
public int countPrimes(int n) {
   boolean[] isPrime = new boolean[n];
   for (int i = 2; i < n; i++) {
      isPrime[i] = true;
   }
   // Loop's ending condition is i * i < n instead of i < sqrt(n)
   // to avoid repeatedly calling an expensive function sqrt().
   for (int i = 2; i * i < n; i++) {
      if (!isPrime[i]) continue;
      for (int j = i * i; j < n; j += i) {
         isPrime[j] = false;
      }
   }
   int count = 0;
   for (int i = 2; i < n; i++) {
      if (isPrime[i]) count++;
   }
   return count;
}
```
### Analysis
* Reference: [Count Primes—4种方法求解](http://blog.csdn.net/xy010902100449/article/details/49361263)

### Slove
#### Method 1: C++
* 方法1：参考题目提示。(Cost 29ms. Beats 80.23%)

```cpp
class Solution 
{
public:
	int countPrimes(int n)
	{
		vector <bool> Flag(n,true);
		int sum = 0;
		int sqrtN = ceil(sqrt(n));
		Flag[0] = false;
		for (int i = 2;i < sqrtN;i++) 
		{
			if(Flag[i-1])
			{
				for (int j = i*i;j <= n;j += i)
				{
					Flag[j - 1] = false;
				}
			}
			
		}
		for (int k = 0;k < (n-1);k++) 
		{
			if (Flag[k]) sum++;
		}
		return sum;
	}
};
```
* 注意，循环终止条件`i<sqrt(n)`中`sqrt()`函数返回的是double类型。若对其取整的话，应该向上取整，调用`ceil()`函数。不能向下取整。例如，对于n=5来说，i终止条件应该是`i<3`，而不是`i<2`。
* 利用`(i*i)<n`可以避免`i<sqrt(n)`的计算。

#### Method 2: JavaScript
原理同C++中的方法，此处不再赘述。
 
#### Method 3: Java
* 方法1：参考题目提示。(Cost 36ms. Beats 38.77%)

```java
public class Solution {
   public int countPrimes(int n) {
       boolean[] isPrime = new boolean[n];
       for (int i = 2; i < n; i++) {
          isPrime[i] = true;
       }
       // Loop's ending condition is i * i < n instead of i < sqrt(n)
       // to avoid repeatedly calling an expensive function sqrt().
       for (int i = 2; i * i < n; i++) {
          if (!isPrime[i]) continue;
          for (int j = i * i; j < n; j += i) {
             isPrime[j] = false;
          }
       }
       int count = 0;
       for (int i = 2; i < n; i++) {
          if (isPrime[i]) count++;
       }
       return count;
    }
}
```

* 方法2：参考方法1，但是在算法实现上进行优化，省去最后的`count++`for循环。(Cost 27ms. Beats 81.45%)

```java
public class Solution {
    public int countPrimes(int n) {
        boolean[] notPrime = new boolean[n];
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (notPrime[i] == false) {
                count++;
                for (int j = 2; i*j < n; j++) {
                    notPrime[i*j] = true;
                }
            }
        }
        
        return count;
    }
}
```

## Reverse String(344)
### Description
* [LeetCode -  344.  Reverse String](https://leetcode.com/problems/reverse-string/?tab=Description)
* Write a function that takes a string as input and returns the string reversed.

### Analysis
* Reference: [LeetCode : 344. Reverse String - CNBlogs](http://www.cnblogs.com/sthv/p/5457464.html)

### Slove
#### Method 1: C++

* 方法1：基本方法。(Cost 9ms. Beats 31.88%)

```cpp
class Solution {
public:
    string reverseString(string s) {
        string temp(s);
        int i=0,len=0;
        len = s.length();
        for(int i=0;i<len;i++){
            temp[i]=s[len-1-i];
        }
        return temp;
    }
};
```

* 方法2：采用STL库中的`reverse()`函数。(Cost 9ms. Beats 31.88%)

> `template < BidirectionalIterator> void reverse (BidirectionalIterator first, BidirectionalIterator last);` 
>
>Reverses the order of the elements in the range [first,last).
> The function calls `iter_swap` to swap the elements to their new locations.


```cpp
class Solution {
public:
    string reverseString(string s) {
        reverse(s.begin(),s.end());
        return s;
    }
};
```
`string.begin()`和 `string.end()`使用了迭代器操作。
 
#### Method 2: JavaScript
* 方法1： 常规解法。(Cost 132ms. Beats 83.79%)

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var reverseString = function(s) {
	    var arr = [];
	    var length = s.length;
	    for(var i=0;i<length;i++){
	        arr[i] = s[length-1-i];
	    }
	    return arr.join("");
};
```
* 方法2：Oneline Slove - `myString.split("").reverse().join("")`。(Cost 179ms. Beats 18.27%)

```javascript
var reverseString = function(s) {
    return s.split("").reverse().join("");
};
```

> `Array.prototype.reverse()`--- The reverse() method reverses an **array** in place. The first array element becomes the last, and the last array element becomes the first.  --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
>


* `Array.prototype.reverse()`函数的传入参数是Array数组类型。因此，若要对字符串String进行反转操作，需要先将其转换为数组Array类型。利用`split()`和`join()`函数可以实现该操作。

```javascript
var str = "LiuBaoshuai";
var newStr = str.split("").reverse().join("");
console.log(str);  // LiuBaoshuai
console.log(newStr);  // iauhsoaBuiL
```

* 调用`Array.prototype.reverse()`函数时，会改变传入的Array参数。

```javascript
var a = ['one', 'two', 'three'];
var reversed = a.reverse(); 

console.log(a);        // ['three', 'two', 'one']
console.log(reversed); // ['three', 'two', 'one']
```

> The `split()` method splits a **String** object into an **array** of strings by separating the string into substrings.
> The `join()` method joins all elements of an **array** (or an array-like object) into a **string**.

#### Method 3: Java

* 方法1： 使用`StringBuffer`对字符串操作。(Cost 7ms. Beats 8.22%)
**由于Java中String内容不能更改（只读），所以需创建StringBuffer对字符串进行操作。**

```java
public class Solution {
    public String reverseString(String s) {
    	StringBuffer s1 = new StringBuffer();
    	for(int i = s.length()-1;i>=0;i--)
    	{
    		s1.append(s.charAt(i));
    	}
    	return s1.toString();
    }
}
```


##  Rising Temperature(197)
### Description
* [LeetCode -  197. Rising Temperature](https://leetcode.com/problems/rising-temperature/?tab=Description)
* Given a Weather table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.

```sql
+---------+------------+------------------+
| Id(INT) | Date(DATE) | Temperature(INT) |
+---------+------------+------------------+
|       1 | 2015-01-01 |               10 |
|       2 | 2015-01-02 |               25 |
|       3 | 2015-01-03 |               20 |
|       4 | 2015-01-04 |               30 |
+---------+------------+------------------+
```
* For example, return the following Ids for the above Weather table

```sql
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```

### Analysis
* The function of `DATEDIFF(A,B)` can return the days between A and B.
* `TO_DAYS` function in MySQL gives the number of days between year 0 and the date parameter.

### Slove
#### Method 1

```sql
SELECT w1.Id FROM Weather AS w1,Weather AS w2 WHERE w1.Temperature >w2.temperature AND DATEDIFF(w1.Date,w2.Date) = 1;
```

#### Method 2

```sql
SELECT w1.Id FROM Weather AS w1,Weather AS w2 WHERE w1.Temperature >w2.temperature AND TO_DAYS(w1.Date) - TO_DAYS(w2.Date) = 1;
```



## Single Number(136)
### Description
* [LeetCode - 136. Single Number](https://leetcode.com/problems/single-number/?tab=Description)
* Given an array of integers, every element appears twice except for one. Find that single one.
* Note: Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

### Analysis
* 使用异或XOR实现。将数组中所有的数值进行XOR操作，出现2次的数进行异或返回值是0，不会影响最后的结果。

```cmake
0 ^ N = N
N ^ N = 0
```
* So, if N is the single number

```cmake
N1 ^ N1 ^ N2 ^ N2 ^..............^ Nx ^ Nx ^ N
= (N1^N1) ^ (N2^N2) ^..............^ (Nx^Nx) ^ N
= 0 ^ 0 ^ ..........^ 0 ^ N
= N
```


### Slove
#### Method 1: C++
* 方法1： 进行XOR操作。 (Cost 12ms. Beats 93.84% )

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for(int i = 0;i<nums.size();i++){
            result = result ^ nums[i];
        }
        return result;
    }
};
```
> 使用`^=`操作符比`= .. ^ ..`更耗时。此题中若使用`result ^= nums[i]`，会耗时16ms，击败 35.64%。
 
 

#### Method 2: JavaScript
* 方法1： 进行XOR操作。 (Cost 85ms. Beats 93.09% )

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    var length = nums.length;
    if(length===1){
        return nums[0];
    }
    else{
        for(var i=1;i<length;i++){
            nums[0] = nums[0] ^ nums[i];
        }
    }
    return nums[0];
};
```

#### Method 3: Java

* 方法1： 进行XOR操作。 (Cost 1ms. Beats 37.42% )

```java
public class Solution {
    public int singleNumber(int[] nums) {
        int length = nums.length;
        if(length==1){
            return nums[0];
        }
        else{
            for(int i=1;i<length;i++){
                nums[0] = nums[0] ^ nums[i];
            }
        }
        return nums[0];
    }
}
```


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
