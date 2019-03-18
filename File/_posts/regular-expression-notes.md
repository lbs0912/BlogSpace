---
title: Regular Expression-Notes
date: 2017-02-15 20:32:27
tags: [Regular Expression]
categories: Programing
toc: true
---


* The paper is mainly written for recording knowledge points of regular expression and problems encounted.
* Read [Regular_Expressions Tutorial](http://deerchao.net/tutorials/regex/regex.htm) to learn RegExp
* [Regular_Expressions - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
* [Regular_Expressions - MSDN](https://msdn.microsoft.com/zh-cn/library/az24scfc.aspx)
* [Regular_Expressions - Runoob](http://www.runoob.com/regexp/regexp-syntax.html)

> `Regular expressions` are patterns used to match character combinations in strings. **In JavaScript, regular expressions are also objects.** These patterns are used with the exec and test methods of RegExp, and with the match, replace, search, and split methods of String. This chapter describes JavaScript regular expressions.  --- [Regular_Expressions - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

<!-- more -->


## Regular Expression Syntax
Visit [Regex Cheatsheet](http://ol3kbaay9.bkt.clouddn.com/davechild_regular-expressions.pdf) to learn it. You can also reference the following links

* [Regex Sheet](http://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
* [Regular_Expressions Tutorial](http://deerchao.net/tutorials/regex/regex.htm)

![regexp-sheet-zh.png](http://ol3kbaay9.bkt.clouddn.com/regexp-sheet-zh.png)

## Creating a regular expression

* Method 1: using a regular expression literal, which consists of a pattern enclosed between slashes

```javascript
var re = /ab+c/;
```

* Method 2: calling the constructor function of the RegExp object

```javascript
var re = new RegExp('ab+c');
```

## Writing a regular expression pattern

### Using simple patterns
Simple patterns are constructed of characters for which you want to find a direct match. For example, the pattern `/abc/` matches character combinations in strings only when exactly the characters `'abc' `occur together and in that order. 

### Using special characters
You can visit [Regular_Expressions - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) to check the special characters that can be used in regular expressions. There is a snapshoot of the special characters in regular expressions.
![regexp-special-character.png](http://ol3kbaay9.bkt.clouddn.com/regexp-special-character.png)

* `*`: matches the preceding expression 0 or more times and it is equivalent to `{0,}`.
* `+`: matches the preceding expression 1 or more times and it is equivalent to `{1,}`.
* `?`: matches the preceding expression 0 or 1 times and it is equivalent to `{0,1}`.

### Using parentheses

## Working with regular expressions
Regular expressions are used with the RegExp methods `test` and `exec` and with the String methods `match`, `replace`, `search`, and `split`. These methods are explained in detail in the [JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference).

![regexp-method.JPG](http://ol3kbaay9.bkt.clouddn.com/regexp-method.JPG)

* `regex.test(str)`
* When you want to know whether a pattern is found in a string, use the `test` or `search` method; 
* For more information (but slower execution),  use the `exec` or `match` methods. If you use `exec` or `match` and if the match succeeds, these methods return an array and update properties of the associated regular expression object and also of the predefined regular expression object, RegExp. If the match fails, the exec method returns null (which coerces to false).

```javascript
var myRe = /d(b+)d/g;
var myArray = myRe.exec('cdbbdbsbz');
```
With these scripts, the match succeeds and returns the array and updates the properties shown in the following table.

![regexp-properties.JPG](http://ol3kbaay9.bkt.clouddn.com/regexp-properties.JPG)

### Using parenthesized substring matches
Including parentheses in a regular expression pattern causes the corresponding submatch to be remembered. For example, `/a(b)c/` matches the characters `'abc'` and remembers `'b'`. To recall these parenthesized substring matches, use the Array elements `[1]`, ..., `[n]`.

The following script uses the `replace()` method to switch the words in the string. For the replacement text, the script uses the `$1` and `$2` in the replacement to denote the first and second parenthesized substring matches.

```javascript
var re = /(\w+)\s(\w+)/;
var str = 'John Smith';
var newstr = str.replace(re, '$2, $1');
console.log(newstr);   // Smith, John
```

### Advanced searching with flags
Regular expressions have four optional flags that allow for global and case insensitive searching. These flags can be used separately or together in any order, and are included as part of the regular expression.

![regexp-flags.JPG](http://ol3kbaay9.bkt.clouddn.com/regexp-flags.JPG)

```javascript
var re = /pattern/flags;
var re = new RegExp('pattern', 'flags');
```
* `y`: perform a "sticky" search that matches starting at the current position in the target string. See [sticky](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/sticky)
* Note that the flags are an integral part of a regular expression. They cannot be added or removed later.

## FAQ
* [Regular Expression Online Test Tool by JavaScript ](http://www.regexpal.com/)

### Realize `trim()`  in JS
```javascript
String.prototype.trim = function () {
return this.replace(/(^\s*)|(\s*$)/g,'');
};
```

* `(^\s*)` can match a string which starts with 0 or 1(or other) white space character. `(\s*$)` can match a string which ends with 0 or 1(or other) white space character.
* Ref [trim - MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/Trim) for more information.


## Contact & Feedback
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 