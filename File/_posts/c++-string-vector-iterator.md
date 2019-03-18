---
title: C++中string, vector和迭代器iterator用法总结
date: 2017-02-24 11:51:00
tags: [C++,vector,iterator,string]
categories: C++
---



* 本文主要对`string`和`vector`这2种标准库类型的用法进行总结。`string`表示可变长的字符序列，`vector`存放的是某种给定类型对象的可变长序列。
* 还有一种标准库类型是**迭代器(iterator)**，它是`string`和`vector`的配套类型，常被用于访问string中的字符或者vector中的元素。



## 标准库类型string
### 定义和初始化string对象
* `string s1`:   默认初始化，s1是一个空串
* `string s2(s1)`:  s2是s1的副本
* `string s2 = s1`: 等价于s2(s1) 
* `string s3("value")`:  s3是字面值""value"的副本，除了字面值最后的那个空字符除外
* `string s3 = "value"`: 等价于s3("value") 
* `string s4(n,"c")`: 把s4初始化为由连续n个字符"c"组成的串

### string对象上的操作
* `os<<s`:  将s写出到输出流os中，返回os 
* `is>>s`:   从is中读取字符串赋值给s，字符串以空白符分隔，返回is
* `getline(is,s)`: 从is读取一行赋值给s，返回is
*  `s.empty()`:  s为空返回true，否则返回false
*  `s.size()`:  返回s中字符的个数
*  `s[n]`:  返回s中第n个字符的引用，位置n从0计起
*  `s1+s2`:  返回s1和s2连接后的结果
*  `s1 = s2`:  用s2的副本代替s2中原来的字符
*  `s1 == s2`: 判断s1和s2是否相等（大小写敏感）
*  `s1 != s2`: 判断s1和s2是否不相等（大小写敏感）
*  `<, <=, >, >=`: 利用字符在字典中的顺序进行比较，且对字母大小写敏感。

#### 读写string对象
在读取string对象时，string对象会自动忽略开头的空白（即空格符，换行符，制表符等）并从第1个真正的字符开始读起，直到遇见下一处空白为止。

```cpp
string s 
cin>>s;
cout<<s<<endl;
```

 上述程序中，若输入"    Hello world!    "（注意开头和结尾处的空白，以及两个单词之间的空格），则会输出"Hello"，而不是"Hello world!"，输出结果中没有任何空格。

string对象的读取操作返回的是运算符左侧的运算对象作为其结果（此处为cin和cout），因此，多个输入和输出可以连写在一起。

```cpp
string s1,s2; 
cin>>s1>>s2;
cout<<s1<<s2<<endl;
```

#### 读取未知量的string对象

```cpp
string word;
while(cin>>word){
	cout<<word<<endl;
}
```

#### 使用getline()读取一行
有时若希望在最终的字符串中保留输入时的空白符，这时应该用`getline()`函数代替原来的`>>`运算符。

```cpp
string line;
while(getline(cin,line)){
	cout<<line<<endl;
}
```
 
### 处理string对象中的字符
下述列表中的操作需要包含`<cctype>`头文件。
* `isalnum(c)`: 当c是字母或者数字时为真
* `isalpha(c)`: 当c是字母时为真
* `iscntrl(c)`: 当c是控制字符时为真
* `isdigit(c)`: 当c是数字时为真
* `isgraph(c)`: 当c不是空格但是可以打印时为真
* `islower(c)`: 当c是小写字母时为真
* `isprint(c)`: 当c是可打印字符时为真
* `ispunct(c)`: 当c是标点符号时为真
* `isspace(c)`: 当c是空白时为真
* `isupper(c)`: 当c是大写字母时为真
* `isxdigit(c)`: 当c是十六进制数时为真
* `tolower(c)`: 将c以小写字母形式输出
* `toupper(c)`: 将c以大写字母形式输出
 
 

## 标准库类型vector
vector是一种顺序容器，相比于数组，**vector的长度可以动态变化**。 要使用`vector`，必须包含`<vector>`头文件：`#include<vector>`。

### 定义和初始化vector对象
* `vector<T> v1`: 创建一个空vector，执行默认初始化
* `vector<T> v2(v1)`: v2中包含v1所有元素的副本
* `vector<T> v2=v1`: 等价于v2(v1)
* `vector<T> v3(n,val)`: v3包含n个重复的元素，每个元素值都是val
* `vector<T> v4(n)`: v3包含n个重复的元素，每个元素执行默认初始化
* `vector<T> v5{a,b,c...}`: 
* `vector<T> v5={a,b,c...}`: 等价于v5{a,b,c...}

若在创建vector对象时候不提供初始值，则会执行默认初始化。初始化值由vector中元素的类型决定。如果元素类型是内置类型，比如int，则元素初始值会被初始化为0。如果元素是某种类类型，如string，则元素有类默认初始化。如果vector对象中的元素不支持默认初始化，则必须提供元素初始值。 

```cpp
vector<int> ivec(10);     //10个元素，每个都初始化为0
vector<string> svec(10);   // 10个元素，每个都是空string对象
```

### vector操作
* `v.empty()`: 判断vector对象是否为空
* `v.clear()`: 清空vector对象
* `v.size()`
* `v.push_back(element)`:  在vector尾部插入元素element
* `v.pop_back()`:  在vector尾部删除元素
* `v.insert(index,element)`: 在指定的index索引值处插入元素element
* `v.[n]`
* `v.erase(index)`: 删除指定index索引值处的元素
* `v.erase(index1，index2)`: 删除下标在[index1,index2]区间内的元素
* `v1 = v2`
* `v = {a,b,c...}`
* `v1==v2`
* `v1 != v2`
* `< <= > >=`

### 与vector相关的函数
####  元素排序 sort()
* 使用`sort()`操作之前，需要先包含头文件：`#include<algorithm>`
* sort()操作可以对vector元素进行排序，默认是按照升序排列。
`sort(vec.begin(),vec.end());`

> Random-access iterators to the initial and final positions of the sequence to be sorted. The range used is [first,last), which contains all the elements between first and last, including the element pointed by first but not the element pointed by last.  ---[Cplusplus.com](http://www.cplusplus.com/reference/algorithm/sort/?kw=sort)


* 允许通过重写排序比较函数按照降序排列，如下所示。

```
#include <algorithm>
bool Comp(const int &a,const int &b)
{
    return a>b;
}
sort(vec.begin(),vec.end(),Comp)   //按照降序排列
```

#### 反转操作reverse()
* 使用reverse将元素翻转之前，需要先包含头文件`include<algorithm>`
* Reverse range:  Reverses the order of the elements in the range [first,last).(Not include last)
* 将vector中的元素反转，反转结果仍旧保存在旧有的vector中。例如，将3,2,1反转为1,2,3。即该操作会改变传入的vector参数。

```cpp 
reverse(vec.begin(),vec.end());
```

## 迭代器(iterator)
可以使用下标运算符`[]`访问string对象和vector对象的元素，还可以采用一种更通用的机制实现该操作，这就是迭代器(iterator)。所有的标准库容器都可以使用迭代器，但是其中只有少数几种才同时支持下标运算。每种容器都定义了自己的迭代器类型。

```cpp
vector<T>::iterator iter; 
```

严格来说，string对象不属于容器类型，但是string支持很多与容器类型类似的操作。vector和string均同时支持下标运算符和迭代器。

> 使用迭代器，需要包含头文件`<iterator>`。
 
### begin() & end()
* 每种容器都定义了`begin()` 和 `end()` 成员函数，用于返回迭代器。
* `begin()`成员函数负责返回指向第1个元素（或第1个字符）的迭代器。
* `end()` 成员函数负责返回指向容器（或string对象）“尾元素的下一位置(one past the end)”的迭代器。也就是说，该迭代器指示的是容器的一个本不存在的“尾后(off the end)”。这样的迭代器不具有实际意义，仅是一个标记而已，表示已经处理完了容器中的所有元素。
* `end`成员返回的迭代器常被称作**尾后迭代器(off-the-end iterator)**或者简称为尾迭代器(end iterator)。特殊情况下，若容器为空，则begin和end返回的是同一个迭代器，都是尾后迭代器。
* 如果不清楚（不在意）迭代器准确的类型到底是什么，可以使用`auto`关键字，将有编译器决定其类型。

```cpp
// 由编译器决定b和e的类型
// b表示v的第1个元素，e表示v尾元素的下一位置
auto b = v.begin(), e = v.end();   //b和e的类型相同
```

![C++-iterator.PNG](http://ol3kbaay9.bkt.clouddn.com/C++-iterator.PNG)

* `begin`和`end`返回的具体类型由对象是否是常量决定。如果对象是常量，begin和end返回`const_iterator`；如果对象不是常量，返回`iterator`。对于string而言，其返回的类型是`string::iterator`。

```cpp
vector<int> v;
const vector<int> cv;
auto it1 = v.begin();   // it1的类型是vector<int>::iterator
auto it2 = cv.begin();  // it2的类型是vector<int>::const_iterator
```

```cpp
vector<int> ivec(10, 3);
	vector<int>::iterator begin = ivec.begin();
	vector<int>::iterator end = ivec.end();
	//或者使用auto
	//auto begin = ivec.begin();
	//auto = ivec.end();
	//auto k = begin;
	for (vector<int>::iterator k = begin; k != end; ++k) {
		cout << *k << endl;
	}
```

* 如果对象只需读操作而无需写操作的话，最好使用常量类型（比如const_iterator）。为了便于专门得到`const_iterator`类型的返回值，C++11新标准中引入了两个新函数，分别是`cbegin()`和`cend()`。（无论容器本身是否是常量，都返回`const_iterator`）

```cpp
vector<int> v;
auto it1 = v.begin();   // it1的类型是vector<int>::iterators
auto it3 = v.cbegin();  // it2的类型是vector<int>::const_iterator
```


### rbegin() & rend()

* `rend()`函数返回一个逆向迭代器，指向字符串的开头（第一个字符的前一个位置）
* `rbegin()`函数返回一个逆向迭代器，指向字符串的最后一个字符。

![C++-iterator-rbegin.PNG](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-rbegin.PNG)

```cpp
#include<iostream>
#include<string>
using namespace std;
int main()
{
    string str1,str2;
    cin >> str1;
    //定义一个正向迭代器
    string::iterator ptr1 = str1.begin();
    //正向输出字符串
    while (ptr1 != str1.end())
        cout << *(ptr1++) << " ";
    cout << endl;

    cin >> str2;
    //定义一个逆向迭代器
    string::reverse_iterator ptr2 = str2.rbegin();
    //逆向输出字符串
    while (ptr2 != str2.rend())
    //注意逆向迭代器移动方向相反，所以从尾部仍然通过++来移动
        cout << *(ptr2++) << " ";
    cout << endl;
}
```

### 迭代器运算符
* `*iter`: 返回迭代器iter所指元素的引用
* `iter->mem`: 解引用iter并获取该元素的名为mem的成员，等价于(*iter).mem
* `++iter`: 令iter指示容器中的下一个元素 
* `--iter`: 令iter指示容器中的上一个元素 
* `iter1 == iter2`: 判断两个迭代器是否相等
* `iter1 != iter2`: 判断两个迭代器是否不相等

### 迭代器中常用的操作
![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-1.jpg)
![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-2.jpg)
![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-3.jpg)
![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-4.jpg)
![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-5.jpg)






### 数组中使用begin(arr)和end(arr)
* 在C++11中，为数组引入了`begin(arr)`和`end(arr)`函数。这两个函数与容器中的两个同名函数功能类似。但是由于数组不是类类型，因此这两个函数不是成员函数。需要将数组作为它们的参数传入。

```cpp
int ia[] = {0,1,2,3,4,5};
int *beg = begin(ia);   //指向ia首元素的指针
int *last = end(ia);    //指向ia尾元素的下一位置的指针
```
* 这两个函数定义在`<iterator>`头文件中。
 
```cpp
#include<iostream>
#include<iterator>
using namespace std;
int main()
{
    int ia[]={0,1,2,3,4,5,6,7};
    auto b = begin(ia);  // C++11才允许对数组使用迭代器
    auto e= end(ia);
    for(auto k=b ; k!=e; ++k)
        cout << *k << endl;
     return 0;
}
```

## 参考资料
[1] [ C/C++迭代器使用详解](http://blog.csdn.net/zhanh1218/article/details/33340959)
[2] [C++ STL 迭代器Iterator](http://blog.csdn.net/jnu_simba/article/details/9454787)
[3] [C++中string类下的begin，end，rbegin，rend的用法](http://blog.csdn.net/z2014jw/article/details/50810569)
 
## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
 