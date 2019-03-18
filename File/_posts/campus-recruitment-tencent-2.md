---
title: 腾讯2017前端笔试题
date: 2017-04-04 19:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,腾讯笔试]
keywords: 校招笔试编程题,腾讯笔试
---





* 本文对腾讯2017前端实习生笔试题目进行记录和归纳。

<!--more-->

## 选择题1 - 移位操作

```cpp
short result = 2017;
result = result << 5;
result = result >> 5;
cout<<result<<endl;
```
A. -32737
A. -31
C.  2017
D. 65505


### 求解

正确答案为B（-31）


关于补码，反码和原码的介绍，参考[补码-原码 介绍](https://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)。


short占2个字节，其二进制位数为16位，最高位为符号位。

2017 = 0000, 0111, 1110, 0001

2017<<5 = 1111, 1100, 0010, 0000     

2017<<5之后，符号为1，表示负数。**负数的补码等于其原码取反再加1（符号位不取反）。补码是用于存储的，计算负数实际的值要化为其原码计算。**因此，其表示的负数为-992。


2017<<5 = 1111, 1100, 0010, 0000    （补码）
减去1，得  1111, 1100, 0001, 1111      (反码)
取反（符号位不取反）得，1000, 0011, 1110, 00000  （原码）
表示-((2^5)+(2^6)+(2^7)+(2^8)+(2^9)) = -992
 




在上述结果上再右移5位，高位补充1。

(2017<<5)>>5 = 1111, 1111,  1110, 0001 。其结果为-31。

**数字右移时，高位填充的是符号位。正数右移高位填充0，负数右移高位填充1。**
 


(2017<<5)>>5 = 1111, 1111,1110, 0001   （补码）
减去1，得       1111, 1111,1110, 0000    (反码)
取反（符号位不取反）得，1000, 0000, 0001, 1110  （原码）
表示-((2^5)-1) = -31
 


## 选择题2 - 树的度数
设树T的度为4，其中度为1，2，3，4的节点个数分别是4，2，1，1，那么T中的叶子树为多少个（即度为0的结点有多少）
    
### 分析
* 结点拥有的子树数称为结点的度（Degree）。

* 度为0的结点称为叶结点（Leaf）或终端结点。度不为0的结点称为非终端结点或者分支结点。除根节点之外，分支结点也称为内部结点。

* 树的度是树内部各结点的度的最大值。
* 树中，**边的个数 =  结点数 - 1**

![tree-degree-1.jpg](http://ol3kbaay9.bkt.clouddn.com/tree-degree-1.jpg)

* 二叉树的度，只有0，1，2三种情况：度为0（叶子节点），度为1（只有一个孩子），度为2（一定有两个孩子）
* **结论：二叉树中，度为0的节点（叶子结点）是度为2的数量加1。**

* 结论证明

一个**二叉树**，共有结点N，二度结点为X，一度结点为Y，求叶子节点的个数Z。

N个结点总共N - 1条边，一度节点一条边，二度节点二条边，叶子节点0条边。则有如下方程组。

```
2X  + Y = N - 1
X + Y  + Z = N
```

求解得，Z = X + 1。即**二叉树中，度为0的节点（叶子结点）是度为2的数量加1。**

### 求解
由于一条边有两个结点，两条边有三个节点，所以边的个数 =  结点数 -1。

度数为4的结点有4条边，度数为3的结点有3条边，度数为2的结点有2条边，度数为1的结点有1条边。度数为0的结点有0条边。

```
//总边数 n
n = 4*1+3*1+2*2+1*4 = 15
//总结点数 item
item = n+1 = 16
//度数为0的结点 （叶子节点）
总结点个数 - （度数不为0的结点个数）
= 16 - (4+2+1+1) 
= 8
```
                

## 选择题3 - IPv6
IPv4和IPv6地址分别有多少位？
IPv6是否采用了高效IP头？
IPv6中主机地址能否自动配置？

### 求解
* IPv4地址是32位。IPv6地址是128位。
* IPv6采用了高效IP头。
* IPv6支持主机地址自动配置。

**IPv6的地址有16字节，即128位。可以彻底解决IPv4地址不足的问题。除此之外，IPv6还采用分级地址模式，高效IP包头，服务质量，主机地址自动配置，认证和加密等许多技术。**

### 分析

参考资料
* [IPv4 和 IPv6](https://wenku.baidu.com/view/13ff21740740be1e640e9a71.html?from=search)
#### IPv4
IPv4是互联网协议(Internet Protocaol, IP)的第4版，也是第1个被广泛使用，构成现今互联网技术的基石的协议。

**IPv4的地址位有4字节，即32位。因此，最多有2^32 （约40亿）的主机可以被连接到Internet上。**

IP地址被划分为A, B, C, D和E共5大类。其中，只有A类，B类，C类是可供一般主机使用的IP地址。

![ip-structure-2.PNG](http://ol3kbaay9.bkt.clouddn.com/ip-structure-2.PNG)

##### IPv4潜伏的3大危机
* 地址枯竭
IPv4的地址为32位，可提供2^32 （约40亿）的IP地址。但因将IP地址按网络规模划分成A，B，C和D，E类后，用户可用地址总数显著减少。根据预测，到2003~2005年，IPv4地址将被用尽。
* 网络号码匮乏
在IPv4中，A类网络只有126个，每个能容纳1亿多个主机；B类网络仅有16382个，每个能容纳6万多个主机；C类网络虽然多达209万个，但是每个只能容纳254个主机。
* 路由表急剧膨胀
IPv4的地址体系结构是非层次化的，每增加一个子网，路由器就增加一个表项，使路由器不堪重负。

##### 解决IPv4危机的措施
* 暂时缓解三大危机的措施
	- 利用内部地址弥补IP地址的不足
	- 用CIDR（无类型网络区域路由技术）扩大网络号码
	- 地址层次化可抑制路径数
* 彻底消除三大危机的措施 - 导入IPv6

IPv6是一个新的IP协议。新的IP协议的设计目标为
* 支持更多的主机
* 减小路由表的长度
* 简化协议，以加快包处理
* 提供更好地安全性
* 通过指定范围来改进组播
* 重视服务类型
* 主机可不必改变地址而漫游
* 协议可以继续发展
* 允许新旧协议共存


#### IPv6
IPv6是下一版本的互联网协议。IPv4定义的有限地址空间将被耗尽，地址空间的不足必将妨碍互联网的进一步发展。为了扩大地址空间，拟通过IPv6重新定义地址空间。

**IPv6的地址有16字节，即128位。可以彻底解决IPv4地址不足的问题。除此之外，IPv6还采用分级地址模式，高效IP包头，服务质量，主机地址自动配置，认证和加密等许多技术。**

IPv6和IPv4是不兼容的，但它们同所有其他的TCP/IP协议簇中的协议兼容。即IPv6完全可以取代IPv4。同IPv4相比较，IPv6在地址容量，安全性，网络管理，移动性以及服务质量等方面有明显改进。IPv6继承了IPv4的优点，摒弃了IPv4的缺点。

##### IPv6地址的对策
IPv6对IPv4所做的主要修改有
* 将IP地址从32位增加到128位（4字节增加到16字节）
* 通过增加一个作用域字段而改进了多点播送地址
* 新的任意播送（anycast）IP地址类型用于向组内任何成员发送包，通常是最近的组成员。
* 用可选的扩展头部替代头部中选项字段
* 删除头部校验和字段
* 删除所有分段处理所用的字段，所以仅执行端到端的分段
* 新的流标号字段可用来标识特定的用户数据流或通信量类型
* 扩展了对认证，数据一致性和（可选的）数据保密的支持

##### IPv6地址的表示
IPv6地址为128位长度，通常写做8组，每组4个十六进制数字。

例如，`2001:0db8:85a3:08d3:1319:8a2e:0370:7344`是一个合法的IPv6地址。

IPv6地址中如果一组的4个数字全部是0，则可以省略（分组的冒号仍然保留）。

例如，`2001:0db8:85a3:0000:1319:8a2e:0370:7344`可以省略书写为`2001:0db8:85a3::1319:8a2e:0370:7344`。

遵从上述规则，如果因为省略而出现了两个以上的冒号的话，可以压缩为1个（再加上非零组的冒号，最后仍然是2个冒号），但是这种零压缩在地址中只能出现1次。否则，会搞不清楚每个压缩中有几个全0的分组。


例如，`2001:0db8:0000:0000:0000:8a2e:0370:7344`可以省略书写为`2001:0db8:0:0:0:8a2e:0370:7344`或者`2001:0db8::8a2e:0370:7344`。但是，`2001::0db8:0370::7344`是不合法的。


IPv6地址中的前导0是可以省略的。

例如，`2001:0db8:02de:0123`可以省略书写为`2001:db8:2de:123`。

##### IPv6中的地址配置
IPv6继承了IPv4的自动配置服务，并将其称为全状态自动配置（stateful autoconfiguration）。除了全状态自动配置，IPv6还采用了一种被称为无状态自动配置（stateless autoconfiguration）的自动配置服务。

##### IPv6中的报头简化
IPv6对数据报头作了简化，以减少处理器开销并节省网络带宽。IPv6的报头由一个基本报头和多个扩展报头（Extension Header）构成。基本报头具有固定的长度（40字节）。

由于Internet上的绝大部分包都只是被路由器简单的转发，因此固定的报头长度有助于加快路由速度。

#### IPv4和IPv6的对比
参考[IPv4 VS IPv6 | IBM](https://www.ibm.com/support/knowledgecenter/zh/ssw_ibm_i_61/rzai2/rzai2compipv4ipv6.htm)


## 选择题4 - 变量声明

```
var a = b = 3;
```
上述程序有无错误？

### 求解

上述程序编译无错。

相当于

```javascript
var a;
var b;
a = b = 3;
```

## 选择题5 - display和visible区别
### 题目
分析display和visible的区别
### 求解
* `display: none`和`visible: hidden`都能把网页上某个元素隐藏起来，但两者有区别。
* `display: none`：不为被隐藏的对象保留其物理空间，即该对象在页面上彻底消失，通俗来说就是看不见也摸不到。
* `visible: hidden` ：使对象在网页上不可见，但该对象在网页上所占的空间没有改变，通俗来说就是看不见但摸得到。


```javascript
<html>
<head>
	<meta charset="utf-8">
	<style type="text/css">
		body{
			margin:10px;
		}
	</style>
</head>
<body>
	<span style="display:none; background-color:red">
		隐藏区域
	</span>
	<span style=" background-color:blue">
		显示区域
	</span>
	<br />
	<span style="visibility:hidden; background-color:red">
		隐藏区域
	</span>
	<span style="background-color:blue">
		显示区域
	</span>
</body>
</html>
```

![display-1.PNG](http://ol3kbaay9.bkt.clouddn.com/display-1.PNG)

## 选择题6 - 堆和栈
### 题目
简要分析堆和栈的区别
### 分析
#### C++中内存分区
在C++中，内存分成5个区，分别为堆，栈，自由存储区，全局/静态存储区和常量存储区。

* 栈（Stack）区
存放局部变量，函数参数等。分配时不处理内存，即变量取随机值。栈区中的变量或参数所占的内存会自动释放。
* 堆（Heap）区
即由`new`分配的内存块。编译器不会自动释放这些内存块，需要程序员手动进行释放，使用delete释放内存。如果没有手动释放，那么在程序结束后，操作系统会自动回收。
* 自由存储区
即由`malloc`等分配的内存块。自由存储区和堆区比较类似，只不过要使用`free`释放内存。
* 全局/静态存储区
全局变量和静态变量被分配到同一块内存中。未初始化的变量会被初始化为0或者空串。
* 常量存储区
常量存储区存放的是常量，不允许修改。


```cpp
void f(){
	int* p = new int[5];
}
```
例如，上述程序中，使用`new`分配了一块堆内存，用于存放大小为5的整型数组。其次，在栈上创建了一个指针p，该指针指向堆内存上的一个数组。若要释放堆上的内存，可以采用`delete []p`。


#### 堆和栈的区别
堆和栈主要表现为以下6点不同。参考[堆和栈](http://www.cnblogs.com/hanyonglu/archive/2011/04/12/2014212.html)了解更多。

1 - 管理方式不同
2 - 空间大小不同
3 - 能否产生碎片不同
4 - 生长方向不同
5 - 分配方式不同
6 - 分配效率不同

* 管理方式不同
对于栈来讲，变量或参数的内存由编译器自动管理，自动分配内存和释放内存。对于堆来讲，使用`new`分配内存，需要手动调用`delet`释放内存。如果忘记手动释放内存，容易产生内存泄漏（memory leak）。

* 空间大小不同
一般来讲，在32位系统下，堆内存可以达到4G的空间。从这个角度来看堆内存几乎是没有什么限制的。但是对于栈来讲，一般都是有一定的空间大小的。
* 能否产生碎片不同
对于堆来讲，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不存在碎片问题。因为栈是先进后出的队列。
* 生长方向不同
对于堆来讲，生长方向是向上的，也就是向着内存地址增加的方向。**对于栈来讲，它的生长方向是向下的，是向着内存地址减小的方向增长。**

> 栈的生长方式是从高到低的。即向着内存地址减小的方向增长。
> 堆的生长方式是从低到高的。即向着内存地址增加的方向增长。


* 分配方式不同
堆都是动态分配的，没有静态分配的对。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由`alloca`函数进行分配。但是栈的动态分配和堆是不同的，它的动态分配是由编译器进行释放的，无需手动释放。
* 分配效率不同
栈的分配效率较高。栈是机器系统提供的数据结构，计算机会在底层对栈提供支持，这就决定了栈的效率比较高。而堆是C++函数库提供的，效率较低。

## 选择题7 - struct 和 class 区别
### 题目

比较struct和class的区别。

### 求解
* struct能包含成员函数
* struct能继承
* struct能实现多态
* struct可以有自己的构造函数和析构函数
* struct支持private, protected和public


struct和class的区别主要包括
* 默认继承权限不同
class默认继承权限为private，而struct默认继承权限为public。
* 成员默认访问权限不同
class默认继承权限为private，而struct默认继承权限为public。
* 模板参数
struct不能作为模板参数，而class可以作为模板参数。



## 选择题8 - 交换机
### 题目
交换机工作在OSI七层的哪一层？

A. 一层
B. 二层
C. 三层
D. 三层以上

### 求解
正确答案是B（二层，数据链路层）。

* OSI七层结构分别是：物理层，数据链路层，网络层，传输层，会话层，表示层和应用层。
* 集线器工作在物理层
* 交换机主要工作在数据链路层，俗称“二层交换机”，但也有多层交换机，如三层交换机工作在第3层- 网络层。
* 路由器工作在网络层
* **交换机根据mac地址进行转发。路由根据IP进行转发。**
交换机是利用物理地址（MAC地址）来确定转发数据的目的地址。而路由器则是利用不同网络的ID号（即IP地址）来确定数据转发的地址。IP地址是在软件中实现的，描述的是设备所在的网络。MAC地址通常是硬件自带的，由网卡生产商来分配的，而且已经固化到了网卡中去，一般来说是不可更改的。而IP地址则通常由网络管理员或系统自动分配。

* 数据链路层的功能
	- 数据链路的建立，维护与拆除
	- 帧包装，帧传输，帧同步
	- 帧的差错恢复
	- 流量控制

## 选择题9 - 进程/线程

### 题目
进程和线程的同步机制有哪些？

A、互斥
B、信号量
C、条件变量
D、共享内存

### 求解
正确答案是A和B。

### 分析
参考[进程/线程同步的方式和机制](http://www.cnblogs.com/memewry/archive/2012/08/22/2651696.html)和[进程和线程](http://www.cnblogs.com/youngforever/p/3250270.html)了解更多。

进程和线程的同步机制包括4种方式。
* 临界区（Critical Section）
* 互斥量（Mutex）
* 信号量（Semaphore）
* 事件（Event）

1 - 临界区
通过对多线程的串行化来访问公共资源或一段代码，速度快，适合控制数据访问。在任意时刻只允许一个线程对共享资源进行访问，如果有多个线程试图访问公共资源，那么在有一个线程进入后，其他试图访问公共资源的线程将被挂起，并一直等到进入临界区的线程离开，临界区在被释放后，其他线程才可以抢占。

2 - 互斥量
采用互斥对象机制。 只有拥有互斥对象的线程才有访问公共资源的权限，因为互斥对象只有一个，所以能保证公共资源不会同时被多个线程访问。互斥不仅能实现同一应用程序的公共资源安全共享，还能实现不同应用程序的公共资源安全共享。互斥量比临界区复杂。因为使用互斥不仅仅能够在同一应用程序不同线程中实现资源的安全共享，而且可以在不同应用程序的线程之间实现对资源的安全共享。

3 - 信号量
它允许多个线程在同一时刻访问同一资源，但是需要限制在同一时刻访问此资源的最大线程数目 .信号量对象对线程的同步方式与前面几种方法不同，信号允许多个线程同时使用共享资源，这与操作系统中的PV操作相同。它指出了同时访问共享资源的线程最大数目。它允许多个线程在同一时刻访问同一资源，但是需要限制在同一时刻访问此资源的最大线程数目。

4 - 事件
通过通知操作的方式来保持线程的同步，还可以方便实现对多个线程的优先级比较的操作 .

## 选择题10 - HTTP2.0新特性
### HTTP2.0新特性知识点复习
参考资料如下
* [HTTP 2 新特性简析](https://zhuanlan.zhihu.com/p/19969498?columnSlug=ye11ow)
* [HTTP 2.0的奇妙日常](http://www.alloyteam.com/2015/03/http2-0-di-qi-miao-ri-chang/)
* [HTTP 2.0 的新特新](http://jiaolonghuang.github.io/2015/08/16/http2/)
* [HTTP 2.0 的新特新浅析](http://io.upyun.com/2015/05/13/http2/)


> 术语补充
> 
>**流**：已建立的连接上的双向字节流。具有唯一的流ID，客户端发起的为奇数ID，服务端发起的为偶数ID。很多个流可以并行的在同一个tcp连接上交换消息。
>
> **消息**：与逻辑消息对应，比如一个请求或一个响应。由一个或多个帧组成。
>
>**帧**：HTTP2中最小的通信单位，每个帧都会有帧首部，每个帧或者用来承载HTTP首部或负荷数据，或其他特定类型的帧。帧是遵循二进制编码的。

#### 基于二进制的协议
这可能是HTTP/2对比HTTP/1.X最“颠覆”的改动。二进制协议的优势显而易见：解析开销更小，描述协议也更高效。

HTTP/2 采用二进制格式传输数据，而非 HTTP/1.x 的文本格式。二进制格式在协议的解析和优化扩展上带来更多的优势和可能。


HTTP1.x用回车换行符作为纯文本的分隔符，在进行解析和差错检测时不方便。


HTTP2引入分帧层后，将报文分隔成一个个更小的帧，并采用二进制编码的方式。通常会将一个消息（首部和数据在一起的）分成一个HEADER帧和若干个DATA帧。如下图所示。

![http2.0-frame-layer.png](http://ol3kbaay9.bkt.clouddn.com/http2.0-frame-layer.png)

#### 头部压缩
HTTP/2 对消息头采用 HPACK 进行压缩传输（一边用index mapping table压缩，一边编码），能够节省消息头占用的网络的流量。而 HTTP/1.x 每次请求，都会携带大量冗余头信息，浪费了很多带宽资源。头压缩能够很好的解决该问题。

#### 连接的多路复用
HTTP/2的连接通过**流**支持了多路复用。这也意味着，一个HTTP/2的TCP连接，可以去请求**多个资源**。从而可以减少建立的TCP连接数。

#### 流的优先级
HTTP/2规定客户端可以显式的指定每个流的优先级，从而在服务器端资源有限的时候，可以按照优先级顺序来传送数据。

#### 服务器推送 (Server Push)
HTTP/2里面的Server Push并不是指类似于现在的Server Sent Event或者websocket的推送技术。它是一种服务器根据客户端以前发送的请求来“猜测”未来的请求，并提前将未来请求的结果推送给客户端的技术。

以打开知乎首页为例，浏览器发送GET请求到`www.zhihu.com`，服务器会返回知乎首页的html文件。如果是HTTP 1.x，那么浏览器会解析当前html之后，再向服务器发送JavaScript/CSS的请求，接着服务器返回这些资源文件。而如果是HTTP/2的话，服务器在发回html文件的时候，就能“猜测”到浏览器需要html里面包含的JavaScript/CSS文件，所以在浏览器做解析并发送请求之前，就主动将这些资源文件推送给浏览器。


#### 重置流
在HTTP/1.X时代，当客户端在接收服务器发回的响应时想终止这个HTTP请求，那么它只能直接中断底层的TCP连接。这样做的代价是：你需要重新建立一个新的TCP连接来发送新的请求。

而HTTP/2的客户端只需要发送一个含重置标记（RST_STREAM）的帧，就可以只中断当前的流而不是整个TCP连接，从而节省下很多开销。






## 问答1 - 优化网站性能

### 题目
如何对网站性能进行优化？

### 分析
参考资料如下。
* [Best Practices for Speeding Up Your Web Site | Yahoo](https://developer.yahoo.com/performance/rules.html)
* [网站性能优化 | Yahoo军规 ](http://ngaaron.com/1747)
* [雅虎前端31条优化](https://github.com/creeperyang/blog/issues/1)
* [前端优化 | Yahoo](http://m.xuehuile.com/blog/a3f67121025e4cdd8e3645e23bf64bd3.html)

### 求解
#### A. Conntent
##### A-1.  Make Fewer HTTP Requests

Minimize HTTP Requests减少/最小化 http 请求数。

到终端用户的响应时间80%花在前端：大部分用于下载组件（js/css/image/flash等等）。减少组件数就是减少渲染页面所需的http请求数。这是更快页面的关键。

减少组件数的一个方法就是简化页面设计。保持富内容的页面且能减少http请求，有以下几个技术

* Combined files。
合并文件，如合并js，合并css都能减少请求数。如果页面间脚本和样式差异很大，合并会更具挑战性。
* CSS Sprites。
雪碧图可以合并多个背景图片，通过`background-image` 和 `background-position` 来显示不同部分。
* Image maps。
合并多个图片到一个图片，一般用于如导航条。由于定义坐标的枯燥和易错，一般不推荐。
* Inline images。
使用`data:url scheme`来内连图片。

减少请求数是为第一次访问页面的用户提高性能的最重要的指导。

##### A-2. Reduce DNS Lookups

减少DNS查询。

就像电话簿，你在浏览器地址栏输入网址，通过DNS查询得到网站真实IP。

DNS查询被缓存来提高性能。这种缓存可能发生在特定的缓存服务器（ISP/local area network维护），或者用户的计算机。DNS信息留存在操作系统DNS缓存中（在windows中就是 DNS Client Serve ）。大多浏览器有自己的缓存，独立于操作系统缓存。只要浏览器在自己的缓存里有某条DNS记录，它就不会向操作系统发DNS解析请求。

IE默认缓存DNS记录30分钟，FireFox默认缓存1分钟。

当客户端的DNS缓存是空的，DNS查找次数等于页面中的唯一域名数。

减少DNS请求数可能会减少并行下载数。避免DNS查找减少响应时间，但减少并行下载数可能会增加响应时间。指导原则是组件可以分散在至少2个但不多于4个的不同域名。这是两者的妥协。

##### A-3. Avoid Redirects

避免跳转。

跳转用301或302状态码来达成。一个301响应http头的例子：

```
HTTP/1.1 301 Moved Permanently
Location: http://example.com/newuri
Content-Type: text/html
```

浏览器自动跳转到Location指定的路径。跳转所需的所有信息都在http头，所以http主体一般是空的。301和302响应一般不会被缓存，除非有额外的头部信息，比如Expires或Cache-Control指定要缓存。meta刷新标签或 JavaScript 也可以跳转，但如果真要跳转，3xx跳转更好，主要是保证返回键可用。

跳转显然拖慢响应速度。在跳转的页面被获取前浏览器没什么能渲染，没什么组件能下载。

**最浪费的跳转之一发生在url尾部slash（/）缺失。**

比如`http://astrology.yahoo.com/astrology`会301跳转到`http://astrology.yahoo.com/astrology/`。这可以被Apache等服务器修复，用Alias，mod_rewrite等等。

##### A-4. Make Ajax Cacheable
让Ajax可缓存。

使用ajax的好处是可以向用户提供很快的反馈，因为它是向后台异步请求数据。但是，这些异步请求不保证用户等待的时间——异步不意味着瞬时。

提高ajax性能的最重要的方法是让响应被缓存，即在[Add an Expires or a Cache-Control Header](https://developer.yahoo.com/performance/rules.html#expires)中讨论的Expires。其它方法是

* gzip组件
* 减少DNS查找
* 压缩JS
* 避免跳转
* 设置ETags

##### A-5. Post-load Components
延迟加载组件。

再看看你的页面然后问问自己，“什么是页面初始化必须的？”。剩下的内容和组件可以延迟。

JavaScript是理想的（延迟）候选者，可以切分到onload事件之前和之后。比如拖放的js库可以延迟，因为拖动必须在页面初始化之后。其它可延迟的包括隐藏的内容，折叠起来的图片等等。

##### A-6. Preload Components
预加载组件。

预加载看起来与延迟加载相反，但它的确有个不同的目标。通过预加载你可以利用浏览器的空闲时间来请求你将来会用到的组件。这样当用户访问下一个页面时，你会有更多的组件已经在缓存中，这样会极大加快页面加载。

有几种预加载类型

* 无条件预加载
一旦onload触发，你立即获取另外的组件。比如谷歌会在主页这样加载搜索结果页面用到的雪碧图。
* 有条件预加载
基于用户动作，你推测用户下一步会去哪里并加载相应组件。
* 预期的预加载
在发布重新设计（的网站）前提前加载。在旧网页预加载新网页的部分组件，那么切换到新网页时就不会是没有任何缓存了。

##### A-7. Reduce the Number of DOM Elements
减少DOM数。

一个复杂的页面意味着更多的内容要下载，以及更慢的DOM访问。比如在有500 DOM数量的页面添加事件处理就和有5000 DOM数量的不同。

如果你的页面DOM元素很多，那么意味着你可能需要删除无用的内容和标签来优化。

##### A-8. Split Components Across Domains

把组件分散到不同的域名。

把组件分散到不同的域名允许你最大化并行下载数。由于DNS查询的副作用，最佳的不同域名数是2-4。

##### A-9. Minimize the Number of iframes

最小化iframe的数量。

iframe允许html文档被插入到父文档。

`<iframe>`优点

* 帮助解决缓慢的第三方内容的加载，如广告和徽章
* 安全沙盒
* 并行下载脚本

`<iframe>`缺点
* 即使空的也消耗（资源和时间）
* 阻塞了页面的onload
* 非语义化（标签）

##### A-10. No 404s
不要404。

http请求是昂贵的，所以发出http请求但获得没用的响应（如404）是完全不必要的，并且会降低用户体验。

一些网站会有特别的404页面提高用户体验，但这仍然会浪费服务器资源。特别坏的是当链接指向外部js但却得到404结果。这样首先会降低（占用）并行下载数，其次浏览器可能会把404响应体当作js来解析，试图从里面找出可用的东西。

#### B. Server
##### B-1. Use a Content Delivery Network
使用CDN。

用户接近你的服务器会减少响应时间。把你的内容发布到多个，地理上分散的服务器可以让页面加载更快。但怎么开始？

首先不要试图把你的架构重新设计成分布式架构。因为可能引进更多复杂性和不可控。

记住80-90%的终端用户响应时间花费在下载页面中的所有组件：图片、样式、脚本、falsh等等。这是`_Performance Golden Rule_`。不要从困难的重新设计后台架构开始，最好首先分发你的静态内容。这不仅可以减少响应时间，用CDN还很容易来做。

CDN是一群不同地点的服务器，可以更高效地分发内容到用户。一些大公司有自己的CDN。

##### B-2. Add an Expires or a Cache-Control Header
加Expires或者Cache-Control头部。

这条规则有两个方面
* 对静态组件，通过设置Expires头部来实现“永不过期”策略。
* 对动态组件，用合适的Cache-Control头部来帮助浏览器进行有条件请求。

页面越来越丰富，意味着更多脚本，样式，图片等等。第一次访问的用户可能需要发出多个请求，但使用Expires可以让这些组件被缓存。这避免了访问子页面时没必要的http请求。Expires一般用在图片上，但应该用在所有的组件上。

浏览器（以及代理）使用缓存来减少http请求数，加快页面加载。服务器使用http响应的Expires头部来告诉客户端一个组件可以缓存多久。比如下面

```
Expires: Thu, 15 Apr 2010 20:00:00 GMT //2010-04-15之前都是稳定的
```
注意，如果你设置了`Expires`头部，当组件更新后，你必须更改文件名。

##### B-3. Gzip Components
传输时用gzip等压缩组件。

http请求或响应的传输时间可以被前端工程师显著减少。终端用户的带宽，ISP，接近对等交换点等等没法被开发团队控制，但是，压缩可以通过减少http响应的大小减少响应时间。

从HTTP/1.1开始，客户端通过http请求中的`Accept-Encoding`头部来提示支持的压缩：

```
Accept-Encoding: gzip, deflate
```

如果服务器看到这个头部，它可能会选用列表中的某个方法压缩响应。服务器通过Content-Encoding头部提示客户端

```
Content-Encoding: gzip
```

gzip一般可减小响应的70%。尽可能去gzip更多（文本）类型的文件。html，脚本，样式，xml和json等等都应该被gzip，而图片，pdf等等不应该被gzip，因为它们本身已被压缩过，gzip它们只是浪费cpu，甚至增加文件大小。

##### B-4. Configure ETags
实体标记（Entity tags，ETag）是服务器和浏览器之间判断浏览器缓存中某个组件是否匹配服务器端原组件的一种机制。实体就是组件：图片，脚本，样式等等。ETag被当作验证实体的比最后更改（last-modified）日期更高效的机制。服务器这样设置组件的ETag

```
HTTP/1.1 200 OK
Last-Modified: Tue, 12 Dec 2006 03:03:59 GMT
ETag: "10c24bc-4ab-457e1c1f"
Content-Length: 12195
```

之后，如果浏览器要验证组件，它用If-None-Match头部来传ETag给服务器。如果ETag匹配，服务器返回304。

```
GET /i/yahoo.gif HTTP/1.1
Host: us.yimg.com
If-Modified-Since: Tue, 12 Dec 2006 03:03:59 GMT
If-None-Match: "10c24bc-4ab-457e1c1f"
HTTP/1.1 304 Not Modified
```

ETag的问题是它们被构造来使它们对特定的运行这个网站的服务器唯一。浏览器从一个服务器获取组件，之后向另一个服务器验证，ETag将不匹配。然而服务器集群是处理请求的通用解决方案。

如果不能解决多服务器间的ETag匹配问题，那么删除ETag可能更好。

##### B-5. Flush the Buffer Early
早一点刷新buffer（尽早给浏览器数据）。

当用户请求一个页面，服务器一般要花200-500ms来拼凑整个页面。这段时间，浏览器是空闲的（等数据返回）。在php，有个方法flush()允许你传输部分准备好的html响应给浏览器。这样的话浏览器就可以开始下载组件，而同时后台可以继续生成页面剩下的部分。这种好处更多是在忙碌的后台或轻前端网站可以看到。

一个比较好的flush的位置是在head之后，因为浏览器可以加载其中的样式和脚本文件，而后台继续生成页面剩余部分。

```
<!-- css, js -->
</head>
<?php flush(); ?>
<body>
<!-- content -->
```

##### B-6. Use GET for AJAX Requests
ajax请求用get。

Yahoo! Mail团队发现当使用XMLHttpRequest，POST 被浏览器实现为两步：首先发送头部，然后发送数据。所以使用GET最好，仅用一个TCP包发送（除非cookie太多）。IE的url长度限制是2K。

POST但不提交任何数据根GET行为类似，但从语义上讲，获取数据应该用GET，提交数据到服务器用POST。

##### B-7. Avoid Empty Image src

避免空src的图片。

空src属性的图片的行为可能跟你预期的不一样。它有两种形式

* html标签：`<img src="">`
* js：`var img = new Image(); img.src = "";`

两种都会造成同一种后果：浏览器会向你的服务器发请求。
* IE，向页面所在的目录发请求。
* Safari和Chrome，请求实际的页面。
* FireFox3及之前和Safari/Chrome一样，但从3.5开始修复问题，不再发请求。
* Opera遇到空图片src不做任何事。

为什么这种行为很糟糕？

1. 由于发送大量的意料之外的流量，会削弱服务器，尤其那些每天pv上百万的页面。
2. 浪费服务器计算周期取生成不会被浏览的页面。
3. 可能会破坏用户数据。如果你在跟踪请求状态，通过cookie或其它，你可能会破坏数据。即使image的请求不会返回图片，但所有的头部数据都被浏览器读取了，包括cookie。即使剩下的响应体被丢弃，破坏可能已经发生。

这种行为的根源是uri解析发生在浏览器。RFC 3986 定义了这种行为，空字符串被当作相对路径，Firefox, Safari, 和 Chrome都正确解析，而IE错误。总之，浏览器解析空字符串为相对路径的行为被认为是符合预期的。

html5在`_4.8.2_`添加了对标签src属性的描述，指导浏览器不要发出额外的请求。

> The src attribute must be present, and must contain a valid URL referencing a non-interactive, optionally animated, image resource that is neither paged nor scripted. If the base URI of the element is the same as the document's address, then the src attribute's value must not be the empty string.

幸运的是将来浏览器不会有这个问题了（在图片上）。不幸的是，`<script src="">`和`<link href="">`没有这样的规范。

#### C. Cookie
##### C-1. Reduce Cookie Size
http cookie的使用有多种原因，比如授权和个性化。cookie的信息通过http头部在浏览器和服务器端交换。尽可能减小cookie的大小来降低响应时间。

* 消除不必要的cookie。
* 尽可能减小cookie的大小来降低响应时间。
* 注意设置cookie到合适的域名级别，则其它子域名不会被影响。
* 正确设置Expires日期。早一点的Expires日期或者没有会尽早删除cookie，优化响应时间。

##### C-2. Use Cookie-free Domains for Components
用没有cookie的域名提供组件。

当浏览器请求静态图片并把cookie一起发送到服务器时，cookie此时对服务器没什么用处。所以这些cookie只是增加了网络流量。所以你应该保证静态组件的请求是没有cookie的。可以创建一个子域名来托管所有静态组件。

比如，你域名是www.example.org，可以把静态组件托管在static.example.org。不过，你如果把cookie设置在顶级域名example.org下，这些cookie仍然会被传给static.example.org。这种情况下，启用一个全新的域名来托管静态组件。

另外一个用没有cookie的域名提供组件的好处是，某些代理可能会阻止缓存待cookie的静态组件请求。

#### D. CSS
##### D-1. Put Stylesheets at the Top
把样式放在顶部。

研究雅虎网页性能时发现把样式表移到`<head>`里会让页面更快。这是因为把样式表移到`<head>`里允许页面逐步渲染。

关注性能的前端工程师希望页面被逐步渲染，这时因为，我们希望浏览器尽早渲染获取到的任何内容。这对大页面和网速慢的用户很重要。给用户视觉反馈，比如进度条的重要性已经被大量研究和记录。在我们的情况中，HTML页面就是进度条。当浏览器逐步加载页面头部，导航条，logo等等，这些都是给等待页面的用户的视觉反馈。这优化了整体用户体验。

把样式表放在文档底部的问题是它阻止了许多浏览器的逐步渲染，包括IE。这些浏览器阻止渲染来避免在样式更改时需要重绘页面元素。所以用户会卡在白屏。

HTML规范清楚表明样式应该在`<head>`里。

##### D-2. Avoid CSS Expressions
避免CSS表达式。

CSS表达式是强大的（可能也是危险的）设置动态CSS属性的方法。IE5开始支持，IE8开始不赞成使用。例如，背景颜色可以设置成每小时轮换

```
background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" );
```

CSS表达式的问题是它们可能比大多数人预期的计算的更频繁。它们不仅在页面载入和调整大小时重新计算，也在滚动页面甚至是用户在页面上移动鼠标时计算。比如在页面上移动鼠标可能轻易计算超过10000次。

要避免CSS表达式计算太多次，可以在它第一次计算后替换成确切值，或者用事件处理函数而不是CSS表达式。

##### D-3. Choose `<link>` over `@import`
选择`<link>`而不是`@import`。

之前的一个最佳原则是说CSS应该在顶部来允许逐步渲染。

在IE用`@import`和把CSS放到页面底部行为一致，所以最好别用。

##### D-4. Avoid Filters
避免使用（IE）过滤器。

IE专有的AlphaImageLoader过滤器用于修复IE7以下版本的半透明真彩色PNG的问题。这个过滤器的问题是它阻止了渲染，并在图片下载时冻结了浏览器。另外它还引起内存消耗，并且它被应用到每个元素而不是每个图片，所以问题（的严重性）翻倍了。

最佳做法是放弃AlphaImageLoader，改用PNG8来优雅降级。

#### E. JavaScript
##### E-1. Put Scripts at the Bottom
把脚本放到底部。

脚本引起的问题是它们阻塞了并行下载。HTTP1.1规范建议浏览器每个域名下不要一次下载超过2个组件。如果你的图片分散在不同服务器，那么你能并行下载多个图片。但当脚本在下载，浏览器不会再下载其它组件，即使在不同域名下。

有些情况下把脚本移动到底部并不简单。比如，脚本中用了document.write来插入内容，它就不能被移动到底部。另外有可能有作用域问题。但大多数情况，有方法可以解决这些问题。

一个替代建议是使用异步脚本。defer属性表明脚本不包含document.write，是提示浏览器继续渲染的线索。不幸的是，Firefox不支持。如果脚本能异步，那么也就可以移动到底部。

##### E-2. Make JavaScript and CSS External

使用外部JS和CSS。

这里的很多性能规则涉及外部组件怎么管理。但你首先要明白一个基本问题：JS和CSS是应该包含在外部文件还是內连在页面本身？

真实世界中使用外部文件一般会加快页面，因为JS和CSS文件被浏览器缓存了。內连的JS和CSS怎在每次HTML文档下载时都被下载。內连减少了http请求，但增加了HTML文档大小。另一方面，如果JS和CSS被缓存了，那么HTML文档可以减小大小而不增加HTTP请求。

核心因素，就是JS和CSS被缓存相对于HTML文档被请求的频率。尽管这个因素很难被量化，但可以用不同的指标来计算。如果网站用户每个session有多个pv，许多页面重用相同的JS和CSS，那么有很大可能用外部JS和CSS更好。

许多网站用这些指标计算后在中间位置。对这些网站来说，最佳方案还是用外部JS和CSS文件。唯一例外是內连更被主页偏爱，如`http://www.yahoo.com/`。主页每个session可能只有少量的甚至一个pv，这时候內连可能更快。

对多个页面的首页来说，可以通过技术减少（其它页面的）http请求。在首页用內连，初始化后动态加载外部文件，接下来的页面如果用到这些文件，就可以使用缓存了。

##### E-3. Minify JavaScript and CSS
压缩JS和CSS。

压缩就是删除代码中不必要的字符来减小文件大小，从而提高加载速度。当代码压缩时，注释删除，不需要的空格（空白，换行，tab）也被删除。

混淆是对代码可选的优化。它比压缩更复杂，并且可能产生bug。在对美国top10网站的调查，压缩可减小21%，而混淆可减小25%。

除了外部脚本和样式，內连的脚本和样式同样应该被压缩。

##### E-4. Remove Duplicate Scripts

删除重复的脚本。

在页面中引入相同的脚本两次会伤害性能。可能超出你的预料，美国top10网站的2家有重复脚本引入。两个主要因素造成同一页面引入相同脚本：团队大小和脚本数量。当确实引入重复脚本，会发出不必要的http请求和浪费js执行时间。

发出不必要的http请求发生在IE而不是Firefox。在IE，如果外部脚本引入两次且没有缓存，它会发出2个请求。即使脚本被缓存，刷新时也会发出额外请求。

除了增加http请求，时间被浪费在执行脚本多次上。不管IE还是Firefox都会执行多次。

一种避免多次引入脚本的方法是在模板系统实现一个脚本管理模块。

##### E-5. Minimize DOM Access

最小化DOM访问。

用JS访问DOM元素是缓慢的，所以为了响应更好的页面，你应该

* 缓存访问过的元素的引用
* 在DOM树外更新节点，然后添加到DOM树
* 避免用JS实现固定布局

##### E-6. Develop Smart Event Handlers

开发聪明的事件处理

有时候页面看起来不那么响应（响应速度慢），是因为绑定到不同元素的大量事件处理函数执行太多次。这是为什么使用_事件委托_是一种好方法。

另外，你不必等到onload事件来开始处理DOM树，DOMContentLoaded更快。大多时候你需要的只是想访问的元素已在DOM树中，所以你不必等到所有图片被下载。

#### F. Images
##### F-1. Optimize Images
优化图片

在设计师建好图片后，在上传图片到服务器前你仍可以做些事

* 检查gif图片的调色板大小是否匹配图片颜色数。
* 可以把gif转成png看看有没有变小。除了动画，gif一般可以转成png8。
* 运行pngcrush或其它工具压缩png。
* 运行jpegtran或其它工具压缩jpeg。

##### F-2. Optimize CSS Sprites
优化CSS雪碧图

把图片横向合并而不是纵向，横向更小。

把颜色近似的图片合并到一张雪碧图，这样可以让颜色数更少，如果低于256就可以用png8.

"Be mobile-friendly"并且合并时图片间的间距不要太大。这对图片大小影响不是太大，但客户端解压时需要的内存更少。100×100是10000个像素，1000×1000是1000000个像素。

##### F-3. Don't Scale Images in HTML

不要在html中缩放图片

不要因为你可以设置图片的宽高就去用比你需要的大得多的图片。如果你需要

```
<img width="100" height="100" src="mycat.jpg" alt="My Cat" /> 
```

那么，就用100x100px的图片，而不是500x500px的。

##### F-4. Make favicon.ico Small and Cacheable
`favicon.ico`小且缓存

`favicon.ico`是在你服务器根路径的图片。邪恶的是即使你不关心它，浏览器仍然会请求它。所以最好不要响应404。另外由于在同一服务器，每次请求favicon.ico时也会带上cookie。这个图片还会影响下载顺序，比如在IE，如果你在onload时下载额外的组件，fcvicon会在这些组件之前被下载。

怎么减轻favicon.ico的缺点？

* 小，最好1K以下
* 设置Expires头部。也许可以安全地设置为几个月。

#### G. Mobile
##### G-1. Keep Components under 25K
保持组件小于25K

这个限制与iPhone不缓存大于25K的组件相关。注意，这是非压缩（uncompressed）的文件大小。在这里minification（压缩，不要与compress混淆）很重要，因为gzip无法满足（iPhone）。

##### G-2. Pack Components into a Multipart Document
打包组件到一个多部父文档

打包组件到一个多部父文档类似于带附件的邮件。它帮助你在一个http请求中获取多个组件，但注意，iPhone不支持。





## 问答2 - URL加载

### 题目
一个页面从输入URL到加载显示，完成了什么？
### 分析
该题目是一道经典的前端面试题。参考如下资料
* [What really happens when you navigate to a URL](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)
* [What really happens when you navigate to a URL | 中文-简书](http://www.jianshu.com/p/29936050b6a1)
* [What really happens when you navigate to a URL | 中文翻译](http://blog.csdn.net/holybin/article/details/39179141)
* [SegmentFault - 1](https://segmentfault.com/a/1190000006879700)
* [SegmentFault - 2](https://segmentfault.com/q/1010000000489803)

### 求解
总体来说，分为以下几个过程。
* 第1步：在浏览器中输入URL地址。
* 第2步：浏览器查找域名的IP地址，由DNS解析完成。
* 第3步：浏览器向Web服务器发送一个HTTP请求。
* 第4步：服务器的永久重定向响应
* 第5步：浏览器跟踪重定向地址
* 第6步：服务器处理请求
* 第7步：服务器返回一个 HTTP 响应
* 第8步：浏览器显示 HTML
* 第9步：浏览器发送请求获取嵌入在 HTML 中的资源（如图片，音频，视频，CSS，JS等）
* 第10步：浏览器发送异步请求

下面对每个步骤进行具体说明。

#### 第1步：在浏览器中输入URL地址。

You enter a URL into the browser. It all starts here

以输入`facebook.com`为例，进行说明。

![url-onload-1.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-1.png)



#### 第2步：浏览器查找域名的IP地址，由DNS解析完成。

![url-onload-2.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-2.png)

The first step in the navigation is to figure out the IP address for the visited domain. The DNS lookup proceeds as follows
导航的第一步是找出待访问的域名的IP地址。 DNS查找过程如下

* **Browser cache** – The browser caches DNS records for some time. Interestingly, the OS does not tell the browser the time-to-live for each DNS record, and so the browser caches them for a fixed duration (varies between browsers, 2 – 30 minutes).
* **浏览器缓存** - 浏览器可以缓存DNS记录并持续一段时间。 有趣的是，操作系统并没有告诉浏览器每个DNS记录的缓存时间，因此浏览器会将其缓存一段固定的时间（大约在2~30分钟之间）。
* **OS cache** – If the browser cache does not contain the desired record, the browser makes a system call (gethostbyname in Windows). The OS has its own cache.
* **操作系统缓存** - 如果浏览器缓存不包含所需的记录，浏览器会请求操作系统，要操作系统从本地的host文件夹中查找。
* **Router cache** – The request continues on to your router, which typically has its own DNS cache.
* **路由器缓存** - DNS查找继续到路由器中进行查询。路由器通常具有自己的DNS缓存。
* **ISP DNS cache** – The next place checked is the cache ISP’s DNS server. With a cache, naturally.
* **ISP DNS缓存** - DNS检查的下一个位置是缓存ISP（Internet Service Provider，互联网服务供应商）的DNS服务器。
* **Recursive search** – Your ISP’s DNS server begins a recursive search, from the root nameserver, through the .com top-level nameserver, to Facebook’s nameserver. Normally, the DNS server will have names of the .com nameservers in cache, and so a hit to the root nameserver will not be necessary.
* **递归搜索** - 您的ISP的DNS服务器开始递归搜索，从根域名服务器开始，再到`.com`顶级域名服务器，再到Facebook的服务器。 通常，DNS服务器缓存中有`.com`域名服务器，因此，对根域名服务器的查找是不必要的。

Here is a diagram of what a recursive DNS search looks like
以下是DNS递归搜索的示意图。

![url-onload-3.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-3.png)

One worrying thing about DNS is that the entire domain like `wikipedia.org` or `facebook.com` seems to map to a single IP address. Fortunately, there are ways of mitigating the bottleneck
关于DNS，有一个令人担忧的事情是，像`wikipedia.org`或`facebook.com`这样的整个域名将只映射到一个IP地址。 幸运的是，有办法来减轻瓶颈

* **Round-robin DNS** is a solution where the DNS lookup returns multiple IP addresses, rather than just one. For example, facebook.com actually maps to four IP addresses.
* **循环DNS **是一个可以让DNS查找返回多个IP地址而不是仅一个的解决方案。 例如，facebook.com实际上映射到四个IP地址。
* **Load-balancer** is the piece of hardware that listens on a particular IP address and forwards the requests to other servers. Major sites will typically use expensive high-performance load balancers.
* **负载均衡器**是用于监听特定IP地址的硬件，并将请求转发到其他服务器。 主要站点通常将使用昂贵的高性能负载平衡器。
* **Geographic DNS** improves scalability by mapping a domain name to different IP addresses, depending on the client’s geographic location. This is great for hosting static content so that different servers don’t have to update shared state.
* **地理DNS **将根据客户的地理位置，通过将域名映射到不同的IP地址，来提高可扩展性。 这非常适合托管静态内容，不同的服务器不必去更新共享的状态。
* **Anycast** is a routing technique where a single IP address maps to multiple physical servers. Unfortunately, anycast does not fit well with TCP and is rarely used in that scenario.
* **Anycast**是一种路由技术，其中单个IP地址映射到多个物理服务器。 不幸的是，anycast不适合TCP，在这种情况下很少使用。

> Most of the DNS servers themselves use anycast to achieve high availability and low latency of the **DNS lookups**.
> 
> 大多数DNS服务器本身都使用anycast来实现** DNS查找**的高可用性和低延迟。

#### 第3步：浏览器向Web服务器发送一个HTTP请求。
The browser sends a HTTP request to the web server.

![url-onload-4.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-4.png)

You can be pretty sure that Facebook’s homepage will not be served from the browser cache because dynamic pages expire either very quickly or immediately (expiry date set to past).
您可以非常确定Facebook的主页将不会从浏览器缓存中提供，因为动态页面会过期或者很快就立即过期（过期日期被设置在过去）。

So, the browser will send this request to the Facebook server。
所以，浏览器会发送这个请求到Facebook服务器。

```
GET http://facebook.com/ HTTP/1.1
Accept: application/x-ms-application, image/jpeg, application/xaml+xml, [...]
User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; [...]
Accept-Encoding: gzip, deflate
Connection: Keep-Alive
Host: facebook.com
Cookie: datr=1265876274-[...]; locale=en_US; lsd=WW[...]; c_user=2101[...]
```

The GET request names the **URL** to fetch: “http://facebook.com/”. The browser identifies itself (**User-Agent** header), and states what types of responses it will accept (**Accept** and **Accept-Encoding** headers). The **Connection** header asks the server to keep the TCP connection open for further requests.
GET 请求命令URL提取地址`http://facebook.com/`。 浏览器标识自身（**用户代理**头），并说明它将接受什么类型的响应（**接受**和**接受编码**头）。 这个**连接**头要求服务器保持TCP连接打开以便接受进一步的请求。


The request also contains the **cookies** that the browser has for this domain. As you probably already know, cookies are key-value pairs that track the state of a web site in between different page requests. And so the cookies store the name of the logged-in user, a secret number that was assigned to the user by the server, some of user’s settings, etc. The cookies will be stored in a text file on the client, and sent to the server with every request.
该请求还包含浏览器对此域名的** cookies **。 您可能已经知道，Cookie是跟踪不同页面请求之间的网站状态的键值对。 因此，Cookie存储登录用户的名称，由服务器分配给用户的密码，用户的某些设置等。Cookie将存储在客户端上的文本文件中，并伴随着每个请求发送到服务器中。

There is a variety of tools that let you view the raw HTTP requests and corresponding responses. My favorite tool for viewing the raw HTTP traffic is `fiddler`, but there are many other tools (e.g., FireBug) These tools are a great help when optimizing a site.
有各种工具可让您查看原始的HTTP请求和相应的响应。 我最喜欢的查看原始HTTP流量的工具是`fiddler`，但还有其他许多工具（例如FireBug）。这些工具在优化站点时非常有帮助。

In addition to GET requests, another type of requests that you may be familiar with is a POST request, typically used to submit forms. A GET request sends its parameters via the URL (e.g.: `http://robozzle.com/puzzle.aspx?id=85`). A POST request sends its parameters in the request body, just under the headers.
除了GET请求之外，您可能熟悉的其他类型的请求是POST请求，通常用于提交表单。 GET请求通过URL发送其参数（例如：`http：//robozzle.com/puzzle.aspx？id = 85`）。 POST请求在请求体中发生其参数，参数位置位于位于头部下方。

The trailing slash in the URL “http://facebook.com/” is important. In this case, the browser can safely add the slash. For URLs of the form http://example.com/folderOrFile, the browser cannot automatically add a slash, because it is not clear whether folderOrFile is a folder or a file. In such cases, the browser will visit the URL without the slash, and the server will respond with a redirect, resulting in an unnecessary roundtrip.
网址`http://facebook.com/`中的尾部斜杠很重要。 在这种情况下，浏览器可以安全地添加斜杠。 但是对于http://example.com/folderOrFile表单的网址，浏览器无法自动添加斜杠，因为folderOrFile是否是文件夹或文件不清楚。 在这种情况下，浏览器将不用斜线访问该URL，并且服务器将以重定向进行响应，导致不必要的往返。



需要指明的是，HTTP协议是使用TCP作为其传输层协议的。

#### 第4步：服务器的永久重定向响应
The facebook server responds with a permanent redirect

![url-onload-4.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-4.png)

This is the response that the Facebook server sent back to the browser request
这是Facebook服务器发送回浏览器请求的响应

```
HTTP/1.1 301 Moved Permanently
Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
      pre-check=0
Expires: Sat, 01 Jan 2000 00:00:00 GMT
Location: http://www.facebook.com/
P3P: CP="DSP LAW"
Pragma: no-cache
Set-Cookie: made_write_conn=deleted; expires=Thu, 12-Feb-2009 05:09:50 GMT;
      path=/; domain=.facebook.com; httponly
Content-Type: text/html; charset=utf-8
X-Cnection: close
Date: Fri, 12 Feb 2010 05:09:51 GMT
Content-Length: 0
```

The server responded with a 301 Moved Permanently response to tell the browser to go to `http://www.facebook.com/` instead of `http://facebook.com/`.

服务器返回了表示永久移动的301状态码，告诉浏览器转到`http://www.facebook.com /`而不是`http://facebook.com /`。

There are interesting reasons why the server insists on the redirect instead of immediately responding with the web page that the user wants to see.
关于为什么服务器坚持重定向，而不是立即响应用户想要查看的网页，有一些有趣的原因。

One reason has to do with search engine rankings. See, if there are two URLs for the same page, say http://www.igoro.com/ and http://igoro.com/, search engine may consider them to be two different sites, each with fewer incoming links and thus a lower ranking. Search engines understand permanent redirects (301), and will combine the incoming links from both sources into a single ranking.
一个原因与搜索引擎排名有关。 例如，如果同一页面有两个URL，`http://www.igoro.com/`和`http://igoro.com/`，搜索引擎可能会认为它们是两个不同的网站，每个网站的入站链路较少， 因此排名较低。 搜索引擎理解永久重定向（301），并将来自两个来源的传入链接组合成单个排名。

Also, multiple URLs for the same content are not cache-friendly. When a piece of content has multiple names, it will potentially appear multiple times in caches.
此外，同一内容的多个URL网址不是缓存友好的。 当一段内容有多个名称时，它可能会在缓存中多次出现。

#### 第5步：浏览器跟踪重定向地址

![url-onload-6.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-6.png)

The browser now knows that `http://www.facebook.com/` is the correct URL to go to, and so it sends out another GET request
浏览器现在知道`http://www.facebook.com /`是正确的URL，所以它发出另一个GET请求

```
GET http://www.facebook.com/ HTTP/1.1
Accept: application/x-ms-application, image/jpeg, application/xaml+xml, [...]
Accept-Language: en-US
User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; [...]
Accept-Encoding: gzip, deflate
Connection: Keep-Alive
Cookie: lsd=XW[...]; c_user=21[...]; x-referer=[...]
Host: www.facebook.com
```

The meaning of the headers is the same as for the first request.
标题的含义与第一个GET请求相同。

#### 第6步：服务器处理请求
The server "handles" the request

![url-onload-7.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-7.png)

The server will receive the GET request, process it, and send back a response.
服务器将收到GET请求，处理它并发送回应。

This may seem like a straightforward task, but in fact there is a lot of interesting stuff that happens here – even on a simple site like my blog, let alone on a massively scalable site like facebook.
这似乎是一个简单的任务，但事实上，这里发生了很多有趣的事情 - 即使像我的博客这样的简单网站，更不用说像Facebook这样大规模扩展的网站了。


* **Web server software**
The web server software (e.g., IIS or Apache) receives the HTTP request and decides which request handler should be executed to handle this request. A request handler is a program (in ASP.NET, PHP, Ruby, …) that reads the request and generates the HTML for the response.
In the simplest case, the request handlers can be stored in a file hierarchy whose structure mirrors the URL structure, and so for example http://example.com/folder1/page1.aspx URL will map to file /httpdocs/folder1/page1.aspx. The web server software can also be configured so that URLs are manually mapped to request handlers, and so the public URL of page1.aspx could be http://example.com/folder1/page1.
* **Web服务器软件**
Web服务器软件（例如，IIS或Apache）接收HTTP请求，并决定执行哪个请求处理程序来处理此请求。 请求处理程序是读取请求并生成响应的HTML的程序（在ASP.NET，PHP，Ruby，...中）。在最简单的情况下，请求处理程序可以存储在可以反映URL结构的文件层次结构中，例如`http://example.com/folder1/page1.aspx` URL将映射到文件`/httpdocs/folder1/ page1.aspx`。 也可以配置Web服务器软件，以便将URL手动映射到请求处理程序，因此`page1.aspx`的公共URL可以是`http://example.com/folder1/page1`。

* **Request handler**
The request handler reads the request, its parameters, and cookies. It will read and possibly update some data stored on the server. Then, the request handler will generate a HTML response.
* **请求处理程序**
请求处理程序读取请求，其参数和Cookie。 它将读取并可能更新存储在服务器上的一些数据。 然后，请求处理程序将生成一个HTML响应。

One interesting difficulty that every dynamic website faces is how to store data. Smaller sites will often have a single SQL database to store their data, but sites that store a large amount of data and/or have many visitors have to find a way to split the database across multiple machines. Solutions include sharding (splitting up a table across multiple databases based on the primary key), replication, and usage of simplified databases with weakened consistency semantics.
每个动态网站面临的一个有趣的难题是如何存储数据。 较小的站点通常会有一个SQL数据库来存储数据，但存储大量数据和/或有许多访问者的站点必须找到一种在多台计算机上分割数据库的方法。 解决方案包括分片（基于主键分割多个数据库的表），复制以及使用弱化一致性语义的简化数据库。

One technique to keep data updates cheap is to defer some of the work to a batch job. For example, Facebook has to update the newsfeed in a timely fashion, but the data backing the “People you may know” feature may only need to be updated nightly (my guess, I don’t actually know how they implement this feature). Batch job updates result in staleness of some less important data, but can make data updates much faster and simpler.
保持数据更新低成本的一种技术是将一些工作推迟到批处理作业。 例如，Facebook必须及时更新新闻源，但支持“您可能认识的人”功能的数据可能只需要每晚更新（我的猜测，我实际上并不知道如何实现此功能）。 批量作业更新导致一些不太重要的数据的过时，但可以使数据更新更快更简单。

#### 第7步：服务器返回一个 HTTP 响应
The server sends back a HTML response

![url-onload-8.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-8.png)

Here is the response that the server generated and sent back
下面是服务器产生并发送回来的响应

```
HTTP/1.1 200 OK
Cache-Control: private, no-store, no-cache, must-revalidate, post-check=0,
    pre-check=0
Expires: Sat, 01 Jan 2000 00:00:00 GMT
P3P: CP="DSP LAW"
Pragma: no-cache
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
X-Cnection: close
Transfer-Encoding: chunked
Date: Fri, 12 Feb 2010 09:05:55 GMT
```

The **Content-Encoding** header tells the browser that the response body is compressed using the gzip algorithm. After decompressing the blob, you’ll see the HTML you’d expect
**内容编码**标头告诉浏览器使用gzip算法压缩响应正文。 解压缩Blob后，您将看到您期望的HTML

```vbscript-html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"   
      "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" 
      lang="en" id="facebook" class=" no_js">
<head>
<meta http-equiv="Content-type" content="text/html; charset=utf-8" />
<meta http-equiv="Content-language" content="en" />
...

```

In addition to compression, headers specify whether and how to cache the page, any cookies to set (none in this response), privacy information, etc.
除了压缩，标题指还定是否以及如何缓存页面，设置Cookie（此响应中无），隐私信息等。

Notice the header that sets **Content-Type** to **text/html**. The header instructs the browser to render the response content as HTML, instead of say downloading it as a file. The browser will use the header to decide how to interpret the response, but will consider other factors as well, such as the extension of the URL.
注意，头部将Content-Type设置为text / html。 标题指示浏览器将响应内容呈现为HTML，而不是将其作为文件下载。 浏览器将使用标题来决定如何解释响应，但也会考虑其他因素，例如URL的扩展。

#### 第8步：浏览器显示 HTML
The browser begins rendering the HTML

Even before the browser has received the entire HTML document, it begins rendering the website。
![url-onload-9.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-9.png)


####  第9步：浏览器发送请求获取嵌入在 HTML 中的资源（如图片，音频，视频，CSS，JS等）
The browser sends requests for objects embedded in HTML.

![url-onload-10.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-10.png)

As the browser renders the HTML, it will notice tags that require fetching of other URLs. The browser will send a GET request to retrieve each of these files.
当浏览器呈现HTML时，它会注意到需要提取其他URL的标签。 浏览器将发送GET请求以检索这些文件。

Here are a few URLs that my visit to facebook.com retrieved
以下是我访问facebook.com的几个URL
* Images

```
http://static.ak.fbcdn.net/rsrc.php/z12E0/hash/8q2anwu7.gif
http://static.ak.fbcdn.net/rsrc.php/zBS5C/hash/7hwy7at6.gif
…
```

* CSS style sheets

```
http://static.ak.fbcdn.net/rsrc.php/z448Z/hash/2plh8s4n.css
http://static.ak.fbcdn.net/rsrc.php/zANE1/hash/cvtutcee.css
…
```

* JavaScript files

```
http://static.ak.fbcdn.net/rsrc.php/zEMOA/hash/c8yzb6ub.js
http://static.ak.fbcdn.net/rsrc.php/z6R9L/hash/cq2lgbs8.js
…
```

Each of these URLs will go through process a similar to what the HTML page went through. So, the browser will look up the domain name in DNS, send a request to the URL, follow redirects, etc.
这些URL中的每一个都将经历和全面所述的HTML页面类似的过程。 因此，浏览器将在DNS中查找域名，向URL发送请求，重定向等。

However, static files – unlike dynamic pages – allow the browser to cache them. Some of the files may be served up from cache, without contacting the server at all. The browser knows how long to cache a particular file because the response that returned the file contained an Expires header. Additionally, each response may also contain an ETag header that works like a version number – if the browser sees an ETag for a version of the file it already has, it can stop the transfer immediately.
但是，静态文件（与动态页面不同）允许浏览器缓存它们。 某些文件可能会从缓存中提供，而不必联系服务器。 浏览器知道缓存特定文件需要多长时间，因为返回文件的响应包含一个Expires头。 此外，每个响应还可以包含一个类似于版本号的ETag标头 - 如果浏览器看到已经具有的文件版本的ETag，则可以立即停止传输。

Can you guess what “fbcdn.net” in the URLs stands for? A safe bet is that it means “Facebook content delivery network”. Facebook uses a content delivery network (CDN) to distribute static content – images, style sheets, and JavaScript files. So, the files will be copied to many machines across the globe.
你能猜出这个URL中的“fbcdn.net”是什么意思吗？ 值得一提的是，这意味着“Facebook内容分发网络”。 Facebook使用内容分发网络（CDN）分发静态内容 - 图像，样式表和JavaScript文件。 因此，这些文件将被复制到全球许多机器。

Static content often represents the bulk of the bandwidth of a site, and can be easily replicated across a CDN. Often, sites will use a third-party CDN provider, instead of operating a CND themselves. For example, Facebook’s static files are hosted by Akamai, the largest CDN provider.
静态内容通常代表站点的大部分带宽，并且可以轻松地通过CDN进行复制。 通常，网站将使用第三方CDN提供商，而不是自己运行CND。 例如，Facebook的静态文件由最大的CDN提供商Akamai主办。

As a demonstration, when you try to ping static.ak.fbcdn.net, you will get a response from an akamai.net server. Also, interestingly, if you ping the URL a couple of times, may get responses from different servers, which demonstrates the load-balancing that happens behind the scenes.
作为演示，当您尝试`ping static.ak.fbcdn.net`时，您将收到akamai.net服务器的响应。 此外，有趣的是，如果您ping了URL几次，可能会收到来自不同服务器的响应，这表明后台发生的负载均衡。

#### 第10步：浏览器发送异步请求
The browser sends further asynchronous (AJAX) requests

![url-onload-11.png](http://ol3kbaay9.bkt.clouddn.com/url-onload-11.png)

In the spirit of Web 2.0, the client continues to communicate with the server even after the page is rendered.
在Web 2.0下，即使在页面呈现后，客户端也会继续与服务器通信。

For example, Facebook chat will continue to update the list of your logged in friends as they come and go. To update the list of your logged-in friends, the JavaScript executing in your browser has to send an asynchronous request to the server. The asynchronous request is a programmatically constructed GET or POST request that goes to a special URL. In the Facebook example, the client sends a POST request to http://www.facebook.com/ajax/chat/buddy_list.php to fetch the list of your friends who are online.
例如，Facebook聊天将继续更新您登录的朋友的名单当你的朋友上线时。 要更新您登录的朋友的列表，浏览器中执行的JavaScript必须向服务器发送异步请求。 异步请求是一种编程式构造的GET或POST请求，转到特殊URL。 在Facebook示例中，客户端向`http://www.facebook.com/ajax/chat/buddy_list.php`发送POST请求，以获取在线的朋友的列表。

This pattern is sometimes referred to as “AJAX”, which stands for “Asynchronous JavaScript And XML”, even though there is no particular reason why the server has to format the response as XML. For example, Facebook returns snippets of JavaScript code in response to asynchronous requests.
这种模式有时被称为“AJAX”，它代表“异步JavaScript和XML”，服务器必须将响应格式化为XML。 例如，Facebook返回JavaScript代码片段以响应异步请求。

Among other things, the fiddler tool lets you view the asynchronous requests sent by your browser. In fact, not only you can observe the requests passively, but you can also modify and resend them. The fact that it is this easy to “spoof” AJAX requests causes a lot of grief to developers of online games with scoreboards. (Obviously, please don’t cheat that way.)
除此之外，提示工具可让您查看浏览器发送的异步请求。 事实上，你不仅可以被动地观察请求，还可以修改并重新发送。 事实上，这是一个容易“欺骗”AJAX请求的事实，给开发人员带来了记分牌的网络游戏。 （显然，请不要以这种方式作弊。）

Facebook chat provides an example of an interesting problem with AJAX: pushing data from server to client. Since HTTP is a request-response protocol, the chat server cannot push new messages to the client. Instead, the client has to poll the server every few seconds to see if any new messages arrived.
Facebook聊天提供了一个AJAX有趣的问题的例子。将数据从服务器推送到客户端。 由于HTTP是请求 - 响应协议，因此聊天服务器无法将新消息推送到客户端。 相反，客户端必须每隔几秒轮询服务器，看看是否有任何新消息到达。

Long polling is an interesting technique to decrease the load on the server in these types of scenarios. If the server does not have any new messages when polled, it simply does not send a response back. And, if a message for this client is received within the timeout period, the server will find the outstanding request and return the message with the response.
长时间轮询是减少这些类型场景中服务器负载的有趣技术。 如果服务器在轮询时没有任何新消息，则根本不会发回响应。 而且，如果在超时期限内收到了一个此客户端的消息，服务器将发现未完成的请求并返回消息。

## 编程题1 - 数的分解

### 题目
对整数有如下分解。

1的分解方式为0种。

2的分解方式为0种。

3= 2+1。
3的分解方式为1种。

4= 3+1 = 2+1+1。
4的分解方式为2种。

5= 4+1 = 3+2 = 3+1+1 = 2+2+1 = 2+1+1+1。
5的分解方式为5种。

6= 5+1 = 4+2 = 4+1+1 = 3+2+1 = 3+1+1+1 = 2+2+1+1 = 2+1+1+1+1。
7的分解方式为7种。

* 输入描述
输入一个整数num。1 <= num <= 90。
* 输出描述
输出其分解的种类数

* 测试样例

```
//input
4
//output
2
//input
6
//output
7
```

### 分析

参考[博客园](http://www.cnblogs.com/xuehaoyue/p/6660315.html?utm_source=itdadao&utm_medium=referral)了解更多。

本题采用**动态规划DP**求解。

分析本题示例可知
* 分解的因子不能全部一样。例如，2=1+1不符合分解的要求，不在计数范围内。
* 如果直接认为一个数num分解的两个因子不一样才能计数，则会计算错误，遗漏了部分情况。例如，4=2+2不符合分解的要求。但是在5=4+1=2+2+1中是符合分解要求的。


针对上述分析情况，可以采用根据每次分解的尾数进行分类计算。每次分解保证因数序列是降序的。

![tencent-factor.PNG](http://ol3kbaay9.bkt.clouddn.com/tencent-factor.PNG)

参考上图，下面介绍使用动态规划求解的思路。

创建一个二维数组，横坐标为数字num，纵坐标为结尾数字。结尾数字为1,2,3直到`floor((num-1)/2)`。例如，`datas[3][1]`表示数字3的以1结尾的分解总数。
动态规划的转移方程为

```
num = factor1 + i

//若factor1为偶数
datas[num][i] = 1 + 1+ (datas[factor1][i] + datas[factor1][i+1] + ... + datas[factor1][col])

//若factor1为奇数
datas[num][i] = 1 + (datas[factor1][i] + datas[factor1][i+1] + ... + datas[factor1][col])
```

* 对于数字num，按照其分解的尾数为`1,2,3 ... floor((num-1)/2)`进行考虑。
* 每种结尾情况都先进性一次初始分解（即转移方程中的加1项）。以7为例，以1结尾时分解成6+1，以2结尾分解成5+2，以3结尾分解成4+3。
* 对于第1次分解出的两个数factor1和factor2，进一步分解factor1。注意只在`factor1 > 2*factor2`的情况下才分解factor1。因为必须保证分解序列是降序的。例如，5=3+2，则不能对3继续分解，否则分解后5=2+1+2，不能保证降序。
* 若factor1是偶数，计算分解情况时+1。例如，5=4+1，进一步分解4时，要考虑4=2+2的情况。
* 参考转移方程，可知，在动态规划时，要保证factor1进一步分解的结尾的数 >= factor2，否则无法保证序列降序。例如，7=5+2，进一步分解5时，只能将5分解成3+2，若分解成任意以1结尾的情况，如4+1，则7=4+1+2不是降序。

### 求解

```
#include <iostream>
#include <vector>
using namespace std;

int main() {
	int num;
	int col;
	int factor1, factor2;
	int res;
	while (cin >> num) {
		if (num == 1 || num == 2) {
			cout << 0 << endl;
			continue;
		}
		vector<vector<int>> datas(num + 1);
		//初始化
		datas[0] = { 0 };
		datas[1] = { 0 };
		datas[2] = { 0 };  //此时默认num>=2,否则访问data[2]会越界访问
		//datas.push_back({ 0 });  //也可以使用push_back 防止越界访问

		//动态规划DP
		for (int i = 3; i <= num; i++) {
			//分解后尾数的种类
			col = floor((i - 1) / 2);
			datas[i].resize(col+1, 0);
			for (int x = 1; x <= col; x++) {
				//初次分解
				datas[i][x] = 1;    //以数字x结尾的分解
				factor1 = i - x;
				factor2 = x;
				//是否继续分解factor1
				if (factor1 > 2 * factor2) {
					if (factor1 % 2 == 0) {
						//如果为偶数，则分解种类加1
						datas[i][x]++;
					}
					//factor1的继续分解
					//保证factor1继续分解的尾数大于等于factor2
					for (int y = x; y < datas[factor1].size(); y++) {
						datas[i][x] += datas[factor1][y];
					}
				}
			}
		}
		//结果输出
		res = 0;
		for (int temp = 0; temp < datas[num].size(); temp++) {
			res += datas[num][temp];
		}
		cout << res << endl;
	}
	system("pause");
	return 0;
}
```

## 编程题2 - 元素出现的次数
### 题目
给定一个数组，计算其中每个元素出现的次数。要求效率最高。

* 测试样例

```
//input
1 2 2 3

//output
1(1)
2(2)
3(1)
```

### 分析
使用`unordered_map`容器实现算法的高效性。

* `unordered_map`中使用`mapName[index]`访问该元素时，若该元素不存在，会自动添加该元素。
* `vector<template T>`中使用`vectorName[index]`访问该元素时，若该元素不存在（越界访问），编译器不会自动添加该元素，而会抛出错误。应该使用`push_back`添加元素。


另外需要注意的是，本程序中，使用`unordered_map`数据结构，程序中只创建了索引值为num的元素。例如，当输入`1 2 2 3 8`时，`unordered_map`数据结构`dataCounts`存储的结果为

```
// dataCount.size() = 4
dataCount[1] = 1
dataCount[2] = 2
dataCount[3] = 1
dataCount[8] = 1
```

因此，输出结果的方案有2种

* 方案1：使用`pair`数据结构
`unordered_map<int, int> dataCounts;`数据结构中每个元素都是一个`pair`，使用`pair.first`和`pair.second`来获取num值和出现的次数。注意`pair.first`和`pair.second`中没有括号，不要写成了`pair.first()`和`pair.second()`。

```cpp
for (auto key : dataCounts) {
	cout << key.first <<"("<<key.second<<")"<< endl;
	//key 为 pair结构  <int, int>
}
```

* 方案2：使用for循环。
若使用for循环，其终止条件不是`i<size`。而是`i<m_max`。`m_max`是输入数据中的最大值。

```cpp
for (int i = 0; i <= m_max; i++) { 
	//m_max是输入数据中的最大值 并且i可以取等
	if (dataCounts[i] == 0) {
		continue;
	}
	else {
		cout << i << "(" << dataCounts[i] << ")" << endl;
	}
}
```

### 求解

```cpp
#include <iostream>
#include <unordered_map>
#include <climits>
#include <algorithm>
using namespace std;




int main() {
	int temp;
	unordered_map<int, int> dataCounts;
	int m_max = INT_MIN;
	while (cin >> temp) {
		m_max = max(m_max, temp);
		dataCounts[temp]++;

	}
	

	for (auto key : dataCounts) {
		cout << key.first <<"("<<key.second<<")"<< endl;
		//key 为 pair结构  <int, int>
	}

	//或者
	/*
	for (int i = 0; i <= m_max; i++) { //可以取等
		if (dataCounts[i] == 0) {
			continue;
		}
		else {
			cout << i << "(" << dataCounts[i] << ")" << endl;
		}
	}
	
	*/
	
	system("pause");
	return 0;
}
```

## 编程题3 - 接受字符串
### 题目
编写代码，接收从屏幕输入的长度为16字节整数倍的字符串，回车后，按示例格式排版输出。

* 示例

屏幕输入（均为可见字符）

abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijkl

屏幕输出(斜体部分是格式说明，不要在结果里输出)

按照“16进制偏移   16进制表示（每一行16个字节）  原文”的格式输出。每个元素之间用双空格隔开。

![tencent-0403-1.jpg](http://ol3kbaay9.bkt.clouddn.com/tencent-0403-1.jpg)

### 分析
题目比较简单，直接处理即可。需要处理好输出的格式。

使用`unordered_map`容器建立小写字母和其ASCII值的映射。需要注意的是，map容器不支持`unordered_map<char,int>`类型，可以使用`unordered_map<int,string>` 或者 `unordered_map<string,string>`。此处使用`unordered_map<int,string>`。将字母进行`str[i]- 'a'`处理，转化为int值。


下面考虑16进制偏移量如何获取。将数字转换为16进制还不够，还需要满足输出8位二进制的格式。

可以初始化一个8位的字符串`"00000000"`。再根据num值的16进制情况更新字符串即可。

* 易错点

C++中的`substr(pos,length)`中的第1个元素是子字符串的起始坐标，第2个参数是子字符串的长度。



> C++
> `string substr (size_t pos = 0, size_t len = npos) const;`
> **Generate substring**
> 
> Returns a newly constructed string object with its value initialized to a **copy** of a substring of this object.
> 
> The substring is the portion of the object that starts at character position pos and spans **len** characters (or until the end of the string, whichever comes first).
>  --- [CPlusPlus.com](http://www.cplusplus.com/reference/string/string/substr/)
>  

JavaScript中的`substring(start,[end])`中的第1个参数是子字符串的起始坐标，第2个参数是子字符串的终点坐标加1。即子字符串的起止范围是`[start,end)`，不包括end坐标对应的元素。并且第2个参数可以省略。此外，函数`substring`的`s`不大写。

> JavaScript
> `str.substring(indexStart[, indexEnd])`
> 
> substring() extracts characters from indexStart up to but not including indexEnd. In particular.
>   --- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)


### 求解

```
#include <iostream>
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;


//字母-ascii 映射表
unordered_map<int, string> alphabets = {
	{ 0,"61" },{ 1,"62" },{ 2,"63" },{ 3,"64" },{ 4,"65" },
	{ 5,"66" },{ 6,"67" },{ 7,"68" },{ 8,"69" },{ 9,"6a" },
	{ 10,"6b" },{ 11,"6c" },{ 12,"6d" },{ 13,"6e" },{ 14,"6f" },
	{ 15,"70" },{ 16,"71" },{ 17,"72" },{ 18,"73" },{ 19,"74" },
	{ 20,"75" },{ 21,"76" },{ 22,"77" },{ 23,"78" },{ 24,"79" },
	{ 25,"7a" }
};


//10进制转换为16进制对应的字符串  长度为8  
string toHex(int num) {
	string res = "00000000";
	int index = 7;
	int temp;
	while(num && index >=0){
		temp = num % 16;
		if (temp < 10) {
			res[index] = temp + '0';
		}
		else if(temp == 10) {
			res[index] = 'a';
		}
		else if (temp == 11) {
			res[index] = 'b';
		}
		else if (temp == 12) {
			res[index] = 'c';
		}
		else if (temp == 13) {
			res[index] = 'd';
		}
		else if (temp == 14) {
			res[index] = 'e';
		}
		else if (temp == 15) {
			res[index] = 'f';
		}
		num = num / 16;
		index--;
	}
	
	return res;
}

int main() {
	string str;
	int length;
	int temp;
	int mul;
	//str = "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijkl";  //测试数据
	while (cin >> str) {
		length = str.size();
		for(int i = 0; i < length; i++) {
			if (i == 0) {
				cout << toHex(i) << "  ";
			}
			else if (i % 16 == 0) {  //换行
				mul = i / 16;
				cout << str.substr((mul - 1) * 16, 16) << endl;
				cout << toHex(i) << "  ";
			}
			temp = (int)(str[i] - 'a');
			cout << alphabets[temp] << "  ";
		}
		cout << str.substr(length-16,16) << endl;  
		//输出最后一组原文 勿忘记 ！！
	}

	//system("pause");
	return 0;
}
```


## 编程题4 - 二叉排序树 & LCA
### 题目
对于一棵满二叉排序树深度为K，节点数为2^K-1，节点值为 1至(2^k-1)。给出K和任意三个节点的值，输出包含该三个节点的最小子树的根节点值。

* 测试样例

```
//input
4 10 15 13

//output
12
```

### 分析

二叉排序树（Binary Sort Tree）也称为二叉搜索树（Binary Search Tree），是指一棵空树或者具备下列性质的二叉树(每个结点都不能有多于两个儿子的树)

* 若任意结点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
* 若任意结点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
* 任意结点的左、右子树也分别为二叉查找树；
* 没有键值相等的结点。

从其性质可知，定义排序二叉树的一种自然的方式是递归的方法，其算法的核心为递归过程，由于它的平均深度为O(logN)，所以递归的操作树，一般不必担心栈空间被耗尽。

更多内容参考 [BS_Tree](https://github.com/xuelangZF/CS_Offer/blob/master/DataStructure/BS_Tree.md)。

如下图所示的**满**二叉排序树，若树深度为K，节点数为2^K-1，节点值为 1至(2^k-1)。用`[left, right]`表示节点值的范围。

例如，初始时，left=1，right=2^K-1。则根节点的值为mid = (left+right)/2。

则根节点的左子树的节点值范围为`[left,mid-1]`，则左节点的值为`mid = (left+mid-1)/2`。依次类推。

根节点的右子树的节点值范围为`[mid+1, right]`，则右节点的值为`mid = (mid+1+right)/2`。依次类推。


题目中的测试样例如下图所示。`10 15 13`节点的最近公共祖先（LCA）是12。

![tencent-bst-1.PNG](http://ol3kbaay9.bkt.clouddn.com/tencent-bst-1.jpg)

因此，可以将该问题转换为求**最近公共祖先LCA**问题。


**最近公共祖先LCA(Lowest Common Ancestor)**参考资料如下
* [LCA - Github](https://github.com/julycoding/The-Art-Of-Programming-By-July/blob/master/ebook/zh/03.03.md)
* [最近公共祖先 | 百度百科](http://baike.baidu.com/link?url=L78S1pxtDnHJzCGyJLtN5YvjJacF-AiqA4NY_jrhX-k8xndJ5ug4pesJ3SmsnofKutEnSIvkKx2qoisU4570L5GGP9g7pvWRQqEwS3mhLZ1sRjhCq0IOq5KsHuCjEcTSAuzS3Toe5SoUZOzYyoa5y_)
* [CSDN](http://blog.csdn.net/nlsqq/article/details/51325961)


求解思路如下。

* 思路1：二分搜索求LCA（暴力求解LCA）
从根节点开始二分搜索。若当前节点值是三个值中间的一个或者三个值不在当前节点的同一侧子树中，说明当前节点就是LCA，直接返回即可。否则，遍历当前节点的子树。若三个值中的最小值小于当前节点值，则遍历其左子树。若三个值中的最小值大于当前节点值，则遍历其右子树。参考[二分搜索求LCA](http://www.bubuko.com/infodetail-2007512.html)了解更多。

 

### 求解

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int getLCA(vector<int> arr, int left, int right) {
	int mid = left + (right - left) / 2;

	//当前节点坐标为三个节点之一 或者 三个节点不在当前节点同一侧的子树
	//则当前节点就是LCA
	if( (arr[0] - mid)*(arr[1] - mid) <= 0 ||
		(arr[0] - mid)*(arr[2] - mid) <= 0 ||
		(arr[1] - mid)*(arr[2] - mid) <= 0 ){
		return mid;
	}
	else {
		//arr[0] 为三个节点值中最小的
		if (arr[0] < mid) {
			//搜索左子树
			return getLCA(arr, left, mid - 1);
		}
		else {
			//搜索右子树
			return getLCA(arr, mid + 1, right);
		}
	}
}

int main() {
	int depth;
	int size = 3;
	vector<int> arr(size);
	while (cin >> depth) {
		for (int i = 0; i < size; i++) {
			cin >> arr[i];
		}
		sort(arr.begin(),arr.end()); //保证arr升序排列，arr[0]为最小值
		cout << getLCA(arr,1,(2<<(depth-1))-1) << endl;
	}
	system("pause");
	return 0;
}
```



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 