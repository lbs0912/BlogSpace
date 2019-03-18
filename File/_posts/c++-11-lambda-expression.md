---
title: C++11中的Lambda表达式（匿名函数）
date: 2017-03-06 18:35:26
categories: C++
tags: [C++,Programing,Lambda]
keywords: Lambda,C++
toc: true
---





* 本文主要对C++中引入的Lambda表达式（匿名函数）进行总结和记录。
* [匿名函数 - 维基百科](https://zh.wikipedia.org/wiki/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0#C.2B.2B)


<!--more-->

## Lambda表达式（匿名函数）
C++11标准提供了匿名函数的支持，在《ISO/IEC 14882:2011》（C++11标准文档）中叫做**lambda表达式**。

### Syntax

```cpp
[capture] (parameters) mutable exception attribute -> return_type { body }
```
必须用方括号括起来的`capture`列表来开始一个`lambda`表达式的定义。

lambda函数的形参表比普通函数的形参表多了3条限制：
* 参数不能有缺省值
* 不能有可变长参数列表
* 不能有无名参数

如果lambda函数没有形参且没有`mutable`, `exception`或`attribute`声明，那么参数的空圆括号可以省略。但如果需要给出`mutable`, `exception`或`attribute`声明，那么参数即使为空，圆括号也不能省略。

如果函数体只有一个`return`语句，或者返回值类型为`void`，那么返回值类型声明可以被省略

```cpp
[capture](parameters){body}
```
### 实例

```cpp
[](int x, int y) { return x + y; } // 从return语句中隐式获得返回值类型
[](int& x) { ++x; }   // 没有返回值类型，返回值类型是void
[]() { ++global_x; }  // 无参数
[]{ ++global_x; }     // 无参数时，()可以被省略
[](int x, int y) -> int { int z = x + y; return z; } //显示指定返回值类型
```


## 函数嵌套
* C++中允许嵌套调用函数，嵌套声明函数，但是不允许嵌套定义函数。

```cpp
void fun3() {
	cout << "Func 3" << endl;
}
void fun1() {
	cout << "Func 1" << endl;
	void func2();  //嵌套声明函数
	fun3();       //嵌套调用函数
	func2();
}
void func2() {
	cout << "Func 2" << endl;
}
int main() {
	fun1();
	return 0;
}
```
上述程序中进行了嵌套调用函数和嵌套声明函数，都是正确地。但是下面程序中的嵌套定义函数是错误地，编译时会产生错误。

```cpp
void fun1() {
	cout << "Func 1" << endl;
	//嵌套定义函数，编译产生错误
	// error C2601: “func2”: 本地函数定义是非法的
	void func2() {  
		cout << "Func 2" << endl;
	};
}
int main() {
	fun1();
	system("pause");
	return 0;
}
```

* C++中，Lambda表达式（匿名函数）可以在函数内部定义。

```cpp
void fun1(){
	cout <<"Func 1"<< endl;
	int x = 2;
	auto myDouble = [](int x) {return x*x; };
	int temp = myDouble(2);
	cout <<temp << endl;  //4
}
```


## sort()函数中自定义比较函数
`sort()`函数中可以自定义比较函数。

```cpp
template <class RandomAccessIterator, class Compare>
  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);

comp

This can either be a function pointer or a function object.

Binary function that accepts two elements in the range as arguments, and returns a value convertible to bool. The value returned indicates whether the element passed as first argument is considered to go before the second in the specific strict weak ordering it defines.

The function shall not modify any of its arguments.
```
### 自定义比较函数不允许是类的成员函数
> **自定义的比较函数不能是类的成员函数。** 其原因是，成员函数作为类的成员， 需要在某个具体的类的实例中才能找到对应的代码段或者在内存中的位置，这样才能被`sort()`使用用来比较。——[C++ 定义实用比较函数注意点](http://www.sigmainfy.com/blog/c-compare-functions-notes.html)

使用`sort()`方法需要先包含`<algorithm>`头文件。
 

对于如下测试程序，如果将比较函数设为Test类的成员函数，即`sort(data.begin(), data.end(), comp_in_class)`;，编译时会产生语法错误。
 
```cpp
class comp {
public:
	bool operator() (const int &l, const int &r) const {
		return l > r;
	}
};

bool comp_fun(const int &a, const int &b) {
	return a > b;
}

class Test {
public:
	bool comp_in_class(const int &a, const int &b) {
		return a > b;
	}

	void test(vector<int> &data) {
		//sort(data.begin(), data.end(), comp_in_class); //<strong>compile error!</strong>
		sort(data.begin(), data.end(), comp_fun);
	}
};
int main(void) {
	vector<int> data = {1,-1,12,15,-8};
	sort(data.begin(), data.end());
	cout << "defualt compare: ";
	for (vector<int>::size_type i = 0; i != data.size(); ++i)
		cout << data[i] << ' ';
	cout << endl;

	Test t;
	t.test(data);
	cout << "class method call: ";
	for (vector<int>::size_type i = 0; i != data.size(); ++i)
		cout << data[i] << ' ';
	cout << endl;
	
	sort(data.begin(), data.end(), comp());
	cout << "function object: ";
	for (vector<int>::size_type i = 0; i != data.size(); ++i)
		cout << data[i] << ' ';
	cout << endl;

	sort(data.begin(), data.end(), comp_fun);
	cout << "just function (pointer): ";
	for (vector<int>::size_type i = 0; i != data.size(); ++i)
		cout << data[i] << ' ';
	cout << endl;
	system("pause");
	return 0;
}
```

### 利用匿名函数创建自定义比较函数
如果将上述程序修改，利用Lambda表达式（匿名函数）来自定义比较函数，编译不会产生错误。

```cpp

class Test {
public:
	/*
	bool comp_in_class(const int &a, const int &b) {
		return a > b;
	}
	*/
	void test(vector<int> &data) {
		auto comp_in_class = [](const int &a, const int &b) {return a > b; };
		sort(data.begin(), data.end(), comp_in_class); //no error
		sort(data.begin(), data.end(), comp_fun);
	}
};
```
 需要注意的是，C++中不允许嵌套定义函数。因此上面程序中，必须使用匿名函数，不能使用普通函数。
 
> `auto`关键字可以用于保存Lambda表达式类型的变量声明。 —— [auto关键字 - 维基百科](https://zh.wikipedia.org/wiki/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0#C.2B.2B)

```cpp
auto ptr = [](double x){return x*x;};//类型为std::function<double(double)>函数对象
```
###  笔试题
*  [LeetCode - 435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/?tab=Description)

## 参考资料
* [C++ 定义实用比较函数(Custom Compare Function) 注意点](http://www.sigmainfy.com/blog/c-compare-functions-notes.html)
* [匿名函数 - 维基百科](https://zh.wikipedia.org/wiki/%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0#C.2B.2B)
* [C++11中的Lambda表达式](https://yq.aliyun.com/articles/32226)
* [C++11中的匿名函数](http://www.cnblogs.com/pzhfei/archive/2013/01/14/lambda_expression.html)
* [C++11新特性中的匿名函数Lambda表达式的汇编实现分析（三）](https://my.oschina.net/ybusad/blog/277840)