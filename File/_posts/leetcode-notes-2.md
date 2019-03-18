---
title: LeetCode Notes - 2
date: 2017-02-23 23:46:26
categories: LeetCode
tags: [LeetCode,Programing,Algorithm]
keywords: LeetCode
toc: true
---



* [LeetCode](https://leetcode.com)
* 本文包括[LeetCode- 520. Detect Capital](https://leetcode.com/problems/hamming-distance/)，[LeetCode -  292.Nim Game](https://leetcode.com/problems/nim-game/?tab=Description)，[LeetCode -  326.Power of Three](https://leetcode.com/problems/power-of-three/?tab=Description)，[LeetCode - 231. Power of Two](https://leetcode.com/problems/power-of-two/)和[LeetCode - 371. Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/?tab=Description)5道题目，每道题目均给出了分析过程以及C++，JAVA，JavaScript代码实现。

<!--more -->



## Detect Capital(520)
### Description

* [LeetCode- 520. Detect Capital](https://leetcode.com/problems/detect-capital/?tab=Description)
* Given a word, you need to judge whether the usage of capitals in it is right or not.
* We define the usage of capitals in a word to be right when one of the following cases holds
	- All letters in this word are capitals, like "USA".
    - All letters in this word are not capitals, like "leetcode".
	- Only the first letter in this word is capital if it has more than one letter, like "Google".
	- Otherwise, we define that this word doesn't use capitals in a right way.
* Example 1

```
Input: "USA"
Output: True
```

* Example 2

```
Input: "FlaG"
Output: False
```

* Note: The input will be a non-empty word consisting of uppercase and lowercase latin letters.

### Analysis

* 本题解决方案可分为2大类。第1类是对字母的ASCII值进行比较。第2类方法是利用正则表达式解决。
* 对字母的ASCII值进行比较方法中，大写字母的ASCII值范围是`65~90`，小写字母的ASCII值范围是`97~122`。大写字母`A`的ASCII值是`65`，大写字母`Z`的ASCII值是`90`；小写字母`a`的ASCII值是`97`，小写字母`z`的ASCII值是`122`。

* JavaScript中，利用`charCodeAt(index)`函数可以获取指定字符串中index索引值处字母的ASCII值。利用`charAt(index)`函数可以获取指定字符串中index索引值处的字母。

```javascript
var word = "LiuBaoshuai";
console.log(word.charAt(3));   //B
console.log(word.charCodeAt(3));  //66
```

类似的，Java中的`charAt(index)`函数和`charPointAt(index)`函数分别对应JavaScript中的`charAt(index)`函数和`codeCodeAt(index)`函数。

Java中，`charAt(index)`函数可以返回字符串String中指定索引值index处的字母，函数返回类型是char。


> `public char charAt(int index)`: This method returns the character located at the String's specified index. The string indexes start from zero.   --- [Java - String charAt() Method](https://www.tutorialspoint.com/java/java_string_charat.htm)
> 
> `public static int Character.codePointAt(CharSequence seq, int index)`： Returns the code point at the given index of the CharSequence. --- [Java Tutorial](http://docs.oracle.com/javase/7/docs/api/java/lang/Character.html#codePointAt%28java.lang.CharSequence,%20int%29)
 
```java
//Java
 String word = "LiuBaoshuai";
 char myChar = charAt(word[3]);   // B
 Character.codePointAt(word,3);   // 66
```


* C++中，利用强制类型转换可以获取字母的ASCII值。C++中计算字符串的长度值的语法为`string.length()`，而JavaScript中对应的语法为`string.length`，注意二者区别。

```cpp
int myASCII = (int)myString[index];   // reuturn ASCII
```

* C++中，`islower()`和`isupper()`可以判断字母的大小写。返回值为int类型（0或1）。

> `int islower ( int c )`: Check if character is lowercase letter.
> `int isupper ( int c )`: Check if character is uppercase letter.
> You should contain `<ctype.h>` head file if you want to use these functions.   --- [Cplusplus.com](http://www.cplusplus.com/reference/cctype/islower/)

类似的，Java中的`Character.isUpperCase()`和`Character.isLowerCase()`函数分别对应C++中的`islower()`和`isupper()`函数。参考[Java - isUpperCase() Method](https://www.tutorialspoint.com/java/character_isuppercase.htm)和[Java - isLowerCase() Method](https://www.tutorialspoint.com/java/character_islowercase.htm)了解更多。

```java
String word = "LiuBaoshuai";
Character.isLowerCase(word.charAt(3));  // true
Character.isLowerCase(word.charAt(3));  //false
```

需要注意的是，`Character.isUpperCase()`和`Character.isLowerCase()`函数接受的参数是字符char类型，不能接收字符串String类型。因此，需要调用`charAt(inde)`函数进行类型转换。 



* 程序中，注意首先对字符串长度为1的情况进行判断。因为若后续程序中使用for循环且循环初始值为i=1，在字符串长度为1的情况下无法访问到。

```javascript
 //由于后续for循环中使用到了word[i]且i初始值为1，因此要先对wordLength=1的情况进行判断
if(wordLength == 1){  
	return true;       
}
```

### Slove
#### Method 1: C++
* 利用强制类型转换。(Cost: 9ms . Beats 39.81%) 

```cpp
class Solution {
public:
    bool detectCapitalUse(string word) {
		int wordLength = word.length();   //.length()
	    int codeFirst = (int)word[0];
	     //由于后续for循环中使用到了word[i]且i初始值为1，因此要先对wordLength=1的情况进行判断
        if(wordLength == 1){  
            return true;       
        }
		if(codeFirst <= 90){   //首字母大写   
			if((int)word[1] <= 90){  //第2个字母大写
				for(int i=2;i<wordLength;i++){
					int code = (int)word[i];
					if(code >= 97){    //后续字母中有小写
						return false;  //TESt
					}
				}
			}
			else{  //第2个字母小写
				for(int i=2;i<wordLength;i++){
					int code = (int)word[i];
					if(code <= 90){   //后续字母中出现大写
						return false;  //TesT
					}
				}
			}
		}
		else{  //首字母小写  
			for(int i=1;i<wordLength;i++){
			    int code = (int)word[i];
				if(code <= 90){  //检测到大写字母
					return false;
				}
			}
		}
		return true;	
	}
};
```

* 利用`islower()`和`isupper()`函数。(Cost: 12ms . Beats 19.81%) 

```cpp
class Solution {
public:
    bool detectCapitalUse(string word) {
        int wordLength = word.length();
        if(wordLength <= 1){
            return true;
        }
        if(islower(word[0]) || (isupper(word[0]) && islower(word[1]))){  
         //首字母小写 或者第1个字母大写并且第2个字母小写    
            for(int i=1;i<wordLength;i++){
                if(isupper(word[i])){  //后续出现大写字母
                    return false;   // gooGle   GooGle
                }
            } 
         }
         else{   //前两个字母均大写
             for(int i=1;i<wordLength;i++){
                 if(islower(word[i])){
                     return false;      //GOOgle
                 }
             }
         }
         return true;
	}
};
```

#### Method 2: JavaScript

* 方法1： Use `charCodeAt()`function. (Cost 116ms. Beats 79.37%)

```javascript
/**
 * @param {string} word
 * @return {boolean}
 */
var detectCapitalUse = function(word) {
    var wordLength = word.length;
     //由于后续for循环中使用到了word[i]且i初始值为1，因此要先对wordLength=1的情况进行判断
        if(wordLength == 1){  
            return true;       
        }
	var codeFirst = word.charCodeAt(0);
	if(codeFirst <= 90){   //首字母大写   
	    if(word.charCodeAt(1) <= 90){  //第2个字母大写
			for(var i=2;i<wordLength;i++){
				var code = word.charCodeAt(i);
				if(code >= 97){    //后续字母中有小写
			    	return false;  //TESt
				}
			}
		}
		else{  //第2个字母小写
	    	for(var i=2;i<wordLength;i++){
			    var code = word.charCodeAt(i);
				if(code <= 90){   //后续字母中出现大写
					return false;  //TesT
				}
	    	}
		}
	}
	else{  //首字母小写  
		for(var i=1;i<wordLength;i++){
		    var code = word.charCodeAt(i);
			if(code <= 90){  //检测到大写字母
				return false;
			}
		}
	}
	return true;
};
```



#### Method 3: Java

* 方法1： Use `codepointAt(str,indx)`function. (Cost 29ms. Beats 83.11%)

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
        int wordLength = word.length();
        if(wordLength == 1){
            return true;
        }
        if(Character.codePointAt(word,0) >= 97 || (Character.codePointAt(word,0) <= 90 && Character.codePointAt(word,1) >= 97)){
            //第1个字母小写 或者第1个字母大写并且第2个字母消息
            for(int i=1;i<wordLength;i++){
                if(Character.codePointAt(word,i) <= 90){  //后续字母中出现大写字母
                    return false;
                }
            }
        }
        else{
            //前两个字母均大写
             for(int i=1;i<wordLength;i++){
                if(Character.codePointAt(word,i) >= 97){  //后续字母中出现小写字母
                    return false;
                }
            }
        }
        return true;
    }
}

```

* 方法2：Use `charAt()` function. (Cost 38ms. Beats 25.64%)

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
        int wordLength = word.length();
        //由于后续for循环中使用到了word[i]且i初始值为1，因此要先对wordLength=1的情况进行判断
        if(wordLength == 1){  
            return true;       
        }
        if(word.charAt(0) >= 97 || (word.charAt(0) <= 90 && word.charAt(1) >= 97)){
            //第1个字母小写 或者第1个字母大写并且第2个字母消息
            for(int i=1;i<wordLength;i++){
                if(word.charAt(i) <= 90){  //后续字母中出现大写字母
                    return false;
                }
            }
        }
        else{
            //前两个字母均大写
             for(int i=1;i<wordLength;i++){
                if(word.charAt(i) >= 97){  //后续字母中出现小写字母
                    return false;
                }
            }
        }
        return true;
    }
}
```

* 方法3： 使用`isLowerCase()`和`isUpperCase()`函数。 (Cost 42ms. Beats 16.40%)

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
        int wordLength = word.length();
        if(wordLength == 1) return true;
        if(Character.isLowerCase(word.charAt(0)) || (Character.isUpperCase(word.charAt(0)) && Character.isLowerCase(word.charAt(1)))){
            //首字母小写 或者第1个字母大写并且第2个字母小写 
            for(int i=1;i<wordLength;i++){
                if(Character.isUpperCase(word.charAt(i))) return false;
            }
        }
        else{
            for(int i=1;i<wordLength;i++){
                if(Character.isLowerCase(word.charAt(i))) return false;
            }
        }
        return true;
    }
}
```

* 方法4： Use RegEex. (Cost 62ms. Beats 2.07%)

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
         return word.matches("[A-Z]+|[a-z]+|[A-Z][a-z]+");
    }
}
```
See [Java - String matches() Method](https://www.tutorialspoint.com/java/java_string_matches.htm) for more information.

* 方法5： Simple Java Solution O(n) time O(1) space. (Cost 43ms. Beats 14.58%)

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
        return word.equals(word.toUpperCase()) || 
               word.equals(word.toLowerCase()) ||
               Character.isUpperCase(word.charAt(0)) && 
               word.substring(1).equals(word.substring(1).toLowerCase());
    }
}
```


 

## Nim Game(292)
### Description
* [LeetCode -  292.Nim Game](https://leetcode.com/problems/nim-game/?tab=Description)
* You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.
* Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.
* For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.
* Hint
If there are 5 stones in the heap, could you figure out a way to remove the stones such that you will always be the winner?

### Analysis
![leetcode-292-nim-game.png](http://ol3kbaay9.bkt.clouddn.com/leetcode-292-nim-game.png)
* When the total of stones is 4, A is bound to lose.
* When the total of stones is 5/6/7, if A takes 1/2/3 stones firstly, then B will face 4 stones, so B is bound to lose.
* When the total of stones is 8, no matter what stones A takes, B can left 4 stones to A, so A is bound to lose.
* So, we can find that, if one is facing stones whose total number is 4 or multiples of 4, he is bound to loose.


### Slove

#### Method 1: C++

```cpp
class Solution {
public:
    bool canWinNim(int n) {
        return n%4;
    }
};
```

#### Method 2: JavaScript
原理同C++方法，此处不再赘述。

```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var canWinNim = function(n) {
    return (n%4) === 0 ? false:true;
};
```

#### Method 3: Java
原理同C++方法，此处不再赘述。


## Power of Three(326)
### Description
* [LeetCode -  326.Power of Three](https://leetcode.com/problems/power-of-three/?tab=Description)
* Given an integer, write a function to determine if it is a power of three.
* Follow up: Could you do it without using any loop / recursion?

### Analysis
* Ref: [A summary of all solutions - LeetCode-326.Power of Three](https://discuss.leetcode.com/topic/33536/a-summary-of-all-solutions-new-method-included-at-15-30pm-jan-8th)
* Method 1
	- Due to the total bits of “int” is 32, so the maximum data is `2^31`. So the maximum data of power of three within the range is `3^19=1162261467`.
	- If the number is can be divided by 3^19, it must be a power of three.
	- It is worthwhile to mention that Method 1 works only when the base is prime(质数).
	- For example, we cannot use this algorithm to check if a number is a power of 4 or 6 or any other composite number.

```cpp
class Solution
{
public:
	bool isPowerOfThree(int n)
	{
		return ((n > 0) && ((1162261467 % n) == 0));
	}
};
```

* Method 2
	- Due to `3^x=n`, it can also be putted in the form of `x=log3(n)=log10(n)/log10(3)`.
	- So, if `x` is an integer, `n` must be a power of three.
	- In C++, the expression `int(a)-a == 0` can be used to judge whether a is an integer.
	- Be careful here, you cannot use log (natural log) here, because it will generate round off error for n=243. This is more like a coincidence. I mean when n=243, we have the following results
	- log(243) = 5.493061443340548    log(3) = 1.0986122886681098
      ==> log(243)/log(3) = 4.999999999999999
   - log10(243) = 2.385606273598312    log10(3) = 0.47712125471966244
      ==> log10(243)/log10(3) = 5.0
   - This happens because log(3) is actually slightly larger than its true value due to round off, which makes the ratio smaller.


```cpp
class Solution
{
public:
	bool isPowerOfThree(int n)
	{
		return (n > 0 && int(log10(n) / log10(3)) - log10(n) / log10(3) == 0);
	}
};
```

* Method 3
 利用正则表达式求解。若n是3的整数幂，则将n转换为3进制数，则其一定可以表示成`100..00`的形式，其正则表达式形式为`/10*/`。

### Slove
#### Method 1: C++
* 方法1：递归函数（Recursive） (Cost 52ms. Beats 78%)

```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
          return n>0 && (n==1 || (n%3==0 && isPowerOfThree(n/3)));
    }
};
```

* 方法2：使用循环  (Cost 72ms. Beats 14.75%)

```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        if(n>1){
            while(n%3==0){
                n /= 3;
            }
        }
    return n==1; 
    }
};
```

* 方法3：  (Cost 69ms. Beats 19.02%)

```cpp
class Solution
{
public:
	bool isPowerOfThree(int n)
	{
		return (n > 0 && int(log10(n) / log10(3)) - log10(n) / log10(3) == 0);
	}
};
```

* 方法4：  (Cost 56ms. Beats 58.10%)

```cpp
class Solution
{
public:
	bool isPowerOfThree(int n)
	{
		return ((n > 0) && ((1162261467 % n) == 0));
	}
};
```

#### Method 2: JavaScript
基本方法同C++。此处对利用正则表达式求解的方法进行介绍。
 
#### Method 3: Java
* 方法1： (Cost 21ms. Beats 25.03%)

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return (Math.log10(n) / Math.log10(3)) % 1 == 0;
    }
}
```

**注意，C++中不能使用该方法。C++中`log10()`返回的是double类型，而取整运算符`%`需要`int`型参数。**
 

* 方法2：  (Cost 66ms. Beats 3.26%)
The idea is to convert the original number into radix-3 format and check if it is of format `10*` where `0*` means `k` zeros with `k>=0`.

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return Integer.toString(n, 3).matches("10*");
    }
}
```

>  `toString()`: The method is used to get a String object representing the value of the Number Object. Eg, `Integer.toString(12,2)` return `1100`.
--- [Java - toString() Method](https://www.tutorialspoint.com/java/number_tostring.htm)




## Power of Two(326)
### Description
* [LeetCode - 231. Power of Two](https://leetcode.com/problems/power-of-two/)
* Given an integer, write a function to determine if it is a power of two.

### Analysis
* 本题和`Power of Three`类似。相似的方法原理不再赘述。
* 除了类似于`Power of Three`中介绍的方法外，此处再介绍一种方法。若`num`是2的整数幂，则其二进制数值为`100..00`的形式（可以利用正则表达式查询求解），`num-1`的二进制数值为`011...1`的形式。因此，`n & (n-1)`一定为0。利用该方法也可以求解。
* 参考[LeetCode Solutions](https://leetcode.com/problems/power-of-two/?tab=Solutions)了解不同算法。


### Slove
#### Method 1: C++
此处不再赘述和`Power of Three`习题同样的方法，介绍一些其他算法。

* 方法1： 利用`n & (n-1)`求解。 (Cost 6ms. Beats 4.10%)

```cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return (n>0) && !(n & (n-1));
    }
};
```

* 方法2

```cpp
class Solution {
public:
    bool isPowerOfTwo(int n)
    {
        return ((n>0) && (((int)(log10(n)/log10(2))-(log10(n)/log10(2))==0)));
    }
};
```

* 方法3

```cpp
class Solution {
public:
    bool isPowerOfTwo(int n)
    {
        if(n>0)
        {
            while((n%2)==0)
            {
                n/=2;
            }
            if(n==1) return true;
            else return false;
        }
        else return false;
    }
};
```

#### Method 2: JavaScript
算法原理同C++，此处不再赘述。
  
#### Method 3: Java
算法原理同C++，此处不再赘述。

## Sum of Two Integers(371)
### Description
* [LeetCode - 371. Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/?tab=Description)
* Calculate the sum of two integers a and b, but you are not allowed to use the operator `+` and `-`.
* Example: Given a = 1 and b = 2, return 3.

### Analysis
* Reference [A summary: how to use bit manipulation to solve problems](https://discuss.leetcode.com/topic/50315/a-summary-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently). 
* 利用异或运算，`sum=a^b`可以求解在无进位情况下的“加法”的和。
* 若`a+b`发生进位，则对应的位上a和b的二进制值均为1。利用与运算可以求解。由于要进位，因此再将与操作的结果向左移动一位即可。即`carry = (a&b)<<1`。

### Slove
#### Method 1: C++

* 方法1：进行位操作，递归函数实现 (Cost 3ms. Beats 2.51% )

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        if(b==0) return a;
        int sum = a^b;
        int carry = (a&b)<<1;
        return getSum(sum,carry);
    }
};
//或者将其简写为1行
class Solution {
public:
    int getSum(int a, int b) {
        return (b==0)? a: getSum(a^b,(a&b)<<1);
    }
};
```

* 方法2：进行位操作，while循环实现 (Cost 3ms. Beats 2.51% )

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        int carry;
        while(b){
            carry = a & b; 
            a = a ^ b;   //sum
            b = carry <<1;  //carry
        }
        return a;
    }
};
```
#### Method 2: JavaScript
算法原理同C++，此处不再赘述。(Cost 132ms. Beats 3.17% )

```javascript
/**
 * @param {number} a
 * @param {number} b
 * @return {number}
 */
var getSum = function(a, b) {
     var carry;
        while(b){
            carry = a & b; 
            a = a ^ b;   //sum
            b = carry <<1;  //carry
        }
        return a;
};
```
#### Method 3: Java
算法原理同C++，此处不再赘述。

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
