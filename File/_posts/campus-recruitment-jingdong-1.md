---
title: 京东2017前端实习生笔试
date: 2017-04-12 11:35:48
categories: 校招笔试编程题
tags: [校招笔试编程题,京东笔试]
keywords: 校招笔试编程题,京东笔试
---




* 本文对京东2017前端实习生笔试题目进行记录和归纳。

<!--more-->

## 选择题1 - 插入排序
### 题目
设有一组初始关键字序列为`(30,20,10,25,15,28)`，则第4趟直接插入排序结束后的结果的是（）

A. 10,15,20,25,30,28
B. 10,15,20,25,28,30
C. 10,20,25,30,15,28
D. 10,20,30,25,15,28 

### 求解
正确答案是A。

**插入排序中第1个元素默认已经被排序，for循环从数组第2个元素开始。**

## 选择题2 - 指令流水线
某指令流水线由5段组成，各段所需要的时间依次如下： t、3t、t、2t、t ， 如果连续执行10条指令，则吞吐率为?

A. 0.1428/t
B. 0.2041/t
C. 0.2857/t
D. 0.3333/t

### 解答
正确选项是C。

**由于是流水线，所以执行各条指令的每个段是不能重叠的。**

所以，第二条指令要从3t时刻开始执行，并且第2条指令的第1段执行完，要等到2t时间等第1条指令的第2阶段执行完才能继续执行。分析可以发现，第2条指令比第1条指令晚结束3t时间。

以此类推，下一条指令比上一条指令晚结束3t时间。综上，有如下计算式

![order-computer-1.gif](http://ojx8u3g1z.bkt.clouddn.com/order-computer-1.gif)

对于上述题目，有如下变形
*  假设一个四段流水线，取指段的时间为t，译码段的时间为t，取数段的时间为3t，执行段的时间为t。流水线示意图如下

![order-computer-2.jpg](http://ojx8u3g1z.bkt.clouddn.com/order-computer-2.jpg)

* 为了便于计算，假设取指和译码段总是连续执行的。流水线执行n条指令时其执行过程的时空图如下图所示

![order-computer-3.jpg](http://ojx8u3g1z.bkt.clouddn.com/order-computer-3.jpg)

 * 从图中不难看出，第一条指令的执行时间是6t；第二条指令在执行时停顿了两个周期，第二条指令的完成时间比第一条指令的完成时间晚3t；第三条、第四条......第n条与此相同。
 * 因此，该流水线执行n条指令的时间就是`6t+3*(n-1)*t`。流水线的实际吞吐率就是`n/6t+(n-1)3t`。


> 流水线时间 = 一条指令所需时间+（指令条数-1）* Δt（前一条指令和后一条指令执行结束时间的差值）
> 
> 吞吐率TP = 指令条数 / 流水线时间

### 指令流水线（补充）
参考资料
* [指令流水线的吞吐率](http://blog.csdn.net/wangjian8855/article/details/12711009)
* [百度百科](https://wenku.baidu.com/view/d6141cfe227916888586d7b0.html?from=search)
* [Google图书](https://books.google.co.jp/books?id=hyVW6BNNtwQC&pg=PA292&lpg=PA292&dq=%E6%8C%87%E4%BB%A4%E6%B5%81%E6%B0%B4+%E5%90%9E%E5%90%90%E7%8E%87&source=bl&ots=qVyyvf_LAt&sig=CWqTdqou66zqZ2nG2-fy2vogaaU&hl=zh-CN&sa=X&ved=0ahUKEwiTpPqPi5rTAhVFqJQKHSDIDNAQ6AEILTAE#v=onepage&q=%E6%8C%87%E4%BB%A4%E6%B5%81%E6%B0%B4%20%E5%90%9E%E5%90%90%E7%8E%87&f=false)

#### 流水线概念
计算机的速度可以用每秒执行的指令条数来表示。

**指令的解释**速度越快，则计算机计算速度也越快。为了加快指令的解释过程，可以
* 选用更高速的器件
* 减少解释过程所需拍数
* 使解释过程的各个动作并行执行

**指令的解释可以有3种控制方式：顺序，重叠，流水。**

顺序方式中，指令逐条执行。一条指令执行完毕之后，才取下条指令来执行。指令内的个条微指令也是顺序串执行的。

重叠方式中，在解释第k条指令的操作完成之前，就可以开始解释第k+1条指令。

流水方式中，将一个重复的时序过程分解成多个子过程Subprocess。每个子过程都可以有效地在其专用功能段上与**其他**子过程同时执行。

**注意，是能够和其他子过程同时执行。不同指令的相同子过程，不能同时执行。流水线中各条指令的每个段是不能重叠的。**

#### 流水线的技术指标
流水线的主要技术指标有吞吐率，加速比，效率等。

##### 吞吐率 Throughput Rate
单位时间内流水线所能处理的任务数（或指令数）。
##### 最大吞吐率
当流水线在连续流动达到稳定状态后的吞吐率。
##### 加速比
m段流水线的速度与等效的非流水线的速度之比。

![order-computer-4.jpg](http://ojx8u3g1z.bkt.clouddn.com/order-computer-4.jpg)


![order-computer-5.jpg](http://ojx8u3g1z.bkt.clouddn.com/order-computer-5.jpg)

##### 效率
流水线上的设备利用率就是效率。

![order-computer-6.jpg](http://ojx8u3g1z.bkt.clouddn.com/order-computer-6.jpg)

![order-computer-8.JPG](http://ojx8u3g1z.bkt.clouddn.com/order-computer-8.JPG)


## 选择题3 - CSS中link和import区别

下面有关CSS中link和@import的区别，描述错误的是？

A. link属于XHTML标签，而@import完全是CSS提供的一种方式
B.  当一个页面被加载的时候，link引用的CSS会同时被加载，而@import引用的CSS会等到页面全部被下载完再被加载
C. link在支持CSS的浏览器上都支持而@import只在5.0以上的版本有效
D. 当使用javascript控制dom去改变样式的时候，只能使用@import方式

### 解答
* D

下面，比较下使用link标签引用CSS和使用@import引用CSS的区别。
* 差别1：归属差别 
	- `link`是一个html/xhtml的一个标签，而`@import`是css的一个标签。
	- `link`标签除了加载CSS外，还可以用来声明页面链接属性，声明目录，定义RSS等。
	- `@import`只能加载CSS。
* 差别2：加载顺序的差别
	- 当一个页面被加载的时候，link引用的CSS会同时被加载
	- @import引用的CSS会等到页面全部被下载完再被加载。
	- 有时候浏览@import加载CSS的页面时，开始会没有样式（就是闪烁），网速慢的时候比较明显。
* 差别3：兼容性的差别 
	- 由于@import是CSS2.1提出的，所以老版本的浏览器不支持，@import只有在IE5以上的浏览器才能识别。
	- link标签无此问题。 
* 差别4：使用DOM控制样式时的差别 
	- **当使用JavaScript控制DOM去改变样式的时候，只能使用link标签**
	- **@import不是DOM可以控制的。 **
* 差别5：@import可以在CSS中再次引入其他样式表 
	- 例如，可以创建一个主样式表，在主样式表中使用`@import`引入其他的样式表。

参考资料： [外部引用CSS：link与@import的区别](http://blog.sina.com.cn/s/blog_51048da70102v4bn.html)
 

## 选择题4 JS对象属性调用
调用一个对象的属性，下列方式不正确的是（）

A. obj["arr"]
B. obj.arr
C. obj["a"+"r"+"r"]
D. obj.arr{"arr"}

### 求解
正确答案是D。

## 选择题5 append VS appendTo
将一个`li`插入`ul`之中，使用append和appendTo实现。

### 求解

```javascript
$("ul").append("<li>Item4</li>");
$("<li>Item4</li>").appendTo("ul");
```

### append VS appendTo
* after

The `after()` method inserts specified content after the selected elements.
To insert content before selected elements, use the `before()` method.

Syntax: `$(selector).after(content,function(index))`   ---- [W3 School](https://www.w3schools.com/jquery/html_after.asp)

`after()`方法在被选元素后插入指定的内容并且**插入的元素在被选元素之外。**
例如，`$("span").after("<a href="#">agter</a>")`之后，a标签是在span标签之外的。

```vbscript-html
<span>selector</span>
<a href="#">after</a>
```

* append

The `append()` method inserts specified content at the end of the selected elements.
To insert content at the beginning of the selected elements, use the `prepend()` method.    --- [W3 School](https://www.w3schools.com/jquery/html_append.asp)

Synatx: `$(selector).append(content,function(index,html))`

`append()`方法在被选元素的结尾插入指定内容并且**插入的元素在被选元素的内部。**


例如，`$("span").append("<a href="#">agter</a>")`之后，a标签是在span标签内的。

```vbscript-html
<span>
	selector
	<a href="#">after</a>
</span>

```
* appendTo

The `appendTo()` method inserts HTML elements at the end of the selected elements.
To insert HTML elements at the beginning of the selected elements, use the `prependTo()` method.      --- [W3 School](https://www.w3schools.com/jquery/html_appendto.asp)

**Syntax: `$(content).appendTo(selector)`**

`appendTo()`方法在被选元素的结尾插入指定内容并且插入元素在被选元素内部。

若要实现上述append的效果，则语句为

```javascript
$("<a href="#">agter</a>").appendTo("span");
```

* 总结

```javascript
$("A").after("B");      //把B插入A之后，并且插入A的外部
$("A").append("B");     //把B插入A之后，并且插入A的内部
$("A").appendTo("B");   //把A插入B之后，并且插入B的内部
```

## 选择题6 - CSS伪类选择器
下列不是CSS伪类选择器的是（）

A. : focus
B. : active
C. : lang
D. : length

### 求解

正确答案是D。

参考资料
* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference#选择器)
* [神通广大的CSS选择器](http://www.cnblogs.com/skylar/p/css3-selector.html#s3)


CSS伪类选择器（Pseudo-classes selectors）是加在选择器后面的用来指定元素状态的关键字。比如，`:hover` 会在鼠标悬停在选中元素上时应用相应的样式。

伪类选择器包括
* :link
* :visited
* :active
* :hover
* :focus
* :first-child
* :nth-child
* :nth-last-child
* :nth-of-type
* :first-of-type
* :last-of-type
* :empty
* :target
* :checked
* :enabled
* :disabled
* :lang

`:lang`是语言伪类选择器，用来指定lang属性的元素，其值为language。

```vbscript-html
<html lang="en-US"></html>
<style type="text/css">
	:lang(en-US) {
		/* do something */
	}
</style>
```
当网页切换不同的语言版本的时候，可以通过这个选择器做一些特殊的处理。

* :not

`:not`是否定伪类选择器，可以用来选取所有除了指定元素外的其他所有元素。

```css
input:not([type=submit]){
	/* do something*/
}
```
上述程序选择了除了类型为submit的input外的所有input元素。


CSS伪元素在CSS3中规定要使用双冒号书写，如`::first-letter`，主要用来区分伪类和伪元素。对于IE6-8，仅支持单冒号表示方法，但是其他现代浏览器中两种表示方法均可，使用单冒号和双冒号均可。

常用的伪元素选择器包括
*  ::first-letter
*  ::first-line
*  ::before
*  ::after
*  ::backdrop
*  ::selection
*  ::placeholder
*  ::marker
*  ::spelling-error
*  ::grammar-error



## 选择题7
getElementByName返回的是什么？

### 求解
`getElementsByName()` 方法可返回带有指定名称的对象的**集合**。

因为一个文档中的`name` 属性可能不唯一，所以 `getElementsByName()` 方法返回的是元素的数组，而不是一个元素。


## 选择题8 - 图的出度和入度

有n个顶点的有向图中，所有顶点出度之和为s，则所有顶点的入度和为 

### 求解
所有顶点的入度之和也是s。

**有向连通无环图中，边数 = 顶点数-1 = 所有顶点的出度之和 = 所有顶点的入度之和**


## 选择题9 - 树的度数
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

## 选择题10 - 散列函数
已知一个线性表`(38，25，74，63，52，48)`，假定采用散列函数`h(key)=key%7`计算散列地址，并散列存储在散列表`A[0..6]`中。若采用线性探测方法解决冲突，则在该散列表上进行等概率成功查找的平均查找长度为（）

A. 1.5
B. 1.7
C. 2.0
D. 2.3


### 散列查找冲突解决办法
参考资料
* [CSDN - 例题](http://blog.csdn.net/yankai0219/article/details/8185847)
* [CSDN - 冲突解决办法](http://blog.csdn.net/xtzmm1215/article/details/47177701)
* 《大话数据结构》

#### 开放定址法
一旦产生冲突（该地址有其他元素），就按某种规则去寻找另一空地址。只要散列表足够大，空的散列地址总能找到，并将记录记入。

若发生了第`i`次冲突，试探的下一个地址将增加 `di`，基本公式是`hi(key) = (h(key)) + di)modTableSize ( 1<= i < TableSize )`

`di` 决定了不同的解决冲突方案
* 线性探测:  di＝i
若遇到冲突，则寻找下一个地址，步长值为1。若遇到散列表尾部，则将散列表当成循环链表进行查找。
* 平方探测（二次探测）:  di＝ +1^2，-1^2，+2^2，-2^2 …… +q^2, -q^2  (q<= m/2)
双向寻找可能存放的位置。增加平方运算的目的是为了不让关键字都聚集在某一块区域。
* 随机探测：位移量di采用随机函数计算得到。
注意此处采用伪随机数。即设置相同的随机种子，那么不断调用随机函数可以生成不会重复的数列。

#### 再散列函数法
每当发生冲突是，就换一个散列函数计算。

#### 链地址法（拉链法）
将所有关键字为同义词的记录存储在一个单链表中，并将这种表称为同义词子表。在散列表中只存储所有同义词子表的头指针。由此，可以解决冲突问题。无论多少冲突，都可以通过在当前位置给单链表增加结点来解决。

例如，一组关键字（32，40，36，53，16，46，71，27，42，24，49，64），哈希表长度为13，哈希函数为`H(key)= key % 13`，则用链地址法处理冲突的结果如下图所示。

![hash-look-2.JPG](http://ol3kbaay9.bkt.clouddn.com/hash-look-2.JPG)

平均查找长度 ASL= (1*7+2*4+3*1)=1.5


与开放定址法相比，拉链法有如下几个**优点**
* 拉链法处理冲突简单，且无堆积现象，即非同义词决不会发生冲突，因此平均查找长度较短
* 由于拉链法中各链表上的结点空间是动态申请的，故它更适合于造表前无法确定表长的情况
* 开放定址法为减少冲突，要求装填因子`α`较小，故当结点规模较大时会浪费很多空间。而拉链法中可取`α≥1`，且结点较大时，拉链法中增加的指针域可忽略不计，因此节省空间
* 在用拉链法构造的散列表中，删除结点的操作易于实现。只要简单地删去链表上相应的结点即可。而对开放地址法构造的散列表，删除结点不能简单地将被删结点的空间置为空，否则将截断在它之后填人散列表的同义词结点的查找路径。这是因为各种开放地址法中，空地址单元(即开放地址)都是查找失败的条件。因此在用开放地址法处理冲突的散列表上执行删除操作，只能在被删结点上做删除标记，而不能真正删除结点。

拉链法的**缺点**
* 指针需要额外的空间，故当结点规模较小时，开放定址法较为节省空间。


#### 平均查找长度ASL
* **查找成功的平均查找长度ASL = SUM(查找次数) / SUM(元素个数)**

* 查找失败的平均查找长度ASL  
计算长度为m的哈希表中装填有n个记录的查找不成功时的平均查找长度，**相当于计算在这张表中填入第n+1个记录时所需要的比较次数的期望值。**

若散列函数为`H(key)= key % Num`，则第n+1个记录值为0, 1, 2 .... Num-1。计算插入1，插入2 ...  插入Num-1时候比较次数的总和，最后求其均值即可。

参见下述例题了解更多。

### 求解
真确答案是C（2.0）。



![hash-look-1.JPG](http://ol3kbaay9.bkt.clouddn.com/hash-look-1.JPG)

38%7 = 3，则将38存放于A[3]中，查找次数为1次。

25%7 = 4，则将25存放于A[4]中，查找次数为1次。

74%7 = 4，则将74存放于A[4]中，但是由于A[4]已经有元素了，则向后再查找1次，A[5]未存储元素，则将74存放于A[5]中。查找次数为2次。

63%7 = 0，则将63存放于A[0]中，查找次数为1次。

52%7 = 3，则将52存放于A[3]中，但是由于A[3]已经有元素了，则向后再查找1次，A[4]也已经被占用，继续向后查找1次，A[5]也已经被占用，继续向后查找1次，A[6]未存储元素，则将52存放于A[6]中。查找次数为4次。

48%7 = 6，则将48存放于A[6]中，但是由于A[6]已经有元素了，则向后（转至数组开头）再查找1次，A[0]也已经被占用，继续向后查找1次，A[1]未存储元素，则将48存放于A[1]中。查找次数为3次。

综上，在该散列表上进行等概率成功查找的平均查找长度为`(1+3+1+1+2+4)/6=2.0`。

再进一步的，计算查找不成功的平均查找长度。计算长度为m的哈希表中装填有n个记录的查找不成功时的平均查找长度，**相当于计算在这张表中填入第n+1个记录时所需要的比较次数的期望值。**

此处计算插入第7个元素的平均查找长度。插入的第7个元素可能值为0,1,2,3,4,5,6。

插入0时，比较次数为3次；
插入1时，比较次数为2次；
插入2时，比较次数为1次；
插入3时，比较次数为7次；
插入4时，比较次数为6次；
插入5时，比较次数为5次；
插入6时，比较次数为4次；

因此，查找不成功的平均查找长度为(3+2+1+7+6+5+4)/7 = 4.0。



### 扩展
对于本题，如果采用**拉链法**解决冲突，则在该散列表上进行等概率成功查找的平均查找长度为多少？

![hash-look-3.JPG](http://ol3kbaay9.bkt.clouddn.com/hash-look-3.JPG)

ASL = (1+1+2+1+2+1)/6 = 1.33



## 选择题11 模块的扇入和扇出
某系统结构图如下图所示。该系统结构图的最大扇入数是（ ）。

![fan-in-fan-out-1.JPG](http://ol3kbaay9.bkt.clouddn.com/fan-in-fan-out-1.JPG)

A. 3
B. 4
C. 2
D. 1



### 分析
参考[Google图书](https://books.google.co.jp/books?id=qE3Fej3IXu4C&pg=PA197&lpg=PA197&dq=%E6%9C%80%E5%A4%A7%E6%89%87%E5%85%A5%E6%95%B0+%E6%9C%80%E5%A4%A7%E6%89%87%E5%87%BA%E6%95%B0&source=bl&ots=ZFl7LS7-bE&sig=66HLt88uq0oLlPNDRLEHPxWZoBE&hl=zh-CN&sa=X&ved=0ahUKEwiH7MSt3Z3TAhWDjLwKHQIcBZMQ6AEIKjAB#v=onepage&q&f=false)了解更多。

![fan-in-fan-out.JPG](http://ol3kbaay9.bkt.clouddn.com/fan-in-fan-out.JPG)

### 求解
正确答案是A(3)。扇入是指调用一个给定模块的模块个数。图中所示功能 n.1 被功能 1 、功能 2 和功能 3 三个模块调用，则最大扇入数为 3 。故本题答案为 A 选项。

## 选择题12 系统深度与宽度

参考[Google图书](https://books.google.co.jp/books?id=qE3Fej3IXu4C&pg=PA197&lpg=PA197&dq=%E6%9C%80%E5%A4%A7%E6%89%87%E5%85%A5%E6%95%B0+%E6%9C%80%E5%A4%A7%E6%89%87%E5%87%BA%E6%95%B0&source=bl&ots=ZFl7LS7-bE&sig=66HLt88uq0oLlPNDRLEHPxWZoBE&hl=zh-CN&sa=X&ved=0ahUKEwiH7MSt3Z3TAhWDjLwKHQIcBZMQ6AEIKjAB#v=onepage&q&f=false)了解更多。

![sys-depth.JPG](http://ol3kbaay9.bkt.clouddn.com/sys-depth.JPG)
 



## 选择题13 软件体系结构
参考资料
* [百度百科](http://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84)
* [Google图书](https://books.google.co.jp/books?id=3J0bGHQdRIwC&pg=PA95&lpg=PA95&dq=%E8%BD%AF%E4%BB%B6%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E7%9A%84%E6%9E%84%E6%88%90+%E6%A8%A1%E5%BC%8F+%E7%BA%A6%E6%9D%9F+%E6%9E%84%E4%BB%B6+%E8%BF%9E%E6%8E%A5%E4%BB%B6&source=bl&ots=rkSPEPPK9t&sig=aWzmVp1ZpJtFlpb7xBuA-uRbu2k&hl=zh-CN&sa=X&ved=0ahUKEwiuntS94p3TAhUIErwKHQOKBEUQ6AEINjAC#v=onepage&q=%E8%BD%AF%E4%BB%B6%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E7%9A%84%E6%9E%84%E6%88%90%20%E6%A8%A1%E5%BC%8F%20%E7%BA%A6%E6%9D%9F%20%E6%9E%84%E4%BB%B6%20%E8%BF%9E%E6%8E%A5%E4%BB%B6&f=false)
* [软件体系结构 | Blog](http://www.liuchuo.net/archives/2653)

### 软件工程
软件工程是用工程、科学和数学的原则与方法研制、维护计算机软件的有关技术及管理方法。

**软件工程包括三个要素：方法、工具和过程**。

### 构件与软件重用
软件重用是指在两次或多次不同的软件开发过程中重复使用相同或相近软件元素的过程。

软件元素包括程序代码、测试用例、设计文档、设计过程、需求分析文档甚至领域知识。这些可重用的元素称做软构件，简称**构件**。

由于构件大都经过严格的质量认证，并在实际运行环境中得到检验，因此，重用构件有助于改善软件质量

### 软件体系结构建模概述
可以将软件体系结构的模型分为5种
* 结构模型
* 框架模型
* 动态模型
* 过程模型
* 功能模型

最常用的是结构模型和动态模型。

### 软件体系结构的核心模型

**软件体系结构的核心模型由5种元素组成**
* **构件**
* **连接件**
* **配置**
* **端口**
* **角色**

**其中构件、连接件和配置是最基本的元素。**

构件是具有某种功能的可重用的软件模块单元，表示系统中主要的计算元素和数据存储。构件有两种：复合构件和原子构件。

连接件表示构件之间的交互，简单的连接件如：管道、过程调用、事件广播等，更为复杂的交互如：客户-服务器通信协议，数据库和应用之间的SQL链接等。

配置表示构件和连接件的拓扑逻辑和约束
软件体系结构的构成。

![software-sys.JPG](http://ol3kbaay9.bkt.clouddn.com/software-sys.JPG)

## 选择题14 - 微指令地址
为确定下一条微指令的地址，通常采用断定方式，其基本思想是（ C ）。

A. 用程序计数器PC来产生后继微指令地址
B. 用微程序计数器µPC来产生后继微指令地址
C. 通过微指令顺序控制字段由设计者指定或由设计者指定的判别字段控制产生后继微指令地址
D. 通过指令中指定一个专门字段来控制产生后继微指令地址

### 求解
正确答案是C。


## 选择题15 - 数据库
* WITH ENCRYPTION： 进行加密
* WITH GRANT OPTION：权限传递。使用这个子句时将允许用户将其权限分配给他人。







## 选择题16 - 文法2
文法G：S->xSx|y所识别的语言是()
A. (xyx)*
B. xyx
C. x*yx*
D. (x^n)*y*(x^n)(n>=0)

### 求解
正确答案是D。

上述文法规则为
1. S-> xSx
2. S-> y

则按照上述映射规则，可以最终得到：y, xyx, (x^2)y(x^2), (x^3)y(x^3), (x^4)y(x^4) ...  

### 分析
参考资料
* [形式文法 | 维基百科](https://zh.wikipedia.org/wiki/%E5%BD%A2%E5%BC%8F%E6%96%87%E6%B3%95)
* [文法 - 自顶向下分析](http://ol3kbaay9.bkt.clouddn.com/%E6%96%87%E6%B3%95%20-%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90%E6%96%B9%E6%B3%95.ppt)

#### 形式文法
在计算机科学中，**形式语言**是某个字母表上，一些有限长字串的集合，而**形式文法**是描述这个集合的一种方法。形式文法之所以这样命名，是因为它与人类自然语言中的**文法**相似的缘故。

形式文法描述形式语言的**基本想法**是：从一个特殊的初始符号出发，不断的应用一些产生式规则，从而生成出一个字串的集合。产生式规则指定了某些符号组合如何被另外一些符号组合替换。举例来说，假设字母表只包含`a`和`b`两个字符，初始符号是`S`，我们应用下述规则
1. S -> aSb
2. S -> ba

于是我们可以通过把`S`重写为`aSb`（规则1），我们还可以继续应用这条规则把`aSb`重写为`aaSbb`。这个重写的过程不断重复，直到结果中只包含字母表中的字母为止。在例子中，我们可以得到`S -> aSb -> aaSbb -> aababb`这样的结果。由文法刻画的语言，包含了所有可以这样产生的字串，比如`ba, abab, aababb, aaababbb`等等。

#### 形式定义
一个形式文法G是下述元素构成的一个四元组（N, Σ, P, S）
* “非终结符号”集合N
* “终结符号”集合Σ，Σ与N无交。
* 取如下形式的一组“产生式规则” P，
(Σ ∪ N)*中的字符串→ (Σ ∪ N)* 中的字符串（这里的*是克莱尼星号），并且产生式左侧的字符串中必须至少包括一个非终结符号
* “起始符号”S，S属于N。

一个由形式文法G = (N, Σ, P, S)产生的语言是所有如下形式的字符串集合，这些字符串全部由“终结符号”集Σ中符号构成，并且可以从“初始符号”S出发，不断应用P中的“产生式规则”而得到。


## 选择题17 - 页式虚拟存储管
参考资料
* [页式管理](http://ettc.sysu.edu.cn/2005wlkc/caozuoxitong/book/chapter6/lesson4/lesson4.htm)
* [存储管理 | 课件](ftp://ftp.cs.sjtu.edu.cn:990/xue-gt/%B2%D9%D7%F7%CF%B5%CD%B3/%B2%D9%D7%F7%CF%B5%CD%B3%A3%AD%B8%B4%CF%B04.pdf) 
* [分段，分页与段页式存储管理](http://blog.csdn.net/zephyr_be_brave/article/details/8944967)


### 虚拟存储器
* 基本思想
操作系统把程序当前使用的那些部分保留在存储器中，而把其他部分保存在磁盘上。

* 例如
一个16M的程序，通过仔细地选择在各个时刻将哪4M内容保留在内存中，并在需要时在内存和磁盘间交换程序的片段，那么就可以在一个4M的机器上运行该程序。

### 虚拟存储管理

* 分页技术

虚地址空间被划分成称为页面（pages）的单位，在物理存储器中对应的单位称为页框（page frames），**页和页框总是同样大小的**。

由程序产生的地址被称为虚地址（virtual addresses），它们构成了一个虚地址空间（virtual address space）。

* 页表
* 关联存储器TLB
* 反置页表

### 页式管理
通过引入进程的逻辑地址，把进程地址空间与实际存储位置分离，从而增加存储管理的灵活性。

#### 页式管理的基本原理
将各进程的虚拟空间划分成若干个长度相等的页(page)，页式管理把内存空间按页的大小划分成片或者页面（page frame），然后把页式虚拟地址与内存地址建立一一对应页表，并用相应的硬件地址变换机构，来解决离散地址变换问题。页式管理采用请求调页或预调页技术实现了内外存存储器的统一管理。

**优点**
* 没有外碎片，每个内碎片不超过页大小。
* 一个程序不必连续存放。便于改变程序占用空间的大小（主要指随着程序运行而动态生成的数据增多，要求地址空间相应增长，通常由系统调用完成而不是操作系统自动完成）。

缺点
* 程序全部装入内存。

## 编程题1 - 拍卖
### 题目
公司最近新研发了一种产品，共生产了n件。有m个客户想购买此产品，第i个客户出价Vi元。为了确保公平，公司决定要以一个固定的价格出售产品。每一个出价不低于要价的客户将会以公司决定的价格购买一件产品，余下的将会被拒绝购买。请你找出能让公司利润最大化的售价。如果有多种定价方案可以最大化总收入，输出最小的定价。

* 输入
输入第一行二个整数n(1<=n<=1000),m(1<=m<=1000)，分别表示产品数和客户数。
接下来第二行m个整数Vi(1<=Vi<=1000000)，分别表示第i个客户的出价。
* 输出
输出一行一个整数，代表能够让公司利润最大化的最小售价。


* 样例输入
5 4
2 8 10 7
* 样例输出
7


### 分析

该题比较简单，直接处理即可。

### 求解


```cpp
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
using namespace std;

typedef long long ll;

int main() {
	int n, m;
	ll maxprofit;
	int total;
	int index;
	while (cin >> n >> m) {
		total = 0;
		vector<ll> prices(m);
		for (int i = 0; i < m; i++) {
			cin >> prices[i];
		}
		sort(prices.begin(), prices.end());
		total = min(n, m);
		int j;
		//产品数小于客户数
		if (n < m) {
			j = m - n;
		}
		else {
			j = 0;
		}
		//初次计算
		index = j;
		maxprofit = prices[j] * total;
		for (j = j + 1; j < m && total >= 0; j++) {
			total--;
			if (maxprofit < (prices[j])*(total)) {
					maxprofit = (prices[j])*(total);
					index = j;
				}
		}
		cout << prices[index] << endl;
	}
	return 0;
}
```

## 编程题2 - 石头分堆
### 题目
小明得到了n个石头，他想把这些石头分成若干堆，每堆至少有一个石头。他把这些石堆排在一条直线上，他希望任意相邻两堆的石头数都不一样。小明最后的得分为石头数大于等于k的石堆数，问他最多能得多少分。

严格地，小明把n个石头分成了m堆，每堆个数依次为a1，a2，.....，am。要求满足：

1、ai≥1（1≤i≤m）
2、ai≠ai+1（1≤i＜m）
3、a1+a2+...+am＝n

小明想知道a1，a2.....，am中大于等于k的数最多能有多少个？


* 输入
输入两个数n, k。（1≤k≤n≤109）
* 输出
输出最大的得分

* 样例输入
5 1
* 样例输出
3

* Hint

```
输入样例2
4 2
输出样例2
1
输入样例3
2 1
输出样例3
1

第一个样例中，一种分堆方法为：1 3 1
第二个样例中，一种分堆方法为：1 2 1
第三个样例中，只能分成一堆，这堆有2个石头
```

### 分析
从石头分堆的两边开始考虑。若要计数，则分解的石头个数应该大于等于k。

先在两边放置两个个数为k的石头堆，然后将问题规模n缩小为n-2k。下面再按照(k+1)放置两堆石头，再将问题规模缩小为n-2k-2(k+1)。

为了保证石头堆的数目最大，下面再按照k放置两堆石头。

如此循环下去，直到结束。

### 求解
 
```cpp
#include <iostream>
using namespace std;

int main() {
	int n, k;
	bool flag; 
	int total;
	while (cin >> n >> k) {
		flag = true;
		total = 0;
		if (n < k) {
			cout << 0 <<endl;
			continue;
		}
		if (n <= 2 * k) {
			cout << 1 << endl;
			continue;
		}
		while (n > 2 * k) {
			total += 2;
			n = n - 2 * k;
			if (flag == true) {
				k = k + 1;
				flag = false;
			}
			else {
				k = k - 1;
				flag = true;
			}
		}
        if (n >= k) {
			total += 1;
		}
		//total += 1;
		cout << total << endl;		
	}
	return 0;
}
```


## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 