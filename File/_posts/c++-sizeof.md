---
title: C++ - sizeof用法和内存补齐
date: 2017-03-28 11:35:48
categories: C++
tags: [C++,sizeof,内存补齐]
keywords: C++,sizeof,内存补齐
---



* 本文对C++中sizeof运算符和内存补齐相关用法进行整理和归纳。

<!--more-->

## sizeof+数据类型

32位系统下，其结果如下所示。

```
cout << sizeof(char) << endl;  // 1
cout << sizeof(short) << endl;  // 2
cout << sizeof(int) << endl;  // 4
cout << sizeof(float) << endl;  // 4
cout << sizeof(long) << endl;  // 4
cout << sizeof(double) << endl;  // 8

cout << sizeof(int*) << endl;  // 4  指针
cout << sizeof(double*) << endl;  // 4  指针
cout << sizeof(char*) << endl;  // 4 指针
```

64位系统下，其结果如下所示。

```
cout << sizeof(char) << endl;  // 1
cout << sizeof(short) << endl;  // 2
cout << sizeof(int) << endl;  // 4
cout << sizeof(long) << endl;  // 8
cout << sizeof(float) << endl;  // 4
cout << sizeof(double) << endl;  // 8

cout << sizeof(int*) << endl;  // 4  指针
cout << sizeof(double*) << endl;  // 4  指针
cout << sizeof(char*) << endl;  // 4 指针
```
## 内存对齐规则
参考资料
* [百度百科](http://baike.baidu.com/item/%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90)
* [内存对齐](http://www.jianshu.com/p/a371e2613ec8) 
* [5分钟搞定内存字节对齐](http://blog.csdn.net/hairetz/article/details/4084088)

每个特定平台上的编译器都有自己的默认**对齐系数**(也叫对齐模数)。可以通过预编译命令`#pragma pack(n)`，n=1,2,4,8,16来改变这一系数，其中的n就是指定的对齐系数。

> VC, VS等编译器默认`#pragma pack(8)`。而GCC默认是`#pragma pack(4)`，并且GCC只支持1,2,4对齐。
> 
> `#pragma pack(1)`表明对齐按照1的整数倍进行，即没有对齐。

内存对齐规则包括3条。

* 原则1：数据成员对齐规则
结构(struct)(或联合(union))的数据成员，第一个数据成员放在offset为0的地方，以后每个数据成员的对齐按照#pragma pack指定的数值和这个数据成员自身长度中，比较小的那个的整数倍进行。


* 原则2：结构体作为成员
如果一个结构里有某些结构体成员，则结构体成员，要从#pragma pack指定的数值和其内部最大元素大小中较小的那个的整数倍地址开始存储。(例如，struct A里存有struct B，B里有char, int, double等元素，那B应该从8的整数倍开始存储)

* 原则3：结构(或联合)的整体对齐规则
在数据成员完成各自对齐之后，结构(或联合)本身也要进行对齐，将按照#pragma pack指定的数值和结构(或联合)最大数据成员长度中，比较小的那个的整数倍对齐。

## 内存对齐原因
* 平台原因(移植原因)
不是所有的硬件平台都能访问任意地址上的任意数据的；某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常。
* 性能原因
经过内存对齐后，CPU的内存访问速度大大提升。其原因参考如下链接。
	- [内存对齐的原因](http://www.cppblog.com/snailcong/archive/2009/03/16/76705.html)

## 内存对齐实例
### 实例1

```cpp
struct BB
{
	//[0]...[3]   首元素的偏移量为0
	int id;  
	//[7]...[15]
	// 原则１: double变量的偏移量为min(n,8) = min(8,8) = 8
    double weight;     
    //原则1： float遍历的偏移量为min(n,4) = min(8,4) = 4
    //因此，存放在[16]...[19]
    //原则3：结构体进行对齐，min(n,maxLength) = min(8,8) = 8。
    //因此，结构体将按照8的整数倍对齐，结构体将补齐[20]..[23]
    float height;     
};

struct AA
{
	//[0],[1] 首元素的偏移量为0
	char name[2];  
	//[4]...[7]
	// 原则１: 偏移量是min(n,4) = min(8,4) = 4  
	int  id;        
	// [8] ... [15]
	//原则1： 偏移量是min(n,8) = min(8,8) = 8
	double score; 
    // [16][17]
	//原则1： 偏移量是min(n,2) = min(8,2) = 2
    short grade; 　
    //[24]...[47]
    //原则2：偏移量是min(n,innerMax) = min(8,8) = 8
    //从BB内容知道，sizeof BB = 24　　
    //原则3： 结构体本身进行内存补齐
    //min(n,maxLength) = min(8,24) = 24。 
    //此时占用的内存已经是24的整数倍了，不需要再进行处理　　　　　　
	BB b;            　　　　　　　
};


int main()
{
	AA a;
	BB b;
	cout << sizeof(a) << " " << sizeof(b) << endl;  //48 24
	cout << sizeof(AA) << " " << sizeof(BB) << endl;  //48 24
	return 0;
}
```
在VS中运行上述程序，默认的`#pragma pack(8)`。上述程序中，结构体BB的sizeof结果是24，结构体AA的sizeof结果是48。

如果显示指定`#pragma pack(8)`，则结构体BB的sizeof结果是24，结构体AA的sizeof结果是48。

sizeof(BB) =  4+8+4 = 16。16已经是min(1,maxLength) = min(1,8) = 1 的整数倍了，不需要再对结构体整体进行补齐。

sizeof(AA) = 2+4+8+2+16 = 32。32已经是min(1,maxLength) = min(1,16) = 1 的整数倍了，不需要再对结构体整体进行补齐。


### 实例2

```cpp
 //环境：vc6 + windows sp2
#include <iostream>
using namespace std;
 
struct st1 
{
	char a; //[0]
	//[1]..[3]补齐
	int  b; //[4]..[7]
	short c; //[8][9],  [10]..[12]补齐
};

struct st2
{
	short c; //[1][2]
	char  a; //[3]
	int   b; [4]..[7]
};
 
int main()
{
	cout<<"sizeof(st1) is "<<sizeof(st1)<<endl;  //12
	cout<<"sizeof(st2) is "<<sizeof(st2)<<endl;  //8
	return 0 ;
}
```
由于内存补齐，同样（功能相同）的结构体，变量定义顺序不同，其sizeof结果也不同。

## sizeof+类
参考资料：[sizeof（类）](http://www.cnblogs.com/raichen/p/5610679.html)


* **空类的sizeof结果是1。**
因为空类也要实例化。所谓的实例化就是在内存中分配一块地址。每个实例在内存中都有独一无二的地址，同样空类也会被实例化，所以编译器会给空类加一个字节，这样空类在实例化后就会有独一无二的地址了，所以空类的sizeof是1。

* **构造函数，析构函数和成员函数，不计入sizeof结果。**
sizeof是针对实例的，而构造函数，析构函数和成员函数是针对类本身的，它们是被多个实例公用的，自然无法归入实例的大小。

* 类的静态成员归类整体所有，不是实例所有。因此，不计入sizeof结果。

* **类的非静态成员和虚析构函数记入sizeof的结果。**
如果在类中声明了虚函数（**不管是1个还是多个**），那么在实例化对象时，编译器会自动在对象里安插一个指针指向虚函数表VTable，在32位/64位机器上，一个对象会增加4个字节来存储此指针，它是实现面向对象中多态的关键。而虚函数本身和其他成员函数一样，是不占用对象的空间的。即使父类和子类中都有虚函数，由于虚函数公用一个指向虚函数表VTable的指针，因此只引入4个字节。

* 类的sizeof计算结果，同样遵守内存补齐原则。

* 若存在类的继承关系，则子类的sizeof值要加上基类的sizeof长度。但是子类整体在进行补齐（内存补齐原则3）时候，不考虑基类的sizeof值，补齐按照`#pragma pack(n)`，以及子类中非静态成员或虚函数的最大长度中较小的那个的整数倍进行补齐。

* 若为子类虚继承父类，子类sizeof需要加上一个虚继承指针，即4字节。并且此时，子类和父类的虚函数也不再共用一个虚表函数，如下实例4所示。

### 实例1

```cpp
class Base  
{  
public:  
    Base();  
    ~Base();  
  
};
sizeof(Base);   // 1   空类
```

### 实例2

```
class Base  
{  
public:  
    Base();                  
    virtual ~Base();         //每个实例都有虚函数表   4字节
    virtual void fun(){}
    virtual void fun1(){}  
    //不管是1个还是多个虚函数，都只引入一个指针，只占4字节
    virtual void fun3(){}
    void set_num(int num)    //普通成员函数，为各实例公有，不归入sizeof统计  
    {  
        a=num;  
    }  
private:  
    int  a;                  //占4字节  
    char *p;                 //4字节指针  
};  
  
class Derive:public Base  
{  
public:  
    Derive():Base(){};       
    ~Derive(){};  
private:  
    static int st;         //非实例独占  
    int  d;                //占4字节  
    char *p;               //4字节指针  
    virtual void fun(){}   
    //父类和子类都有虚函数，只引入一个指针，只增加4字节
    char c;       //本身占1字节
    //但是由于内存补齐，类整体要补齐为4的整数倍，因此会再补齐3个字节（类整体进行补齐时候不考虑继承的类的sizeof值）
  
};  
  
int main()   
{   
    cout<<sizeof(Base)<<endl;    //12
    cout<<sizeof(Derive)<<endl;   //20
    return 0;  
}  
```

Base类里的`int  a;`和`char *p;`共占8个字节。而虚析构函数`virtual ~Base();`的指针占4子字节。不管是1个还是多个虚函数，都只引入1个指针，只增加4个字节。其他成员函数不归入sizeof统计。因此，`sizeof(Base) = 12`。

Derive类首先要具有Base类的部分，也就是占12字节。`int  d;`, `char *p;`占8字节，`static int st;`不归入sizeof统计。`char c`占1个字节，但是由于内存补齐，会增补3个字节（类整体进行补齐时候不考虑继承的类的sizeof值），所以一共是20+4=24字节。（子类和父类都有虚函数，但是只引入一个指针，只增加4个字节）

由此，可以总结如下结论。
>* 类的大小为类的**非静态**成员数据的类型大小之和，也就是说静态成员数据不作考虑。
>* 普通成员函数与sizeof无关。
>* 虚函数由于要维护在虚函数表，所以要占据一个指针大小，也就是4字节。
>* 类的总大小也遵守类似class字节对齐的调整规则。类整体进行补齐时候不考虑继承的类的sizeof值。


### 实例3
子类虚继承父类

```
#include <iostream> 
using namespace std;

class A
{
    int num;
};
class B : virtual public A
{
    int num2;
};

int main()
{
    cout << sizeof(A) << endl;  //4
    cout << sizeof(B) << endl;  //12
    system("pause");
}
```

 sizeof(子类)=sizeof(基类)+sizeof(虚表指针)+sizeof(子类数据成员)

### 实例4
 
 **如果子类和基类都有虚函数，并且是虚继承的话，各自用各自的虚表**。

```cpp
#include <iostream> 
using namespace std;

class A
{
    int num;  //4字节
    virtual void fun(){}  //4字节
};
class B : virtual public A  //虚继承
{
    int num2;   //4字节
    virtual void fun1(){}   //4字节
};

int main()
{
    cout << sizeof(A) << endl;  //8
    cout << sizeof(B) << endl;  //20 = 8+4+4+4
    system("pause");
}
```


sizeof(子类) = sizeof(基类)+  sizeof(虚继承指针) +  sizeof(子类成员变量) +  sizeof(子类虚表指针) 

## 考题1

```cpp
#include<iostream>
using namespace std;

class Base {
public:
	virtual int foo(int x) {
		return x * 10;
	}
	int foo(char x[14])
	{
		return sizeof(x) + 10;
	}
};
class Derived : public Base {
	int foo(int x) {
		return x * 20;
	}
	virtual int foo(char x[10]) {
		return sizeof(x) + 20;
	}
};

int main() {
	Derived stDerived;
	Base *pstBase = &stDerived;
	char x[10];

	printf("%d\n", pstBase->foo(100));  //2000
	printf("%d\n", pstBase->foo(x));  //14
	printf("%d\n", pstBase->foo(100) + pstBase->foo(x)); //2014

	system("pause");
	return 0;
}
```
上述程序运行结果是？（2000,14,2014）

### 分析
* `pstBase->foo(100)`调用子类的foo函数，因为父函数有virtual修饰，被子类的同名函数覆盖。因此，其结果为2000。
* `pstBase->foo(x)`调用父类函数。因为父函数没有有virtual修饰。`int foo(char x[14])` 中，函数参数传递时，数组是以指针的方式传递的，指针是4个字节。 所以结果是14。
* 因此，第3行求和的结果是2014。




## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 
