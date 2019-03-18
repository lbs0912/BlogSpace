---
title: string和STL容器用法小结
date: 2017-08-23 14:35:48
categories: C++
tags: [map,set,vector,deque,queue]
keywords: map,set,vector,deque,queue
---



* 本文主要对`string`和`vector`顺序容器的用法进行总结,对`map`,`set`这些关联容器的用法进行总结。
* 对标准库类型迭代器(iterator)进行了介绍。它是`string`和`vector`的配套类型，常被用于访问string中的字符或者vector中的元素。
* 对`queue`和`deque`的用法进行了介绍。




<!--more-->

## string
参考资料
* [string 常用操作](http://buptjz.github.io/2013/02/23/string_summary)
* `C++ Primer`

### 定义和初始化
* `string s1`:   默认初始化，s1是一个空串
* `string s2(s1)`:  s2是s1的副本
* `string s2 = s1`: 等价于s2(s1) 
* `string s3("value")`:  s3是字面值""value"的副本，除了字面值最后的那个空字符除外
* `string s3 = "value"`: 等价于s3("value") 
* `string s4(n,"c")`: 把s4初始化为由连续n个字符"c"组成的串

### 常用操作
#### 操作清单
* `cout<<s`:  将s写出到输出流`cout`中，返回`cout` 
* `cin>>s`:   从cin中读取字符串赋值给s，字符串以空白符分隔，返回cin
* `getline(cin,s)`: 从is读取一行赋值给s，返回cin
*  `s.empty()`:  s为空返回true，否则返回false
*  `s.size()`:  返回s的长度，即字符的个数
*  `s.length()`:  同`size()`方法
*  `s[n]`:  返回s中第n个字符的引用，位置n从0计起
*  `s1+s2`:  返回s1和s2连接后的结果
*  `s1 = s2`:  用s2的副本代替s2中原来的字符
*  `s1 == s2`: 判断s1和s2是否相等（大小写敏感）
*  `s1 != s2`: 判断s1和s2是否不相等（大小写敏感）
*  `<, <=, >, >=`: 利用字符在字典中的顺序进行比较，且对字母大小写敏感。
*  `str.begin()`： 迭代器，指向字符串的开头**所在的地址**
*  `str.end()`： 迭代器，指向字符串的**末尾的下一个元素所在的地址**
*  `str.rbegin()| str.rend()`：迭代器，对符串逆序考虑，其余同上
*  `*str.begin()`：返回迭代器指向的字符，即第1个元素，等价于`str[0]`
*  `*(str.end()-1)`：返回迭代器指向的字符，即最后1个元素，等价于`str[str.size()-1]`。注意，最后一个字符，是`*(str.end()-1)`，不是`*(str.end())`。
*  `*str.rbegin() | *(str.rend()-1)`：同上，不再赘述




#### 读写string对象
**在读取string对象时，string对象会自动忽略开头的空白（即空格符，换行符，制表符等）并从第1个真正的字符开始读起，直到遇见下一处空白为止。**

```cpp
string s 
cin>>s;
cout<<s<<endl;
```
 上述程序中，若输入`"  Hello world!  "`（注意开头和结尾处的空白，以及两个单词之间的空格），则会输出"Hello"，而不是"Hello world!"，输出结果中没有任何空格。

string对象的读取操作返回的是运算符左侧的运算对象作为其结果（此处为`cin`和`cout`），因此，多个输入和输出可以连写在一起。

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
while(getline(cin,line)){ //记住用法 getline(cin,str)
	cout<<line<<endl;
}
```
 
### 处理string对象中的字符
**下述列表中的操作需要包含`<cctype>`头文件。**
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
* `tolower(c)`: 将c以小写字母形式输出，参数为char，不是string
* `toupper(c)`: 将c以大写字母形式输出，参数为char，不是string

* 大小写转换示例-1

```cpp
string str="abcdABCD";
//toupper(c)返回值对应的大写字母
//但是，cout直接输出toupper(c)时候
//cout会对其进行类型转换，会默认输出字符的ASCII值
cout<<toupper(str[0])<<endl;  //65

//toupper(c)返回值对应的大写字母
str[0] = toupper(str[0]);
cout<<str[0]<<endl;           // A
```

* `toupper() | tolower()`方法，只能对字符`char`参数进行转换，不能转换字符串`string`参数。如果要对其转换，一般采用遍历转换或者`transform()`方法(需要包含`algorithm`头文件)。


```cpp
string str = "abcdef";

//方法1： 遍历转换
for(int i=0;i<str.size();i++){
    str[i] = toupper(str[i]);
}
cout<<str<<endl;

//方法2： transform方法
//#include <algorithm>  包含头文件
//using namespace std;  标准命名空间

transform(str.begin(),str.end(),str.begin(),toupper);

//transform会对容器1的每个元素应用指定的函数操作，然后存放到容器2中
//transform函数接受4个参数
//参数1 - 容器1的首迭代器
//参数2 - 容器1的尾迭代器
//参数3 - 容器2的首迭代器
//参数4 - 函数操作

```

* JS中大小写转换，采用如下方式。JS中，可以直接对字符串整体进行大小写转换。


```javascript
str.toUpperCase(); 
str.toLowerCase();
```

### 高级操作
#### 反转和排序 reverse() | sort()
*  `sort(str.begin(),str.end())`：对字符串字典排序
*  `reverse(str.begin(),str.end())`：对字符串反转  


> `A`的ASCII值是65，加上`25`，得到`Z`的ASCII值90。
>
> `a`的ASCII值是97，加上`25`，得到`z`的ASCII值122

#### 截取子字符串 substr
* C++中，`substr(startIndex [, length])`函数中，第2个参数可选，用于指定截取的长度。
* JS中，`substr(startIndex [, length])`函数中，第2个参数可选，用于指定截取的长度。这点和C++相同。
* JS中，还提供了`substring(startIndex [, endIndex])`函数，第2个参数可选，用于指定截取`[startIndex,endIndex)`区间的字符串，区间左闭右开。


#### 查找 find()
* `find(c[,index])`: 查找，返回字符`c`出现的第1个位置的索引值，**如果查找不成功，返回`string::npos`**。第2个参数可选，用于指定从索引值为`index`处的地方开始查找。
* `rfind` :	反向查找
* `find_first_of`:	查找包含子串中的任何字符，返回第一个位置
* `find_first_not_of`:	查找不包含子串中的任何字符，返回第一个位置
* `find_last_of`:	查找包含子串中的任何字符，返回最后一个位置
* `find_last_not_of`:	查找不包含子串中的任何字符，返回最后一个位置
* 参考[CPlusPlus.com](http://www.cplusplus.com/reference/string/string/find/)了解更多。


**需要注意的是**
1. 上述所有函数的返回值都是`size_type`类型。因此，在判断是否查找到结果时候，需要将`find()`函数返回值赋值给`string::size_type`类型，而不要赋值给一个`int`类型。`string::npos`在头文件中被定义为了`-1`。


```cpp
string::size_type index = str.find("bcd");
if(index == string::npos){
    cout<< "Can not find."<<endl;
}

```
2. 上述所有函数接受参数的格式都相同，参见上述对`find()`参数的介绍。
3. `find()`函数和`find_first_of()`函数的区别。

```cpp
string str = "abcdbbef";
str.find_first_of("db"); //只要匹配到目标元素中任何一个即可，最先匹配到"b"，返回结果1
str.find("db");  //匹配目标元素整体，即"db"，返回结果3
```


#### 替换 replace()
* 参考[CPlusPlus.com](http://www.cplusplus.com/reference/string/string/replace/)了解更多。

当`replace()`函数参数采用位置方式时，

* `replace(pos,len,str2)`: 从str的第`pos`个字符开始，删除`len`个字符，并替换为`str2`。类似于JS中的`splice()`方法。
* `replace(pos,len,str2,pos2,len2)`: 从str的第`pos`个字符开始，删除`len`个字符，并进行替换。替换时候，从`str2`的第`pos2`个字符开始，用`len2`个字符进行替换。
* `replace(pos,len,str2,len2)`: 从str的第`pos`个字符开始，删除`len`个字符，并进行替换。替换时候，用`str2`的前`len2`个字符进行替换。
* `replace(pos,len,nums，str2)`: 从str的第`pos`个字符开始，删除`len`个字符，并进行替换。替换时候，用`nums`个`str2`进行替换。


```cpp
string base = "this is a test string.";
string str2 = "n example";
string str3 = "sample phrase";
string str4 = "useful.";

// 方式1： 使用位置              
string str = base;           // "this is a test string."
str.replace(9, 5, str2);          // "this is an example string." (1)

str.replace(19, 6, str3, 7, 6);   // "this is an example phrase." (2)

str.replace(8, 10, "just a");     // "this is just a phrase."     (3)

str.replace(8, 6, "a shorty", 7);  // "this is a short phrase."    (4)

str.replace(22, 1, 3, '!');        // "this is a short phrase!!!"  (5)

```

当`replace()`函数参数采用迭代器方式时，

* `replace(it1,it2,str2)`: 将str的`[it1,it2)`区间内的字符删除，并替换为`str2`。区间左闭右开。
* `replace(it1,it2,str2,len)`: 将str的`[it1,it2)`区间内的字符删除，并替换为`str2`的前`len`个字符。区间左闭右开。
* `replace(it1,it2,nums,str2)`: 将str的`[it1,it2)`区间内的字符删除，并用`nums`个`str2`替换。区间左闭右开。
* `replace(it1,it2,it3,it4)`: 将str的`[it1,it2)`区间内的字符删除，并用新字符串的`[it3,it4)`区间内字符替换。区间左闭右开。



```cpp
string base = "this is a test string.";
string str2 = "n example";
string str3 = "sample phrase";
string str4 = "useful.";
string str = "this is a short phrase!!!";

str.replace(str.begin(), str.end() - 3, str3);           
// "sample phrase!!!"      (1)

str.replace(str.begin(), str.begin() + 6, "replace");   
// "replace phrase!!!"     (3)

str.replace(str.begin() + 8, str.begin() + 14, "is coolness", 7); 
// "replace is cool!!!"    (4)

str.replace(str.begin() + 12, str.end() - 4, 4, 'o'); 
// "replace is cooool!!!"  (5)

str.replace(str.begin() + 11, str.end(), str4.begin(), str4.end());
// "replace is useful."  (6)
```

## vector
vector是一种顺序容器，相比于数组，**vector的长度可以动态变化**。 要使用`vector`，必须包含`<vector>`头文件：`#include<vector>`。

### 定义和初始化
* `vector<T> v1`: 创建一个空vector，执行默认初始化
* `vector<T> v2(v1)`: v2中包含v1所有元素的副本
* `vector<T> v2=v1`: 等价于v2(v1)
* `vector<T> v3(n,val)`: v3包含n个重复的元素，每个元素值都是val
* `vector<T> v4(n)`: v3包含n个重复的元素，每个元素执行默认初始化
* `vector<T> v5{a,b,c...}`: 
* `vector<T> v5={a,b,c...}`: 等价于v5{a,b,c...}

若在创建vector对象时候不提供初始值，则会执行默认初始化。初始化值由vector中元素的类型决定。如果元素类型是内置类型，比如int，则元素初始值会被初始化为0。如果元素是某种类类型，如string，则元素由类默认初始化。如果vector对象中的元素不支持默认初始化，则必须提供元素初始值。 

```cpp
vector<int> ivec(10);     //10个元素，每个都初始化为0
vector<string> svec(10);   // 10个元素，每个都是空string对象
```
* 二维vector初始化

```cpp
vector<vector<int>> ivec(rows,vector<int> (cols));
```

* **`vector`容器中添加元素可以使用`push_back`，不能使用下标访问符号直接添加元素，否则会编译错误（越界访问不存在的元素）。`map`容器中，若采用下标访问符号，可以直接添加元素。**

```cpp
vector<int> ivec;
ivec[0] = 1;  //Error

vector<int> ivec(length);
ivec[0] = 1;  //OK

map<int,int> myMap;
myMap[1] = 2;  //OK
```

### 常用操作
#### 操作清单
* `v.empty()`: 判断vector对象是否为空
* `v.clear()`: 清空vector对象，**清空之后，vector的size()值为0，但是capacity()值不受影响。**
* `v.size()`：长度值
* `v.push_back(element)`:  在vector尾部插入元素element
* `v.pop_back()`:  在vector尾部删除元素
* `v.insert(it,[nums],val)`: 在迭代器`it`指定的位置，插入`val`值或者`nums`个`val`值。
* `v.[n]`：访问第`n-1`个元素
* `v.erase(it)`: 删除迭代器it指定位置的元素
* `v.erase(it1,it2)`: 删除迭代器指定的`[it1,it2)`区间内的元素。区间左闭右开。
* `v1 = v2`
* `v = {a,b,c...}`
* `v1==v2`
* `v1 != v2`
* `< <= > >=`: 利用字符在字典中的顺序进行比较，且对字母大小写敏感。


```cpp
vector<int> vec(10,0);
vector<int>::iterator it;
it = vec.begin();

vec.insert(it+3,5,-1); //在迭代器it指定的位置处，插入5个-1

vec.earse(it+2,it+4);
vec.earse(it+1);

```

### 高级操作
####  元素排序 sort()
* `sort(it1,it2)`: 对迭代器指定的`[it1,it2)`区间内的元素进行排序。区间左闭右开。
* 使用`sort()`操作之前，需要先包含头文件：`#include<algorithm>`
* `sort()`操作可以对vector元素进行排序，默认是按照升序排列。对数字按照升序排序，对字符串按照字典排序。这点和JS不同。JS中的`sort()`方法，对数字和字符串都是按照字典排序。


```cpp
//c++
sort(vec.begin(),vec.end());

//JS
["Tom","Green","David"].sort();

```

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

C++中，比较函数的返回值类型是一个**布尔值**。默认情况下，对于元素a和b，判断条件是`a<b`，进行升序排序。重写比较函数，进行降序排序时候，判断条件改为`a>b`即可。

JS中，重写`sort`方法的比较函数时，比较函数的返回值是一个**数值**。对于元素a和b，若返回值小于0，则a排在b之前，进行升序排序；若返回值大于0，则b排在a之前，进行降序排序；若返回值等于0，不改变a和b的位置。

例如，JS中，对于数字型数组排序，升序时候，比较函数返回值为`return a-b;`；降序时候，比较函数返回值为`return b-a;`。



#### 反转操作reverse()
* `reverse(it1,it2)`: 对迭代器指定的`[it1,it2)`区间内的元素进行反转。区间左闭右开。
* 使用reverse将元素翻转之前，需要先包含头文件`include<algorithm>`
* Reverse range:  Reverses the order of the elements in the range `[first,last)`.(Not include last)
* 将vector中的元素反转，反转结果仍旧保存在旧有的vector中。例如，将3,2,1反转为1,2,3。即该操作会改变传入的vector参数。
```cpp 
reverse(vec.begin(),vec.end());
```


#### 查找 find()
* `find(it1,it2,val)`: 在区间`[it1,it2)`内查找`val`。返回值是一个**迭代器*。如果查找不成功，则返回`it2`，如果查找成功，则返回`val`对应的迭代器。


```cpp
vector<int> vec(10, 100);
if (find(vec.begin(), vec.begin()+3, 2) == vec.begin()+3) {
    //查找失败，返回vec.begin()+3迭代器
	cout << *find(vec.begin(), vec.begin() + 3, 2) << endl; //100
}
```

## 迭代器(iterator)
可以使用下标运算符`[]`访问string对象和vector对象的元素，还可以采用一种更通用的机制实现该操作，这就是迭代器(iterator)。**所有的标准库容器都可以使用迭代器，但是其中只有少数几种才同时支持下标运算。**每种容器都定义了自己的迭代器类型。

```cpp
vector<T>::iterator iter; 
```

严格来说，string对象不属于容器类型，但是string支持很多与容器类型类似的操作。vector和string均同时支持下标运算符和迭代器。

> **使用迭代器，需要包含头文件`<iterator>`。**
 
### begin() & end()
* 每种容器都定义了`begin()` 和 `end()` 成员函数，用于返回迭代器。
* `begin()`成员函数负责返回指向第1个元素（或第1个字符）的迭代器。
* `end()` 成员函数负责返回指向容器（或string对象）**尾元素的下一位置(one past the end)**的迭代器。也就是说，该迭代器指示的是容器的**一个本不存在的“尾后(off the end)”**。这样的迭代器不具有实际意义，仅是一个标记而已，表示已经处理完了容器中的所有元素。
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
* 如果对象只需读操作而无需写操作的话，最好使用常量类型（比如`const_iterator`）。为了便于专门得到`const_iterator`类型的返回值，C++11新标准中引入了两个新函数，分别是`cbegin()`和`cend()`。（无论容器本身是否是常量，都返回`const_iterator`）

```cpp
vector<int> v;
auto it1 = v.begin();   // it1的类型是vector<int>::iterators
auto it3 = v.cbegin();  // it2的类型是vector<int>::const_iterator
```


### rbegin() & rend()
![C++-iterator-rbegin.PNG](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-rbegin.PNG)
* `rend()`函数返回一个逆向迭代器，指向字符串的开头（**第一个字符的前一个位置**）
* `rbegin()`函数返回一个逆向迭代器，指向字符串的最后一个字符的位置。

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

* `vec.front()`：等价于`vec[0]`或者`*vec.begin()`
* `vec.back()`：等价于`vec[vec.length()-1]`或者`*(vec.end()-1)`


![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-2.jpg)

* 上述操作中，`p`是迭代器，不是索引值（`int`类型）


![C++-iterator-operator](http://ol3kbaay9.bkt.clouddn.com/C++-iterator-operator-3.jpg)

* `size()`是容器中元素实际的个数，和`resize()`操作对应。`resize()`会改变`size()`的值。使用`clear()`操作，会将`size()`值清空为0，但是`capacity()`值不受影响。
* `capacity()`是指预分配的内存空间，和`reserve()`操作对应。C++中只有`string`和`vector`有这个属性。


```cpp
vector<int> v;

cout << "v.size() == " << v.size() << " v.capacity() = " << v.capacity() << endl;
//size = 0  capacity = 0

v.reserve(10);
cout << "v.size() == " << v.size() << " v.capacity() = " << v.capacity() << endl;
//size = 0  capacity = 10

v.resize(10);
cout << "v.size() == " << v.size() << " v.capacity() = " << v.capacity() << endl;
//size = 10  capacity = 10

v.push_back(0);
cout << "v.size() == " << v.size() << " v.capacity() = " << v.capacity() << endl;
//size = 11  capacity = 15
//size=capacity之后，再进行push操作，会触发重新分配内存空间的操作。
//至于具体分配多少，不同的库有着不同的实现
//此处是增加原来内存空间的一半

v.clear();
cout << "v.size() == " << v.size() << " v.capacity() = " << v.capacity() << endl;
//size = 0  capacity = 15
```

* **需要注意的是，当size小于capacity时，使用`vec[index]`访问大于size的索引值处的元素，是会编译产生错误的，提示越界访问。因为`capacity()`只为容器分配了内存空间，`size()`才表明真正可用`[]`访问的界限。**

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


## map
### 关联容器 | 高效查找和访问
* 关联容器和顺序容器（`string`，`vector`）不同。关联容器中的元素都是按照关键字来保存和访问的。与之相对，顺序容器中的元素是按照它们在容器中的位置来顺序保存和访问的。

* **关联容器支持高效的关键字查找和访问。*** `map`和`set`是两个最常见的关联容器。`map`中的元素时一些关键字-值(`key-value`)对。

* 标准库提供了8个关联容器
    1. `map`: 关联数组，保存`key-value`对，关键字不允许重复,关键字**自动**升序排序
    2. `set`: 只保存关键字key，关键字不允许重复，关键字**自动**升序排序
    3. `multimap`: 关键字可重复的`map`，关键字**自动**升序排序
    4. `multiset`: 关键字可重复的`set`，关键字**自动**升序排序
    5. `unordered_map`: 用哈希组织的`map`，关键字无序
    6. `unordered_set`: 用哈希组织的`set`，关键字无序
    7. `unordered_multimap`: 用哈希组织的`multimap`，关键字无序,关键字允许重复
    8. `unordered_multiset`: 用哈希组织的`multiset`，关键字无序，关键字允许重复

* 上述8个关联容器中，带有`multi`的表明关键字允许重复，带有`unordered`的表明关键字无序。
* 使用`map`和`multimap`，需要包含`map`头文件。使用`set`和`multiset`，需要包含`set`头文件。无序容器，需要包含`unordered_map`和`unordered_set`头文件。


### 定义和初始化
* `#include<map> | #include <unordered_map>`：包含头文件
* `map<string,int> datas`: 创建key-value形式的map容器

* **`vector`容器中添加元素可以使用`push_back`，不能使用下标访问符号直接添加元素，否则会编译错误（越界访问不存在的元素）。`map`容器中，若采用下标访问符号，可以直接添加元素。**

```cpp
vector<int> ivec;
ivec[0] = 1;  //Error

vector<int> ivec(length);
ivec[0] = 1;  //OK

map<int,int> myMap;
myMap[1] = 2;  //OK
```
* `map`会对`key-value`键值对**自动**按照`key`进行升序排列。


### 常用操作
#### 操作清单
* `insert(make_pair(key,value))`: 插入元素
* `mapName[key] = value`: 插入元素
* `size()`: 返回元素个数
* `mapName.find(key)`: 查找关键字key。返回值是一个迭代器。若查找成功，则返回对应的迭代器。若查找失败，则返回`mapName.end()`。
* `it->first`: 输出`it`迭代器的`key`值
* `it->second`: 输出`it`迭代器的`value`值
* `(*it).first`: 输出`it`迭代器的`key`值
* `(*it).second`: 输出`it`迭代器的`value`值
* `mapName.count(key)`: 返回包含key关键字的关键值对（不是key对应的value值）。因为`map`中`key`都是唯一的，因此返回值一定是1或者0。因此，利用此方法，可以判断map中是否有待查找的元素，但是不会返回元素的位置(`find()`会返回元素的**迭代器位置**）。

```cpp

map<int, int> datas;
datas.insert(make_pair(10,1));
datas.insert(make_pair(4, 2));
dats.size();   // 2
map<int, int>::iterator it;
it = datas.find(100000);
if (it == datas.end()) {
    //查找失败
	cout << "no match" << endl;
}
else {
    //查找成功
    //输出key-value关键值对的2种方法
	cout << it->first << " " << it->second << endl;
	cout << (*it).first << " " << (*it).second << endl;
}
if(datas.count(10) == 1){
    cout<<"查找成功"<<endl;
}
```

* `mapName.erase(key);`: 删除key关键字对应的元素
* `erase(it);`: 删除迭代器it对应的元素
* `erase(it1,it2);`: 删除迭代器指定的区间`[it1,it2)`内的元素

```
datas.erase(4);
erase(datas.begin());
erase(datas.begin()+3,datas.end()-1);
datas.clear();  //等效于 erase(datas.begin(),datas.end())
```

* `mapName.clear()`: 清空所有元素，清空后，`size()`值为0。等效于`erase(datas.begin(),datas.end())`
* `mapName.empty()`: 判断容器是否为空，返回true或者false。


### map元素排序
参考资料
* [STL容器（三）——对map排序 | CSDN](http://blog.csdn.net/puqutogether/article/details/41889579)
* [map排序 | CSDN](http://blog.csdn.net/iicy266/article/details/11906189)

#### 按照key排序
##### 升序

* `map`会对`key-value`键值对**自动**按照`key`进行升序排列。

* 默认情况下，初始化map时候，实际上是调用的`map<template1,template2,less<template1>>`。第3个参数`less<template1>`表明按照升序排列，`template1`是`key`的类型。第3个参数可以缺省，默认为`less<template1>`

##### 降序

如果要降序排列，则只需将第3个参数改为`greater<template1>`即可。使用`greater`方法，需要先包含`#include <xfunctional>`头文件。


```cpp
#include <xfunctional>
#include <map>

//greater<string>，其参数就是key的类型  
map<string, int, greater<string> > scoreMap;  
scoreMap["LiMin"] = 90;   
scoreMap["ZZihsf"] = 95;   
scoreMap["Kim"] = 100; 
```

类似的函数，还有
* `equal_to`: 相等
* `not_equal_to`: 不相等
* `less`: 小于
* `greater`: 大于
* `less_equal`: 小于等于
* `greater_equal`:  大于等于


##### 自定义排序
上述`less`和`greater`都是**函数对象**，重载了`()操作符。如果要自定义排序，只要声明对应的比较函数对象即可。

```cpp
#include <xfunctional>
#include <map>

struct cmp  //自定义比较规则  
{  
    bool operator() (const string& str1, const string& str2)  
    {  
        return str1.length() < str2.length();   
        //按照key值长度从小到大排列
    }  
};  

//greater<string>，其参数就是key的类型  
map<string, int, cmp> scoreMap;  
scoreMap["LiMin"] = 90;   
scoreMap["ZZihsf"] = 95;   
scoreMap["Kim"] = 100; 
```


#### 按照value排序

在`map`使用中，更多的是按照value进行排列。

思路是将`map`中的元素存放到一个`vector`中。利用`vector`的`sort`方法进行排序。在`sort`方法中，可以自定义比较函数。插入时候，由于`map`的元素是`pair`类型，因此，`vector`的声明为`vector<pair<template1,template2>> vec`。`template1`和`template2`分别是`key`和`value`的类型。



```cpp


typedef pair<string, int> PAIR;
//或者
//#define PAIR  pair<string, int>
//#define语句后面不能加分号

struct cmp  //自定义比较规则  
{
	bool operator() (const PAIR& P1, const PAIR& P2)  
	{
		//注意是PAIR类型，需要.firt和.second。这个和map类似  
		return P1.second < P2.second;
	}
};


map<string, int> scoreMap;      
map<string, int>::iterator iter;

scoreMap["LiMin"] = 90;
scoreMap["ZZihsf"] = 95;
scoreMap["Kim"] = 100;
scoreMap.insert(make_pair("Jack", 88));

vector<PAIR>scoreVector;
//或者
//vector<pair<string,int>> scoreVector;
	
for (iter = scoreMap.begin(); iter != scoreMap.end(); iter++) {
	//转化为PAIR的vector 
	scoreVector.push_back(*iter);
}
	 
sort(scoreVector.begin(), scoreVector.end(), cmp());  //需要指定cmp  

for (int i = 0; i < scoreVector.size(); i++) {            
    //也要按照vector的形式输出  
	cout << scoreVector[i].first << ' ' << scoreVector[i].second << endl;
}
```

> `#define 别名  XXX`，不能以分号结尾（起别名）
>
> `typedef XXX  别名`，以分号结尾（宏定义，预处理）

## set
`set`用法大体同`map`，此处进行简单介绍。

### 常用操作
* `set<template1> setName`
* `size()`
* `empty`: 返回true或者false
* `clear()`: 清空
* `insert(key)`
* `find(key)`：返回值是一个迭代器。如果不存在，返回`setName.end()`
* `count(key)`: 返回值是0或者1
* `earse(key)`
* `earse(it)`
* `earse(it1,it2)`
* `lower_bound(key)`: 返回第1个不是小于（大于或者等于）key的迭代器。`Returns an iterator pointing to the first element in the container which is not considered to go before val (i.e., either it is equivalent or goes after).`

* `upper_bound(key)`: 返回第1个大于key的迭代器。`Returns an iterator pointing to the first element in the container which is considered to go after val.`


```cpp
//示例1
set<int> s;
s.insert(1);
s.insert(3);
s.insert(4);
//s = {1,3,4};
cout << *s.lower_bound(2) << endl;  //3
cout << *s.lower_bound(3) << endl;  //3
cout << *s.upper_bound(3) << endl;  //4


//示例2
set<int> myset;
set<int>::iterator itlow,itup;

for (int i=1; i<10; i++){
    myset.insert(i*10);
} 
// 10 20 30 40 50 60 70 80 90

//第1个大于等于30的元素，指向第3个元素，即30
itlow=myset.lower_bound (30); 
//第1个大于60的元素，指向第7个元素，即70
itup=myset.upper_bound (60);            
myset.erase(itlow,itup); // 10 20 70 80 90

```

> 若`upper_bound(key)`等于`lower_bound(key)`，说明`key`未出现。查找不成功时，均返回`setName.end()`。
>
> 若`upper_bound(key)`和`lower_bound(key)`指向的位置仅差1，说明`key`只出现1次。
>
> 若`upper_bound(key)`和`lower_bound(key)`指向的位置差大于1，说明`key`出现多次。

## pair
* **使用`pair`，需要包含`utility`头文件**
* 向map中插入元素时，使用`make_pair(key,val)`的方式。
* map中元素的类型是`pair<template1,template2>`
* `pairName.first`: 访问对象，用成员运算符
* `pairName.second`：访问对象，用成员运算符
* `it->first | it->second`: 访问指针指向的内容，用`->`访问。
* `(*it).first | (*it).second`: 将指针转换为对象，用成员运算符。最终效果同上。


```cpp
map<int, int> datas;
datas.insert(make_pair(9, 2));
datas.insert(make_pair(7, 8));
datas.insert(make_pair(4, 3));
for (auto it = datas.begin(); it != datas.end(); it++) {
	//此时it是指针，采用->访问
	cout << it->first << " " << it->second << endl;
	//或者
	//此时(*it)是一个pair对象，使用成员运算符访问
	cout << (*it).first << " " << (*it).second << endl;
}
vector<pair<int, int>> vec; //插入的元素时pair对象
for (auto it = datas.begin(); it != datas.end(); it++) {
	vec.push_back(*it);
}
for (int i = 0; i < vec.size();i++) {
	//vec[i]是pair对象，使用成员运算符访问
	cout << vec[i].first << " " << vec[i].second << endl;
}
```

## queue
* 包含`<queue>`头文件
* `queue<template> queName`
* `queue<template> queName(len,val)`
* `queue<template> queName = {1,2,3,4}`
* `push(ele)`: **不是`push_back`**
* `pop()`: **注意，函数没有返回值，不会返回被弹出的元素**
* `size()`
* `q.front()`: 返回队首元素
* `q.back()`: 返回队末元素
* `q.empty()`
* `q.clear()`
* `q.erase(it) | q.erase(it1,it2)`
* `inser(it,val)`
* `inser(it,nums,val)`
* `at(index)`
* `begin() | end() | rbegin() | rend()`
* `swap(q1,q2)`: **交换q1和q2，交换的是两个队列，不是队列中的元素**
* `resize()`


## deque
* 包含`<deque>`头文件
* `deque<template> dequeName`
* `deque<template> dequeName = {1,2,3,4}`
* `deque<template> dequeName(len,val)`
* `push_back(ele) | push_front(ele)`: **不是`push_back`**
* `pop_back() | pop_front()`: **注意，函数没有返回值，不会返回被弹出的元素**
* `size()`
* `q.front()`: 返回队首元素
* `q.back()`: 返回队末元素
* `q.empty()`
* `q.clear()`
* `q.erase(it) | q.erase(it1,it2)`
* `inser(it,val)`
* `inser(it,nums,val)`
* `at(index)`
* `begin() | end() | rbegin() | rend()`
* `swap(q1,q2)`: **交换q1和q2，交换的是两个队列，不是队列中的元素**
* `resize()`



## 参考资料
[1] [ C/C++迭代器使用详解](http://blog.csdn.net/zhanh1218/article/details/33340959)

[2] [C++ STL 迭代器Iterator](http://blog.csdn.net/jnu_simba/article/details/9454787)

[3] [C++中string类下的begin，end，rbegin，rend的用法](http://blog.csdn.net/z2014jw/article/details/50810569)
 

## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
 