---
title: 华为研发工程师上机笔试题
date: 2017-03-24 20:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,华为笔试]
keywords: 校招笔试编程题,华为笔试
---

* 本文主要对华为笔试题进行记录和归类。
* 在线练习本文中的试题：[牛客网](https://www.nowcoder.com/test/1088888/summary)
* 下载本文中的[华为笔试试题](http://static.nowcoder.com/pdf/paper/%E5%8D%8E%E4%B8%BA%E7%A0%94%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88%E7%BC%96%E7%A8%8B%E9%A2%98.pdf)

<!--more-->

##  汽水瓶
### 题目
* 有这样一道智力题：“某商店规定：三个空汽水瓶可以换一瓶汽水。小张手上有十个空汽水瓶，她最多可以换多少瓶汽水喝？”答案是5瓶，方法如下：先用9个空瓶子换3瓶汽水，喝掉3瓶满的，喝完以后4个空瓶子，用3个再换一瓶，喝掉这瓶满的，这时候剩2个空瓶子。然后你让老板先借给你一瓶汽水，喝掉这瓶满的，喝完以后用3个空瓶子换一瓶满的还给老板。如果小张手上有n个空汽水瓶，最多可以换多少瓶汽水喝？

* 输入描述
输入文件最多包含10组测试数据，每个数据占一行，仅包含一个正整数n（1<=n<=100），表示小张手上的空汽水瓶数。n=0表示输入结束，你的程序不应当处理这一行。

* 输出描述
对于每组测试数据，输出一行，表示最多可以喝的汽水瓶数。如果一瓶也喝不到，输出0。

* 输入例子
3
10
81
0

* 输出例子
1
5
40

### 分析
注意，当瓶子数目等于2时候，可以先赊账一瓶。
 
### 求解

```cpp
#include <iostream>
using namespace std;

int calMax(int num){
    int res = 0;
    if(num < 2){
        return 0;
    }
    while(num >= 2){
        if(num == 2){
            res++;
            num = 0;
        }
        else{
            res += (num/3);
            num = (num%3) + (num/3);
        }
    }
    return res;
}

int main(){
    int num;
    int count=0;
    while((cin>>num) && count <10){
        if(num == 0){
            break;
        }
		cout<<calMax(num)<<endl;        
    }
    return 0;
}
```

## 明明的随机数
### 题目
* 明明想在学校中请一些同学一起做一项问卷调查，为了实验的客观性，他先用计算机生成了N个1到1000之间的随机整数（N≤1000），对于其中重复的数字，只保留一个，把其余相同的数去掉，不同的数对应着不同的学生的学号。然后再把这些数从小到大排序，按照排好的顺序去找同学做调查。请你协助明明完成“去重”与“排序”的工作。
 
```cpp
Input Param 
     n               输入随机数的个数     
inputArray      n个随机整数组成的数组 
     
Return Value
     OutputArray    输出处理后的随机整数
```

* 注：测试用例保证输入参数的正确性，答题者无需验证。测试用例不止一组。

* 输入描述
输入多行，先输入随机整数的个数，再输入相应个数的整数

* 输出描述
返回多行，处理后的结果

* 输入例子
11
10
20
40
32
67
40
20
89
300
400
15

* 输出例子
10
15
20
32
40
67
89
300
400

### 分析
使用`find(vec.begin(),vec.end(),ele) == vec.end()`判断。
 
### 求解

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void mySort(vector<int>& arr){
    vector<int> res;
    int size_arr = arr.size();
    for(int i=0;i<size_arr;i++){
        if(find(res.begin(),res.end(),arr[i]) == res.end()){
            //未找到该元素
            res.push_back(arr[i]);
        }
    }
    sort(res.begin(),res.end());
    int size_res = res.size();
    for(int j=0;j<size_res;j++){
        cout<<res[j]<<endl;
    }
}

int main(){
    int length;
    while(cin>>length){
        vector<int> arr(length);
        for(int i=0;i<length;i++){
            cin>>arr[i];
        }
        mySort(arr);
    }
    return 0;
}
```

## 进制转换
### 题目
* 写出一个程序，接受一个十六进制的数值字符串，输出该数值的十进制字符串。（多组同时输入 ）

* 输入描述:
输入一个十六进制的数值字符串。

* 输出描述:
输出该数值的十进制字符串。

* 输入例子:
0xA

* 输出例子:
10

### 分析
* C++中大小写字母转换函数是`tolower(char)`和`toupper(char)`，需要包含`<cctype>`函数。
* `A`的ASCII值是65
* `Z`的ASCII值是90
* `a`的ASCII值是97
* `z`的ASCII值是122
 
### 求解

* 方法1

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int mapValue(char hexDigit){
    int res = -1;
    switch(hexDigit){
        case '0':{
            res = 0;
            break;
        }
        case '1':{
            res = 1;
            break;
        }
        case '2':{
            res = 2;
            break;
        }
        case '3':{
            res = 3;
            break;
        }
        case '4':{
            res = 4;
            break;
        }
        case '5':{
            res = 5;
            break;
        }
        case '6':{
            res = 6;
            break;
        }
        case '7':{
            res = 7;
            break;
        }
        case '8':{
            res = 8;
            break;
        }
        case '9':{
            res = 9;
            break;
        }
        case 'A':
        case 'a': {
            res = 10;
            break;
        }
       	case 'B':
        case 'b': {
            res = 11;
            break;
        }
        case 'C':
        case 'c': {
            res = 12;
            break;
        }
        case 'D':
        case 'd': {
            res = 13;
            break;
        }
        case 'E':
        case 'e': {
            res = 14;
            break;
        }
        case 'F':
        case 'f': {
            res = 15;
            break;
        }     
    }
    return res;
}
int hexToDec(string str){
	int sum = 0;
    int size = str.size();
    for(int i=2;i<size;i++){
        sum = sum*16+mapValue(str[i]);
    }
    return sum;
}

int main(){
    string str;
    while(cin>>str){
        if((str[0] == '0') && (str[1] == 'x' || str[1] == 'X')){
            //输入的数字是16进制数
            cout<<hexToDec(str)<<endl;
        }
    }
    return 0;
}
```

* 方法2

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cctype>

using namespace std;

int mapValue(char hexDigit){
    int res = -1;
    if(hexDigit <= '9'){
        res = (int)(hexDigit - '0');
    }
    else{
    	//转换为小写
        hexDigit = tolower(hexDigit);
        res = 10+ (hexDigit - 'a');
    }
    return res;
}
int hexToDec(string str){
	int sum = 0;
    int size = str.size();
    for(int i=2;i<size;i++){
        sum = sum*16+mapValue(str[i]);
    }
    return sum;
}

int main(){
    string str;
    while(cin>>str){
        if((str[0] == '0') && (str[1] == 'x' || str[1] == 'X')){
            //输入的数字是16进制数
            cout<<hexToDec(str)<<endl;
        }
    }
    return 0;
}
```



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 