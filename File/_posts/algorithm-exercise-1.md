---
title: 算法习题记录 - 1
date: 2017-03-05 22:21:00
tags: [Algorithm,Programing]
categories: Algorithm
toc: true
---



* 本文主要记录求职笔试和面试中一些常见的算法习题。

<!--more-->



## Facebook在线笔试 - 16进制转换
### 题目
* 给出一个整型`int`的数将其转换为十六进制的表示方法，负数要用二进制补码的形式表示。

### 分析
* 思路1
	- 若该数为正数，直接转换即可。
	- 若该数为负数，则负数在计算机存储时，其补码是正数的补码取反后再加1。例如，`-2`的存储为`1110`，由`2=0010`的补码取反得`1101`，再加1得`1110`。

* 思路2
	- 由于数值在计算机中本身就是以二进制形式存储。因此，不用自己编程去计算正数和负数的16进制数，可以直接考虑将二进制转换成十六进制数。
	- 每4位二进制数转换成1位16进制数即可。转换过程中，采用位操作会提高程序运行速度，`num&15`和`num%16`的效果是相同的，但是前者运算速度更快。

> JavaScript中的`Number.prototype.toString(16)`会把数组转换为16进制数，但是其返回的结果是直接用正负号表示`num`的正负的。例如`16.toString(16) = '10'`，`(-16).toString(16) = '-10'`。

### 解答
* 方法1： 参照思路1，先求取`abs(num)`的十六进制值，然后在对正负数进行判断，若为负数则将正数的补码取反加1，求取负数的补码。

```cpp
//C++
class Solution {
public:
	string toHex(int num) {
		long n = (num>0)? num:-num;
		if (num == 0) {
			return 0;
		}
		const int length = sizeof(int) * 8/4;
		int bit[length] = {0};  //初始化为0
		string res = "0x";
		int len = 0;
		//计算正数的补码
		while (n > 0) {
			bit[len++] = n % 16;
			n /= 16;
		}
		//计算负数的补码  正数的补码取反+1
		if (num < 0) {
			for (int i = 0; i < length; i++) {
				bit[i] = 15 - bit[i];
			}
			int pos = 0;
			while ((bit[pos] == 15) && (pos < length)) {
				bit[pos] = 0;
				pos++;
			}
			bit[pos]++;
		}
		bool lead0 = true;
		for (int j = (length - 1); j >= 0; j--) {
			if (bit[j] != 0) {
				lead0 = false;
			}
			//先导0判断
			if (lead0) {
				continue;
			}
			if (bit[j] < 10) {
				res += (char)(bit[j]+'0');
			}
			else {
				res += (char)('A' + bit[j] - 10);
			}
		}
		return res;
	}
};
```

需要注意的是，将`int`转换为`char`值，若直接进行强制类型转换，则会按照ASCII值进行转换，例如，`(char)(65)='A'`。因此，可以采用`res += (char)(bit[j]+'0');`的方式进行转换。

此外，也要注意程序中先导0的判断。
 
 * 方法2：参照思路2，将二进制数转换成十六进制数。

```cpp
//C++
class Solution {
public:
	string toHex(int num) {
		if (num == 0) {
			return 0;
		}
		int length = sizeof(int) * 8 / 4;
		string res = "";
		int len = 0;
		int temp = 0;
		//bool lead0 = true;
		while ((num != 0) && (len < length)) {
			temp = num & 15;  //取出4位二进制数
			if (temp < 10) {
				res = (char)(temp + '0') + res;
			}
			else {
				res = (char)(temp -10 + 'A') + res;
			}
			num >>= 4;  //向右移位
			len++;
		}
		return ("0x"+res);
	}
};
```
需要注意的是，由于最后的`while`循环条件中有`num!=0`的判断，因此，该方法中省去了先导0的判断。
 

## 微软在线笔试 - 位运算
### 题目
* 给定2个数`l`和`r`，`0 < l <= r < 100, 000, 000`。求`l xor (l + 1) xor (l + 2) ... xor (r)`
### 分析
* 思路1（基本方法）: 采用for循环遍历。但是该算法效率低下，算法复杂度为`O(n)`。

* 思路2（优化算法-1）

记作`f(l,r)=l xor (l + 1) xor (l + 2) ... xor (r)`，由于`num xor num = 0`。若记 `F(x)= 1 xor 2 xor 3 xor ... xor x`，则有如下推导

```cmake
f(l,r)=l xor (l + 1) xor (l + 2) ... xor (r)
	  =(1 xor 2 xor ... xor (l-1)) xor (1 xor 2 xor ... xor (l-1)) xor f(l,r)
	  = (1 xor 2 xor ... xor (l-1)) xor (1 xor 2 xor ... xor (l-1) xor l xor (l+1) xor ... xor r)
	  = F(l-1) xor F(r)
```
根据上述推导，可以得到`f(l,r)= F(l-1) xor F(r)`，其中`F(x)= 1 xor 2 xor 3 xor ... xor x`。下面只需求出`F(x)`即可。

```cpp
0001      x%4=1%4=1   F(x)=1=1
0010      x%4=2%4=2   F(x)=3=x+1
0011      x%4=3%4=3   F(x)=0=0
0100      x%4=4%4=0   F(x)=4=x
0101      x%4=1%4=1   F(x)=1=1
0110      x%4=2%4=2   F(x)=3=x+1
0111      x%4=3%4=3   F(x)=0=0
1000      x%4=4%4=0   F(x)=4=x
```
从上述分析可知，F(x)的值和`x%4`有关。据此，可以构建算法，求解题目。

### 解答
* 方法1： 思路1（基本方法）的实现。

```cpp
class Solution {
public:
	int xorSum(int l,int r) {
		if (l > r) {
			return - 1;  //返回-1表示错误
		}
		int temp = 0;
		for (int i = l; i <= r; i++) {
			temp ^= i;
		}
		return temp;
	}

};
```

* 方法2： 思路2（优化算法-1）的实现。

```cpp
class Solution {
public:
	int xorSum(int l, int r) {
		if (l > r) {
			return -1;  //返回-1表示错误
		}
		int fl = 0;
		int fr = 0;
		l = l - 1;
		switch (l % 4) {
		case 1: fl = 1; break;
		case 2: fl = l + 1; break;
		case 3: fl = 0; break;
		case 0: fl = l; break;
		}
		switch (r % 4) {
		case 1: fr = 1; break;
		case 2: fr = r + 1; break;
		case 3: fr = 0; break;
		case 0: fr = r; break;
		}
		return fl ^ fr;
	}
};
```



## 2017京东校招笔试 - 进制转换 & 辗转相除
### 题目
* 时间限制：C/C++语言 1000MS； 其他语言：3000MS
* 内存限制：C/C++语言 65536KB； 其他语言：589824KB
* 尽管是一个CS专业的学生，小B的数学基础很好并对数值计算有着特别的兴趣，喜欢用计算机程序来解决数学问题。现在，她正在玩一个数值变换的游戏。她发现计算机中经常用不同的进制表示同一个数，如十进制数`123`表达为`16`进制时只包含两位数`7`、`11`（B），用八进制表示时为三位数`1`、`7`、`3`。按不同进制表达时，各个位数的和也不同，如上述例子中十六进制和八进制中各位数的和分别是`18`和`11`。
* 小B感兴趣的是，一个数`A`如果按`2`到`A-1`进制表达时，各个位数之和的平均值是多少？她希望你能帮她解决这个问题。
* 所有的计算均基于十进制进行，结果也用十进制表示为不可约简的分数形式。
* 输入：输入中有多组测试数据。每组测试数据为一个整数>A（1<=A<=5000）。
* 输出：对每组测试数据，在单独的行中以`X/Y`的形式输出结果。
* 样例输入

```cpp
5 
3
```
* 样例输出

```cpp
7/3
2/1
```

### 分析
* 编写一个函数，用于计算该数值的所有进制的各个位数之和。
* 再编写一个函数，用于计算两个数的最大公约数，可以采用辗转相除法（欧几里得算法）。
### 解答

* 方法1：C++实现

```cpp
class Solution {
public:
	//求解所有进制数的位数和
	int radixCount(int num) {
		int count = 0;
		int temp;
		
		for (int i = 2; i<num; i++) {
			temp = num;
			while (temp != 0) {
				count += temp%i;
				temp /= i;
			}
		}
		return count;
	}
	//求解两个数的最大公约数（欧几里得算法，也称为辗转相除法）
	//gcd(a,b) = gcd(b,a%b)
	int gcd(int a, int b)
	{
		if (b == 0)
			return a;
		return gcd(b, a % b);
	}
	string slove(int num) {
		string res = "";
		int count = radixCount(num);
		int myGcd = gcd(count, (num - 2));
		res += (char)((count / myGcd) + '0');
		res += "/";
		res += (char)(((num - 2) / myGcd) + '0');
		return res;
	}
};
```

* 方法2：JavaScript实现，原理同C++。只是需要注意的是JavaScript中所有的数都是浮点数，因此正数除法需要借助`parseInt()`函数。

```javascript
function radixCount(num){
	var count = 0;
	var temp;
	for(var i=2;i<num;i++){
		temp = num;
		while(temp!=0){
			count += temp%i;
			//注意使用parseInt()
			temp =parseInt(temp/i);
		}
	}
	return count;
}
function gcd(a,b){
	if(b===0){
		return a;
	}
	else{
		return gcd(b,a%b);
	}
}

function slove(num){
	var count = radixCount(num);
	var myGcd = gcd(count,num-2);
	return (count/myGcd).toString()+"/"+((num-2)/myGcd).toString();
}
```




## 华为笔试 - 16进制转换10进制

### 题目
* 写出一个程序，接受一个十六进制的数值字符串，输出该数值的十进制字符串。（多组同时输入 ）
* 输入描述
输入一个十六进制的数值字符串。
* 输出描述
输出该数值的十进制字符串。
* 例子

```cpp
0x0A  //输入

10    //输出
```

### 分析
* 思路1
该题比较简单，直接进行转换即可。计算每一位的权值，遍历每一位，其对应的权值为`pow(16,i)`。
* 思路2
原理同思路1，只是进行优化。不去计算`pow(16,i)`（耗时较多），而是从最高位求起，利用`sum=16*sum+t` 计算，提升算法性能。
* 思路3
利用`switch .. case`语句，列表给出所有的十六进制和十进制的对应关系。
 

 
### 解答
* 方法1：参考思路1，C++实现。

```cpp
//cpp
int toDec(string num) {
	int res = 0;
	for (int i = num.size() - 1, j = 0; (num[i] != 'x' && num[i] != 'X'); i--,j++) {
		int weight = (int)pow(16, j);
		if (num[i] < '10') {
			int temp = (int)(num[i] - '0');
			res += temp* weight;
		}

		else {
			if (num[i] >= 'A') { //大写字母
				res += (int)(num[i] - 'A'+ 10)* weight;
			}
			else {  //小写字母
				res += (int)(num[i] - 'a' + 10)* weight;
			}
		}
	}
	return res;
}

int main() {
	string num;
	while (cin >> num) {
		cout << toDec(num) << endl;
	}
	return 0;
}

```

* 方法2：参考思路2，C++实现。

```cpp
//cpp
int toDec(string num) {
	int res = 0;
	int temp = 0;
	int size = num.size();
	//默认输入的数字以0x或0X开头
	for (int i = 2; i < size; i++) {
		if (num[i] < '10') {
			temp = (int)(num[i] - '0');
		}
		else {
			if (num[i] >= 'A') { //大写字母
				temp = (int)(num[i] - 'A'+ 10);
			}
			else {  //小写字母
				temp = (int)(num[i] - 'a' + 10);
			}
		}
		res = res * 16 + temp;
	}
	return res;
}

int main() {
	string num;
	while (cin >> num) {
		cout << toDec(num) << endl;
	}
	return 0;
}
```

* 方法3：参考思路3，C++实现。

```cpp
//cpp

int getNum(char ch);
int toDec(string num) {
	int res = 0;
	int temp = 0;
	int size = num.size();
	for (int i = 2; i < size; i++) {
		res = res * 16 + getNum(num[i]);
	}
	return res;
}

int getNum(char ch) {
	switch (ch) {
		case '0': return 0;
		case '1': return 1;
		case '2': return 2;
		case '3': return 3;
		case '4': return 4;
		case '5': return 5;
		case '6': return 6;
		case '7': return 7;
		case '8': return 8;
		case '9': return 9;
		case 'a':
		case 'A': return 10;
		case 'b':
		case 'B': return 11;
		case 'c':
		case 'C': return 12;
		case 'd':
		case 'D': return 13;
		case 'e':
		case 'E': return 14;
		case 'f':
		case 'F': return 15;
	}
}
int main() {
	string num;
	while (cin >> num) {
		cout << toDec(num) << endl;
	}
	return 0;
}
```

## 华为笔试 - 16进制转化8进制
### 题目
* 给定`n`个十六进制正整数，输出它们对应的八进制数。
* 输入格式
	- 输入的第一行为一个正整数n （1<=n<=10）。
	- 接下来n行，每行一个由`0~9`、大写字母`A~F`组成的字符串，表示要转换的十六进制正整数，每个十六进制数长度不超过`100000`。
* 输出格式
	- 输出n行，每行为输入对应的八进制正整数。
* 注意
	- 输入的十六进制数不会有前导0，比如012A。
	- 输出的八进制数也不能有前导0。
* 样例输入

```cpp
2
39
123ABC
```

* 样例输出

```cpp
71
4435274
```


### 分析
先将十六进制数转换成二进制数，再由二进制数转换成八进制，三位合并为一位。

### 解答

```cpp
//cpp
//16进制转换2进制
string hexToBin(string num) {
	int size = num.size();
	string res = "";
	string table[16] = { 
		"0000","0001","0010","0011",
		"0100","0101","0110","0111",
		"1000","1001","1010","1011",
		"1100","1101","1110","1111"
	};
	int temp = 0;
	for (int i = 0; i < size; i++) {
		temp = 0;
		if (num[i] <= '9') {  //使用'9',不要使用'10'
			temp = (int)(num[i]-'0');
		}
		else {
			temp = (int)(num[i] - 'A'+10);
		}
		res += table[temp];
	}
	return res;
}


//2进制转换为8进制，3位合1位
string toOct(string num) {
	string res = hexToBin(num);
	string final = "";
	int temp = 0;
	int size = res.size();
	for (int i = size - 1; i >= 2; i=i-3) {
		temp = (int)(res[i] - '0') + (int)(res[i-1] - '0')*2 + (int)(res[i-2] - '0')*4;
		if (temp == 0) {
			continue;
		}
		else {
			final = (char)(temp + '0') + final;
		}
	}
	return final;
}

int main() {

	int count;
	cin >> count;
	vector<string> vec(count);
	for (int i = 0; i < count; i++) {
		cin>>vec[i];
	}
	for (int i = 0; i < count; i++) {
		cout<< toOct(vec[i])<<endl;
	}
	return 0;
}
```

## 2011华为上机笔试 - 识别字符串中整数

参考链接：[华为2011上机笔试题2+参考程序](http://www.cnblogs.com/jerry19880126/archive/2012/08/06/2625854.html)

### 题目
* 识别输入字符串中所有的整数，统计整数个数并将这些字符串形式的整数转换为数字形式整数。
* 要求实现函数
`void take_num(const char *strIn, int *n, unsigned int *outArray)`
* 输入` strIn`：   输入的字符串
* 输出
	- ` n`：计识别出来的整数个数
	- `outArray`：识别出来的整数值，其中`outArray[0]`是输入字符串中从左到右第一个整数，` outArray[1]`是第二个整数，以此类推。数组地址已经分配，可以直接使用。
* 返回：无
* 注意
	- 不考虑字符串中出现的正负号(+, -)，即所有转换结果为非负整数（包括0和正整数）
	- 不考虑转换后整数超出范围情况，即测试用例中可能出现的最大整数不会超过`unsigned int`可处理的范围
	- 需要考虑 `0` 开始的数字字符串情况，比如 `00035` ，应转换为整数`35`；
	- `000` 应转换为整数0；`00.0035` 应转换为整数0和35（忽略小数点：`mmm.nnn`当成两个数`mmm`和`nnn`来识别）
	- 输入字符串不会超过100 Bytes，请不用考虑超长字符串的情况。
* 示例 

```cpp
输入
strIn = "ab00cd+123fght456-25  3.005fgh"

输出：
n = 6
outArray = {0, 123, 456, 25, 3, 5}
```

### 分析
该题比较简单，直接使用数字的ASCII值处理即可。
 
### 解答

```cpp
//cpp
void take_num(const char *strIn) {
	*n =0;   //数字计数值
	unsigned int len = strlen(strIn);  //获取输入字符的长度
	unsigned int outArrayIndex = 0;
	bool hasNumber = false;  //标志位 temp中是否已经存在数字
	unsigned int temp = -1;

	//遍历输入的字符串
	for (unsigned int i = 0; i < len; i++) {
		//如果是数字
		if ((strIn[i] >= '0') && (strIn[i] <= '9')) {
			if (!hasNumber) {
				//间隔后第1次出现数字
				hasNumber = true;
				temp = (unsigned int)(strIn[i] - '0');
				n++;

			}
			//temp已经存有数字
			else {
				temp = temp * 10 + (unsigned int)(strIn[i] - '0');
			}
		}
		//出现的不是数字
		//设置对应的标志位，并把temp中的数值存放如数组，最后清空temp
		else {
			hasNumber = false;  //数字出现间断，标志位设为false
			if (temp != -1) {
				outArray[outArrayIndex] = temp;
				//outArray.push_back(temp);
				temp = -1;
			}
		}
	}
}
```


## 2011华为上机笔试 - IP地址识别
参考链接：[华为2011上机笔试题2+参考程序](http://www.cnblogs.com/jerry19880126/archive/2012/08/06/2625854.html)
 
### 题目
在路由器中，一般来说转发模块采用 *最大前缀匹配原则* 进行目的端口查找，具体如下。

* IP地址和子网地址匹配

IP地址和子网地址所带掩码做AND运算后，得到的值与子网地址相同，则该IP地址与该子网匹配。

比如（示例1），若IP地址为`192.168.1.100`，子网为`192.168.1.0/255.255.255.0`，其中`192.168.1.0`是子网地址，`255.255.255.0`是子网掩码。由于

```
192.168.1.100&255.255.255.0 = 192.168.1.0
```
因此， 该IP和子网`192.168.1.0`匹配。

 
比如（示例2），下述示例中则IP和子网`192.168.1.128`不匹配。

```
IP地址：192.168.1.100

子网：192.168.1.128/255.255.255.192

192.168.1.100&255.255.255.192 = 192.168.1.64
```
 
 * 最大前缀匹配

任何一个IPv4地址都可以看作一个32bit的二进制数，比如`192.168.1.100`可以表示为`11000000.10101000.00000001.01100100`， `192.168.1.0`可以表示为`11000000.10101000.00000001.00000000`。

最大前缀匹配要求IP地址同子网地址匹配的基础上，二进制位从左到右完全匹配的位数尽量多（从左到右子网地址最长）。比如，IP地址`192.168.1.100`，同时匹配子网`192.168.1.0/255.255.255.0`和子网`192.168.1.64/255.255.255.192`。但对于子网`192.168.1.64/255.255.255.192`，匹配位数达到26位，多于子网`192.168.1.0/255.255.255.0`的25位，因此`192.168.1.100`最大前缀匹配子网是`192.168.1.64/255.255.255.192`。


请编程实现上述最大前缀匹配算法。

* 要求实现函数 
`void max_prefix_match(const char *ip_addr, const char *net_addr_array[], int *n)`
* 输入
	- `ip_addr`：IP地址字符串，严格保证是合法IPv4地址形式的字符串
	- `net_addr_array`：子网地址列表，每一个字符串代表一个子网，包括子网地址和掩码。表现形式如上述，子网地址和子网掩码用`/`分开，严格保证是合法形式的字符串；如果读到空字符串，表示子网地址列表结束。

* 输出
	- `n`：最大前缀匹配子网在`*net_addr_array[]`数组中对应的下标值。如果没有匹配返回`-1`。

* 示例 

```
输入

ip_addr = "192.168.1.100"

net_addr_array[] =

{"192.168.1.128/255.255.255.192",

"192.168.1.0/255.255.255.0",

"192.168.1.64/255.255.255.192",

"0.0.0.0/0.0.0.0",

""}


输出
n = 2
```

### 分析
* Step 1：将子网和掩码的值存放在二维数组中。
* Step 2：将IP地址存放在数组中。
* Step 3：进行AND运算。
* Step 4： 统计最大匹配长度。

### 解答

```cpp
//cpp
void max_prefix_match(const char *ip_addr, const char *net_addr_array[], int *n) {
	*n = -1;
	bool matchFlag = false;  //IP地址和子网地址匹配 标志位
	//获取子网掩码，将其存放入二维数组中
	const int count = sizeof(net_addr_array) / sizeof(char);  //子网个数
	int subIP[count][4] = { 0 }; //子网数组
	int mask[count][4] = { 0 }; //掩码数组
	
	//计算子网数组和掩码数组
	const char *p;    //临时变量
	bool subIPFlag = false;   //是否计算的是子网IP
	int temp = 0;
	int weight[3] = { 1,10,100 };
	int k=0;
	int m = 3;
	for (int i = 0; i < count; i++){
		//192.168.1.0/255.255.255.0//
		subIPFlag = false;
		p = net_addr_array[i];
		int length = strlen(p);
		m = 3;
		for(int j=length-1;j>=0;j--){
			while ((p[j] != '.') && (p[j] != '/') && (j >= 0)) {
				temp += (p[j] - '0')*weight[k++];
				j--;
			}
			if ((p[j] == '.') || (p[j]=='/') ||(j==-1)) {
				//计算的是掩码
				if (!subIPFlag) {
					mask[i][m] = temp;
					m--;
				}
				//计算的是子网IP
				else {
					subIP[i][m] = temp;
					m--;

				}
				if (p[j] == '/') {
					subIPFlag = true;
					m = 3;
				}
				//清空temp和权重值计数k
				temp = 0;
				k = 0;
				
			}

		}		
	}

	//IP地址转换为数组
	int ipAddress[4] = { 0 };
	m = 3;
	int ipLength = strlen(ip_addr);
	temp = 0;
	k = 0;
	for (int j = ipLength - 1; j >= 0; j--) {
		while ((ip_addr[j] != '.')  && (j >= 0)) {
			temp += (ip_addr[j] - '0')*weight[k++];
			j--;
		}
		if ((ip_addr[j] == '.') || (j==-1)) {
			ipAddress[m] = temp;
			m--;
			temp = 0;
			k = 0;
		}
	}
	
	bool m_matchFlag = true;
	
	//IP地址和子网地址所带掩码做AND运算
	int calculate = 0;
	int m_max = 0;
	for (int i = 0; i < count; i++) {
		m_matchFlag = true;
		calculate = 0;
		for (int j = 0; j < 4; j++) {
			mask[i][j] &= ipAddress[j];
			if (mask[i][j] != subIP[i][j]) {
				m_matchFlag = false;
				break;  //不相等，直接退出循环
			}
		}
		//匹配成功
		if (m_matchFlag == true) {
			//计算IP和子网的匹配数量，保存最大值
		
			
			for (int k = 0; k < 4; k++) {
				if (ipAddress[k] == subIP[i][k]) {
					calculate += 8;
				}
				else {
					for (int x = 7; x >= 0; x -- ) {
						int bit = 1 << x;
						if ((ipAddress[k] & bit) == (subIP[i][k] & bit)) {
							calculate++;
						}
						else {
							//记录最长匹配数
							if (m_max < calculate) {
								*n = i;
								m_max = calculate;
							}
							break;
						}
					}
					
				}
			}

		}
	}
	cout << *n<< endl;
}
```



## 反馈与建议
- 邮箱：<lbs1203940926@163.com>
- 微信：[@脱缰的哈士奇(ab1203940926)](http://ojx8u3g1z.bkt.clouddn.com/wechat-id.jpg)
- 微博：[@脱缰的哈士](http://weibo.com/2329754491/profile) 