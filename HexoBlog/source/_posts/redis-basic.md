---
title: Redis基础知识梳理
date: 2020-01-31 10:25:26
categories: Back-End
tags: [Back-End,DataBase,Redis]
keywords: Back-End,DataBase,Redis
toc: true
password:
comments: true
top:
---


* 对 Redis 基础知识进行梳理，包括Redis的5种数据类型，事务，过期时间，消息通知，优先级队列，管道，数据持久化，复制，哨兵，事务等

<!--more-->

## 更新日志
* 2020/01/30，撰写




## 学习资料汇总
* [Redis官网](https://redis.io/)
* [Redis中文官网](http://www.redis.cn/)
* [Redis菜鸟教程](https://www.runoob.com/redis/redis-tutorial.html)
* [Redis源码](https://github.com/antirez/redis) —— 使用C语言开发，源码只有3万多行，降低了用户通过修改Redis源码来提升性能的门槛



## Redis 初识
* Redis = Remote Dictionary Server，远程字典服务器
* Redis 是一个 高性能的 `key-value` 存储系统，通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和有序集合(sorted sets)等类型。
* Redis 与其他 `key - value` 缓存产品有以下3个特点
    1. Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用
    2. Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储
    3. Redis支持数据的备份，即master-slave模式的数据备份
* Redis 性能极高，读取速度是110000次/s，写入的速度是81000次/s。Redis数据库中的所有数据都存储在内存中，内存读写速度远快于磁盘。
* Redis功能丰富，除了用于数据库开发，还可以用于缓存，队列系统等。
* Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。

## Redis环境配置

### 安装 Redis

* Mac上，建议使用Homebrew安装Redis

```
brew install redis    //会默认安装当前最新的稳定版本

/// 此处安装路径为 usr/local/Cellar/redis/5.0.7
//==> Caveats
// To have launchd start redis now and restart at login:
//  brew services start redis
//Or, if you don't want/need a background service you can just run:
//  redis-server /usr/local/etc/redis.conf
//==> Summary
//🍺  /usr/local/Cellar/redis/5.0.7: 13 files, 3.1MB
```
* 如果需要后台运行Redis服务，使用命令 `brew services start redis`
* 如果不需要后台运行Redis服务，使用命令 `redis-server /usr/local/etc/redis.conf`

### 启动/停止Redis
执行 `brew services start redi` 命令，第一次启动 Redis 后，在 `/usr/local/bin` 目录下，会生成如下文件夹

文件名 | 说明
-------|----------
redis-server | redis 服务器
redis-cli | redis 命令行客户端
redis-benchmark | redis 性能测试工具
redis-check-aof | AOF 文件修复工具
redis-check-dump | RDB 文件检查工具
redis-sentinel | Sentinel 服务器 



* 启动Redis



```
redis-server   //默认端口号 6379

redis-server --port 6380  //指定端口号
```

* 停止Redis

考虑到 Redis 有可能正在将内存中数据同步到硬盘中，强行终止Redis进程可能会导致数据丢失。因此，正确停止Redis的方式应该是向Redis发送 `SHUTDOWN` 命令，方法为

```
redis-cli SHUTDOWN
```

当 Redis 收到 `SHUTDOWN`  命令后，会先断开所有客户端连接，然后根据配置执行数据持久化，最后完成退出。

Redis 可以妥善处理 `SIGTERM` 信号，所以使用 `kill` Redis 进程的 PID，也可以正常结束 Redis，效果和发送 `SHUTDOWN` 命令一样。



### redis-cli

Redis 命令用于在 Redis 服务上执行操作。要在 Redis 服务上执行命令需要一个 Redis 客户端。

`redis-cli` 是Redis自带的基于命令行的Redis客户端，下面介绍如果通过 `redis-cli` 向 Redis 发送命令。


通过 `redis-cli` 向 Redis 发送命令有2种方式
* 方式1：将命令作为 `redis-cli` 的参数执行。例如 `redis-cli SHUTDOWN`
* 方式2：执行 `redis-cli`（不附带任何参数），进入交互模式后，可以自由输入命令


```
// 方式1：将命令作为 redis-cli 的参数执行

redis-cli SHUTDOWN

//redis默认服务器地址127.0.0.1，默认端口号6379 
//也可以使用-h指定服务器地址，-p指定端口号
redis-cli -h 127.0.0.1 -p 6379  

//使用PING命令测试客户端和Redis的连接是否正常
redis-cli PING  //返回PONG 表示连接正常
```


```
//方式2: 执行 redis-cli（不附带任何参数），进入交互模式后，可以自由输入命令
lbsMacBook-Pro:~ lbs$ redis-cli
127.0.0.1:6379> PING
PONG
127.0.0.1:6379> 
```



### 多数据库

Redis 是一个字典结构的存储服务器，**一个 Redis 实例**提供了多个用来存储的字典，可以把其中的每个字典都理解成一个独立的数据库。
* 每个数据库对外都是一个从0开始的递增数字命名，Redis默认支持16个数据库
* Redis不支持自定义数据库名称，每个数据库都以编号命名
* 需要注意的是，一个Redis实例的多个数据库之间并不是完全隔离的，比如 `FLUSHALL` 命令可以清空一个 Redis 实例中所有数据库中的数据。综上所述，这些数据库更像是一种命名空间，而不适宜存储不同应用程序的数据。比如，可以使用0号数据库存储应用A的生产环境数据，使用1号数据库存储应用A的测试环境数据，而不应该使用1号数据库存储应用B的数据。
* **不同的应用，应该使用不同的Redis实例存储数据。** 由于Redis非常轻量，一个空的Redis实例占用的内存只有1MB，所以不用担心多个Redis实例会额外占用很多内存。



## Redis 入门

### 命令行基础




1. 获得符合规则的键名列表

```
KEYS pattern
```

`pattern` 支持 `glob` 风格通配符格式，具体如下


符号    | 含义
--------|-----------
?       | 匹配一个字符
*       | 匹配任意个（包括0个）字符
[]      | 匹配括号间的任一字符，可以使用 `-` 表示一个范围，如 `[1-9]`
\x      | 用于转义字符


例如，查询当前的所有键名列表

```
> set bar 1
OK
> KEYS *
1) "bar"
```

2. 判断一个键是否存在

```
EXISTS key

//返回(integer) 1，表示存在
//返回(integer) 0，表示不存在
```

3. 删除键

```
DEL key [key ...]

//返回(integer) 1，表示删除成功
//返回(integer) 0，表示删除不成功
```

4. 获得键值的数据类型

```
TYPE key

//返回 string list 等
```



### Redis 5 种数据类型
Redis 支持5种数据类型
* string（字符串）
* hash（哈希或散列）
* list（列表）
* set（集合）
* zset(sorted set，有序集合)


#### 字符串类型

1. 赋值和取值

```
SET key value

GET key   //当键不存在时会返回空结果
```

2. 递增数字

```
INCR key
```

当要操作的键不存在时，默认键值为0，所以第一次递增后结果为1，如下所示

```
> INCR  num
(integer) 1
> INCR  num
(integer) 2
```

3. 增加指定的整数


```
INCRBY key increment
```

4. 减少指定的整数


```
DECR key

DECRBY key increment
```

5. 增加指定浮点数

```
INCRBYFLOAT key  increment
```

6. 向尾部追加值

```
APPEND key  value
```

`APPEND` 命令中，若键不存在，则将该键的值设为 `value`，相当于执行了 `SET key value`。


7. 获取字符串长度

```
STRLEN key
```

对于汉字，Redis使用UTF-8编码的中文，如下示例中，“你”和“好”两个字的UTF-8编码的长度都是3，所以返回的字符串长度为6。

```
> SET key1 你好
OK
> STRLEN key1
(integer) 6
```

8. 同时获得/设置多个键值

```
MSET key1 value1 key2 value2 key3 value3

MGET key1 key2 key3
```


9. 位操作

```
GETBIT key offset   //获取一个字符串类型键指定位置的二进制位的值（0或1）
 
SETBIT key offset value  //设置指定位置的二进制位的值

BITCOUNT key [start] [end] //获得字符串类型键中值是1的二进制位个数

BITOP operation destkey key [key ...] //对多个字符串类型键进行位运算，结果存储在destkey
```

一个字节由8个二进制位组成。Redis 提供了 4 个命令可以直接对二进制位操作。

下面进行示例分析。

```
SET foo bar
```

"bar"的3个字母对应的ASCII码分别是98，97，114，转换为对应的二进制数值，`foo` 键中的二进制位结构为`01100010,01100001,01110010`。


(1) 使用 `GETBIT key offset ` 获取一个字符串类型键指定位置的二进制位的值（0或1）

```
redis> GETBIT foo 6
(integer) 1
```

如果获取的二进制位的索引超出了键值的二进制位的实际长度，则默认返回 0

(2) `BITOP operation destkey key [key ...]` 可以对多个字符串类型键进行位运算，结果存储在 `destkey`

```
127.0.0.1:6379> SET foo1 bar
OK
127.0.0.1:6379> SET foo2 aar
OK
127.0.0.1:6379> BITOP OR res foo1 foo2  //或运算
(integer) 3
127.0.0.1:6379> GET res
"car"
```


#### 哈希（散列）类型

哈希（或散列）`hash` 类型的键值也是一种字典结构，其存储了字段（`field`）和字段值的映射，但字段值只能是字符串，不支持其他数据类型。


1. 赋值和取值

```
HSET key field value  //不区分插入和更新操作，即更新数据时不用事先判断是否存在 
HGET key field

HMSET key field value [field value ...]
HMGET key field [field ...]

HGETALL key
```

2. 判断字段是否存在

```
HEXISTS key field
```

3. 仅仅在字段不存在是赋值

```
HSETNX key field value
```

`HSETNX` 和 `HSET` 命令类似，区别在于如果字段已经存在，`HSETNX` 命令将不执行任何操作。

`HSETNX` 中的 `NX` 表示 `if Not eXists`。

4. 增加数字

```
HINCRBY key filed increment
```

5. 删除字段


```
HDEL key field [field ...]
```

6. 只获取字段名称或字段值

```
HKEYS key

HVALS key
```
7. 获得字段数量

```
HLEN key
```

#### 列表类型

列表（`list`）可以存储一个有序的字符串列表，常用的操作是向列表两端添加元素，或者获取列表的某一个片段。

列表类型内部使用的是双向链表实现的，所以想列表两端添加元素的时间复杂度是 `O(1)`，获取越接近两端的元素速度就越快，但是通过索引访问元素会比较慢。因此，针对双向链表的特点，列表类型特别适合如下场景
* 如社交网站的新鲜事，我们只关心最新内容，即使新鲜事达到几千万条，获取列表尾部的100条最新数据也是很快的
* 如日志记录场景，双向链表保证了插入新日志的速度不会受到已有日志数量的影响
* 借助列表类型，Redis还可以作为队列使用

1. 向列表两端增加元素


```
LPUSH key value [value ...]   //向列表左边插入元素
RPUSH key value [value ...]
```


2. 从列表两端弹出元素


```
LPOP key 
RPOP key 
```

3. 获取列表中元素个数


```
LLEN key     // 当键不存在则返回0
```

4. 获得列表片段


```
LRANGE key start stop   //返回区间[start,stop]的列表片段（区间闭合），不改变原列表
```


5. 删除列表中指定的值


```
LREM key count value  
//删除列表中前count个值为value的元素，返回值是实际删除的元素个数
```

当 `count` 大于0时，会从列表左边开始删除前`count` 个；当小于0时，会从列表右边边开始；当 `count` 等于0时，会删除列表中所有值为 `value` 的元素。



6. 获得/设置指定索引的元素值


```
LINDEX key index

LSET key index value
```
若 `index` 小于0，表示从列表右边开始计算索引，最右边的元素的索引值是 -1。




7. 只保留列表指定片段


```
LTRIM key start stop   //只保留区间[start,stop]的列表片段（区间闭合），改变了原列表
```


8. 向列表中插入元素


```
LINSERT key BEFORE|AFTER pivot value   
//从左到右查找列表的pivot元素，在该元素前或后，插入value元素
```

9. 将元素从一个列表转到另一个列表


```
RPOPLPUSH source destination
//RPOPLPUSH表示先执行RPOP，再执行LPUSH
```

#### 集合类型

相比于列表，集合（`set`）中的元素是全局唯一的，并且是无序的。

集合（`set`）类型在Redis内部是使用值为空的散列表(`hash table`)实现的，所以向集合中插入或删除元素，判断元素是否存在，这些操作的时间复杂度都是 `O(1)`。

更方便的是，采用集合类型，多个集合类型之间还可以进行交集，并集和差集运算。



1. 增加/删除元素

```
SADD key member [menber ...] //如果键不存在，则自动创建，返回值为成功加入的元素的数量
SREM key member [menber ...]
```

2. 获得集合找那个所有的元素

```
SMEMBERS key
```

3. 判断元素是否在集合中

```
// 时间复杂度位O(1) 速度较快
SISMEMBER key member
```

4. 集合之间的运算

```
SDIFF  key [key ...]    //差集
SINTER key [key ...]    //交集
SUNION key [key ...]    //并集
```

5. 获取集合中元素个数

```
SCARD key
```


6. 进行集合运算并将结果存储

```
SDIFFSTORE destination key [key ...]
SINTERSTORE destination key [key ...]
SUNIONSTORE destination key [key ...]
```

7. 随机获得集合中的元素

```
SRANDMEMBER key [count]
```

8. 从集合中弹出一个元素

```
SPOP key 
```

#### 有序集合类型

有序集合(`sorted list`)类型，是在集合类型的基础上，为每个元素关联一个分数（`score`，可以理解为索引值），使得元素有序。

Redis中，采用哈希表和跳跃表（`Skip list`）实现有序集合类型。所以即使读取位于中间部分的数据，速度也是很快的（时间复杂度是``O(logN)`）。



1. 增加元素

```
//添加一个member元素和该元素的分数score
ZADD key score member [score member]
```

2. 获得元素的分数

```
ZSCORE key member
```

3. 获得排名在某个范围的元素列表

```
//按照元素分数从小到大的顺序，返回索引在区间[start,stop]的所有元素
//如果有参数WITHSCORES，表示返回的元素列表包含分数信息
ZRANGE key start stop [WITHSCORES]

//类似ZRANGE，只不过是按照分数从大到小的顺序
ZREVRANGE key start stop [WITHSCORES]
```


4. 获得指定分数范围的元素

```
//按照元素分数从小到大的顺序，返回索引在区间[start,stop]的所有元素
//如果有参数WITHSCORES，表示返回的元素列表包含分数信息
ZRANGE key start stop [WITHSCORES]

//类似ZRANGE，只不过是按照分数从大到小的顺序
ZREVRANGE key start stop [WITHSCORES]
```

5. 获得指定分数范围的元素

```
//按照元素分数从小到大，返回分数在区间[min,max]之间的元素
//LIMIT offset count 表示在获得元素列表的基础上，向后偏移（跳过）offset个元素，并且只获取之后的前count个元素
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
```


6. 增加某个元素的分数

```
ZINCRBY key increment member
```


7. 获得集合中元素的数量

```
ZCARD key
```

8. 获得指定分数范围内的元素个数

```
ZCOUNT key min max
```


9. 按照排名范围删除元素

```
//按照元素分数从小到大顺序，删除指定排名范围内的元素，并返回删除的元素数量
ZREMRANGEBYRANK key start stop
```

10. 按照分数范围删除元素

```
//返回删除的元素数量
ZREMRANGEBYSCORE key min max
```

11. 获得元素的排名

```
ZRANK key member    //按照分数从小到大顺序
ZRANK key member    //按照分数从大到小顺序
```


## Redis进阶

### 事务

#### MULTI EXEC

事务(`transaction`)是一组命令的集合，事务同命令一样，都是Redis的最小执行单位。一个事务中的命令要么都执行，要么都不执行。事务的应用非常普遍，如银行转账等。

Redis的事务还可以保证一个事务内的命令一次执行而不被其他命令插入。

事务的原理是先将属于一个事务的命令发送给Redis，然后再让Redis一次执行这些命令，例如

```
redis> MULTI
OK
redis> SADD "user:1:following" 2
QUEED
redis> SADD "user:2:following" 1
QUEED
redis> EXEC
1) (integer) 1
2) (integer) 1
```

上面的代码演示了事务的使用方式。首先使用 `MULIL` 命令告诉Redis：“下面我发给你的命令属于同一个事务，你先不要执行，而是把它们暂时存起来”。

随后发送两个 `SADD` 命令执行事务操作，Redis返回 `QUEED` 表示这2条命令已经进入等待执行的事务队列中了。

最后，发送 `EXEC` 命令告诉Redis将等待执行的事务队列中的所有命令按照发送顺序执行。

**事务中的命令是在 `EXEC` 之后才执行的，因此，一个事务中，只有当所有命令都依次执行完成后，才能得到每个结果的返回值。**

#### WATCH

**一个事务中，只有当所有命令都依次执行完成后才能得到每个结果的返回值。** 可是有些情况下，需要先获得一条命令的返回值，根据返回值再执行下一条命令。针对该情况，可以使用 `WATCH` 命令。

`WATCH` 命令可以监控一个或多个键，一旦其中有一个键被修改或删除，之后的事务就不会执行。监控一直到 `EXEC` 命令。（事务中的命令是在 `EXEC` 之后才执行的，所以在 `MULTI` 命令后可以修改 `WATCH` 监控的键值）

```
redis> SET key 1
OK
redis> WATCH key
OK
redis> SET key 2
OK
redis> MULTI
OK
redis> SET key 3
QUEED
redis> EXEC
nil
redis> GET key
"2"
```

上例中，执行 `WATCH` 命令后，事务修改了 `key` 值，所以最后事务代码并没有执行，`EXEC` 命令返回结果为 `nil`。

执行 `EXEC` 命令后悔取消对所有键的监控。


### 过期时间

Redis 中可以使用 `EXPTRE` 命令设置一个键的过期时间，到时间后 Redis 会自动删除它。


1. 设置过期时间

```
EXPIRE key seconds   //seconds 单位为秒

PEXPIRE key milliseconds  //milliseconds 单位为毫秒
```

2. 查询键还有多少时间会被删除


```
TTL key   //返回时间单位为秒

PTTL key  //返回时间单位为毫秒

// 键不存在时，命令返回-2
// 键未设置过期时间时，命令返回-1
```


3. 取消键的过期时间设置

```
PERSIST key
```
除了 `PERSIST` 命令外，使用 `SET` 或 `GETSET` 命令为键赋值，也会同时清除键的过期时间。

#### 实现访问频率限制

考虑如下场景——为了减轻服务器的压力，限制每个用户（IP）每分钟最多只能访问10次，就可以使用过期时间 `EXPIRE` 实现
* 创建一个 `rate.limiting:userIP` 的键
* 设置 `EXPIRE key seconds`，过期时间为60s。一分钟后，该键会被自动删除
* 用户每次访问服务器，使用 `INCR` 递增该键值
* 当访问次数达到10后，提示用户稍后访问

上述流程的伪代码如下

```
$isKeyExists = EXISTS rate.limiting:$IP
if isKeyExists is 1
    $time = INCR rate.limiting:$IP
    if $time > 10
        print 访问频率超过了限制，请稍后再试
        exit
else
    MULTI           //使用事务，避免EXPIRE因为某种原因未执行，导致该键值一直存在
    INCR rate.limiting:$IP
    EXPIRE $keyName, 60
    EXEC
```

上述代码还有一个问题，比如用户在第1分钟的最后一秒访问了9次，又在下一分钟的第一秒访问了10次。这种访问是可以通过上述访问限制的，但实际上用户在2秒内访问了19次服务器。

为了处理上述场景，可以对代码进行优化
* 使用一个列表存储用户最近10次访问服务器的时间
* 一旦键中的元素大于10个，就判断时间最早的元素距现在的时间是否小于1分钟。
* 如果是，则表示用户最近一分钟的访问次数超过了10次，进行限流提醒。
* 如果不是，就将现在的时间加入到列表中，同时把最早的元素删除。

```
$listLength = LLEN rate.limiting:$IP
if listLength < 10
    LPUSH rate.limiting:$IP,now()
else
    $time = LINDEX rate.limiting:$IP, -1
    if now() - $time > 60
        print 访问频率超过了限制，请稍后再试
    else
        LPUSH rate.limiting:$IP,now()
        LTRIM rate.limiting:$IP,0,9
```

#### 实现缓存

为了提高服务器负载能力，常常需要将一些访问频率较高但是CPU或则IO资源消耗较大的操作的结果缓存起来，并希望让这些缓存过一段时间后自动过期。

实际开发中很难为缓存键设定合理的过期时间，为此可以限制Redis可以使用的最大内存，并让Redis按照一定的规则淘汰不需要的缓存键。这种方式在只将Redis用作缓存系统时非常实用。

Redis配置文件的 `maxmemory` 属性限定了Redis可以使用的最大内存。当超出这个限制时，Redis会依据 `maxmemory-policy` 参数指定的策略来删除不要的键值直到Redis占用的内存大小小于指定内存。

`maxmemory-policy` 支持`LRU`(`Least Recently Used`) 算法规则，即“最近最少使用原则”，其认为最近最少使用的键在未来一段时间内也不会被用到，当内存不足时这些键是可以被删除的。



### 排序

`SORT` 命令可以对列表类型，集合类型和有序集合类型键进行排序，并且可以完成和关系数据库中的连接查询类似的任务。

```
SORT key BY 参考键 GET ... STORE destkey
```
* `BY 参考键`中，如果提供了 `BY` 参数，`SORT` 命令将不再依据元素自身的值进行排序，而是对每个元素使用元素的值替换参考键中的第一个 `"*"` 并获取其值，然后依据该值对元素进行排序。

例如，下述语句将读取如 `post:2`，`post:6`，`post:12`，`post:26` 几个散列键中的 `time` 字段的值并以此决定排序结果。

```
SORT tag:ruby:posts BY post:*->time DESC
```


* `GET` 参数不影响排序，它的作用是使 `SORT` 命令的返回结果不再是元素自身的值，而是 `GET` 参数中指定的键值。
* `STORE` 参数用于将排序结果存储到指定的键中


#### 性能优化
`SORT` 命令的时间复杂度是 `O(n+mlog(m))`，其中 `n` 表示要排序的列表（或集合或有序集合）中的元素个数，`m`表示要返回的元素个数。当 `n` 较大的时候，排序命令的性能相对较低，并且 Redis 在排序前会建立一个长度为 `n` 的容器来存储待排序的元素，虽然是一个临时的过程，但是如果同时进行较多的大数据量的排序操作则会严重影响性能。

所以开发中使用 `SORT` 命令需要注意
1. 尽可能减少待排序键中元素的数量（使 `n` 尽可能小）
2. 使用 `LIMIT` 参数只获取需要的数据（使 `m` 尽可能小）
3. 如果需要排序的数据量较大，尽可能使用 `STORE` 参数将结果缓存


### 消息通知

#### 任务队列

任务队列，即“传递任务的队列”。和任务队列进行交互的实体有两类，一类是生产者（`producer`），另一类是消费者（`consumer`）。生产者会将需要处理的任务放入任务队列中，而消费者会不断地从任务队列中读入任务信息并执行。

使用任务队列的好处
1. 松耦合：生产者和消费者不需要知道彼此的实现细节，只需约定好的任务的描述格式即可。
2. 易于扩展：消费者可以扩展到多个，而且可以分布在不同的服务器中，可以轻易地降低单台服务器的负载


#### 使用Redis实现任务队列


使用Redis的列表结构和 `RPOP`，`LPUSH` 命令，可以实现简单的任务队列，伪代码如下所示

```
# 无限循环读取任务队列中的内容
loop
    $task = RPOP queue
    if $task
        # 如果任务队列中有任务则执行任务
        execute($task)
    else
        # 如果没有则等待1秒，以免过于频繁地请求数据
        wait 1 second
```

上述伪代码有个不足之处，当任务队列中没有任何任务时，消费者每秒都会调用一次 `RPOP` 命令查询是否有新任务。

如果可以实现一旦有新任务加入任务队列就通知消费者就好了。其实借助 `BRPOP` 命令就可以实现这样的需求。


如上伪代码可以优化为

```
loop
    # 如果任务队列中没有新任务，BRPOP命令会一直阻塞，不会执行execute()
    $task = BRPOP queue, 0
    # 返回值是一个数组，数组第2个元素是我们需要的任务
    execute($task[1])
```


**`BRPOP` 和 `RPOP` 命令类似，唯一的区别就是当列表中没有元素时，`BRPOP` 命令会一直阻塞连接，直到有新元素加入。** 

```
BRPOP key [key ...] timeout
```

`BRPOP` 命令接收2个参数，第1个参数是键名，第2个参数是超时时间，单位是秒。当超过了此时间仍然没有获得新元素的话，就会返回 `nil`。如果传入时间参数为0（如下伪代码示例），则表示不限制等待时间，即如果没有新元素加入队列就会永远阻塞下去。

获得一个元素后，`BRPOP` 命令会返回一个数组，共2个值，分别是键名和元素值。第2个参数，元素值，就是待处理的任务。



#### 优先级队列

在实际开发中，针对多种不同的任务，经常会需要根据任务的优先级，去执行优先级较高的任务，即需要实现一个优先级队列。


`BRPOP` 和 `BLPOP` 命令可以同时接收多个键，可以实现优先级队列。

```
BRPOP key [key ...] timeout
```

例如，`BLPOP queue:1 queue:2 0`，表示同时检测多个键——`queue:1` 和 `queue:2`
* 如果所有键都没元素则阻塞
* 如果有一个键有元素则会从该键中弹出元素
* 如果多个键都有元素，则按照**从左到右**顺序读取第一个键中的一个元素



```
redis> LPUSH queue:2 task1
1) (integer) 1
redis> LPUSH queue:3 task2
1) (integer) 1

//...
redis>BRPOP queue:1 queue:2 queue:3 0
1) "queue:2"
2) "task1"


redis>BLPOP queue:1 queue:2 queue:3 0
1) "queue:3"
2) "task2"
```

使用 `BRPOP` 创建优先级队列时，`RPOP` 表示队列左进右出，因此有如下结构。即 `BRPOP queue:1 queue:2 queue:3 0` 命令中，越靠左的键优先级越高（`queue:1` 的优先级最高，因此会按照**从左到右**顺序读取第一个键中的一个元素。

```
(L-左进)--- | queue:3| queue:2| queue:1| --> (R-右出) 
```

使用 `BLPOP` 创建优先级队列时，`LPOP` 表示队列右进左出，因此有如下结构。即 `BLPOP queue:1 queue:2 queue:3 0` 命令中，越靠右的键优先级越高（`queue:3` 的优先级最高），因此会按照**从右到左**顺序读取第一个键中的一个元素。

```
(L-左出) <--- | queue:1| queue:2| queue:3| --- (R-右进) 
```



#### “发布/订阅”模式

“发布/订阅”模式包含两种角色，分别是发布者和订阅者。订阅者可以订阅一个或多个频道（`channel`），发布者可以向指定的频道发送消息，所有订阅该频道的订阅者都会受到改消息。

* 发布消息

```
PUBLISH channel message   //命令返回值表示接收到这条消息的订阅者的数量
```

发布出去的消息不会被持久化，也就是说当有客户订阅该频道 `channel`后只能收到后续发布的消息，之前发送的消息就收不到了。


* 订阅消息

```
SUBSCRIBE channel [channel ...]
```

执行 `SUBSCRIBE` 命令后客户端会进入订阅状态，此状态下的客户端不能使用 `SUBSCRIBE`, `UNSUBSCRIBE`, `PSUBSCRIBE` 和 `PUNSUBSCRIBE` 这4个属于 “发布/订阅”模式的命令之外的命令，否则会报错。

进入订阅状态后的客户端，可能收到3种类型的回复，每种类型的回复都包括3个值。第一个值是消息类型，根据消息类型的不同，第二，第三个值的含义也不同。消息类型的取值可能有以下3个
1. `subscribe`。表示订阅消息成功的反馈。第2个值是订阅成功的频道名称，第3个值是当前客户端订阅的频道数量
2. `message`。表示接收到的消息。第2个参数表示频道名称，第3个参数是消息内容
3. `unscribe`。表示成功取消订阅某个频道。第2个参数表示频道名称，第3个参数是当前客户端订阅的频道数量，当该值为0时，客户端会退出订阅状态。

下面看一个实例。

1. 首先Redis的一个实例RedisA在频道 `channel1.1` 发布一个消息

```
//RedisA
redisA> PUBLISH channel1.1 hi
(integer) 0     //表示当前没有客户端订阅该消息
```

2. Redis的另外一个实例RedisB 订阅频道 `channel1.1`

```
//RedisB
redisB> SUBSCRIBE channel1.1
Reading messages... (press Ctrl-C to quit)
1) "subscribe"   //订阅消息成功
2) "channel1.1"
3) (integer) 1
```

3. 实例RedisA继续在频道 `channel1.1` 发布一个消息

```
//RedisA
redisA> PUBLISH channel1.1 hello
(integer) 1     //表示当前没有客户端订阅该消息
```

4. 此时，实例RedisB 会收到如下消息


```
1) "message"
2) "channel1.1"
3) "hello"
```


#### 按照规则订阅

可以使用 `PSUBSCRIBE` 订阅指定的规则，规则支持 `glob` 风格通配符格式。

符号    | 含义
--------|-----------
?       | 匹配一个字符
*       | 匹配任意个（包括0个）字符
[]      | 匹配括号间的任一字符，可以使用 `-` 表示一个范围，如 `[1-9]`
\x      | 用于转义字符


例如 `PSUBSCRIBE channel1.?*`命令中，规则 `channel1.?*` 可以匹配 `channel1.1` 和 `channel1.10`，但不会匹配 `channel1.1`。


### 管道

* 客户端和Redis使用TCP协议连接。
* 不论是客户端向Redis发送命令，还是Redis向客户端返回命令结果，都需要经过网络传输。这两个部分的总耗时成为**往返时延。**
* 大致来说，到本地回环地址（`loop back address`）的往返时间，在数量级上相当于Redis处理一条简单命令 (如 `LPUSH list 1 2 3`) 的时间。
* Redis的底层通信协议对管道（`pipelining`）提供了支持。通过管道可以一次性发送多条命令并在执行完后一次性将结果返回。
* 当一组命令中每条命令都不依赖于之前命令的执行结果时就可以将这一组命令一起通过管道发出。管道通过减少客户端和Redis的通信次数，来实现降低往返时延累计值的目的。

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/back-end-2020/redis-basic-pipelining-1.png)



### 节省空间
Redis是一个基于内存的数据库，所有数据都存储在内存中。因此如何节省内存，控制成本，至关重要。节省内存空间，可以从以下方面考虑
1. 精简键名和键值
2. 内部编码优化

Redis为每种数据类型都提供了2种内部编码方式，用于节省内存空间。

以散列类型为例，散列类型是通过散列表实现的，这样可以实现时间复杂度 `O(1)` 的查找，赋值操作。然而当元素较少时，`O(1)`的操作并不会比 `O(n)` 有明显的性能提高。所以Redis会根据实际情况自动调整，采用一种更为紧凑但性能稍差（查找元素的复杂度为`O(n)`）的编码方式。


**内部编码方式的选择，对于开发者来说是透明的。** 可以使用 `OBJECT ENCODING key` 命令查看某个键的内部编码方式。

```
redis> SET foo bar
OK
redis> OBJECT ENCODING foo
"raw"
```

## Redis 脚本

Redis 脚本使用 Lua 解释器来执行脚本。 Redis 2.6 版本（和之后版本）通过内嵌支持 Lua 环境。执行脚本的常用命令为 `EVAL`。 使用脚本的好处包括
1. 减少网络开销：使用脚本可以将多个命令的多次请求，通过一次请求完成，减少网络往返时延
2. 原子操作：Redis将整个脚本作为一个整体执行，中间不会被其他命令插入。即编写脚本过程中无需担心会出现竞态条件，也无需使用事务。事务可以完成的所有功能都可以使用脚本实现。
3. 复用：客户端发送的脚本会永久存储在Redis中，其他客户端也可以复用这一脚本。


## Redis 持久化

Redis 支持2种方式的持久化
1. `RDB` 方式：根据指定的规则“定时”将内存中的数据存储在硬盘上
2. `AOF` 方式：每次执行命令后将命令本身记录下来

通过 `RDB` 方式实现持久化，一旦 Redis 异常退出，就会丢失最后一次快照以后更改的所有数据。如果数据相对重要，希望损失降到最小，则可以使用 `AOF` 方式进行持久化。


Redis 允许同时开启 `RDB` 和 `AOF` 方式，既保证了数据安全又使得进行备份等操作十分容易。此时重新启动 Redis 后，Redis 会使用 AOF 文件来恢复数据，因为 AOF 方式的持久化可能丢失的数据更少。


### RDB方式

`RDB` 方式的持久化是通过快照（`snapshoting`）完成的，当符合一定条件时 Redis 会自动将内存中的所有数据生成一份副本并存储在硬盘上，这个过程即“快照”。


#### 快照触发条件

Redis 会在以下几种情况下对数据进行快照
1. 根据配置规则进行自动快照：每当时间窗口 M 内被更改的键的个数大于 N 时，即符合自动快照条件
2. 用户执行 `SAVE` 或 `BGSAVE` 命令
    * 执行 `SAVE` 命令时，Redis同步地执行快照操作。快照执行过程中会阻塞所有来自客户端的请求。所以应该尽量避免在生产环境中执行该指令
    * `BGSAVE` 命令可以在后台异步地执行快照操作，同时可以响应客户端的请求
3. 执行 `FLUSHALL` 命令
    * `FLUSHALL` 命令会清楚数据库中的所有操作
    * 只要自动快照条件不为空（即使不满足该条件），执行 `FLUSHALL` 命令后，也会触发快照操作
    * 当没有定义自动快照条件时，执行 `FLUSHALL` 命令不会触发快照操作
4. 执行复制（`replication`）时：设置了主从模式时，Redis会在复制初始化时进行自动快照

#### 快照原理

Redis 默认会将快照文件存储在 Redis 当前进程的工作目录种的 `dump.rdb` 文件中。快照的过程如下
1. Redis 使用 `fork` 函数复制一份当前进程（父进程）的副本（子进程）
2. 父进程继续接收并处理客户端发来的命令，而子进程开始将内存中的数据写入硬盘中的临时文件
3. 当子进程写入完成所有数据后会用该临时文件替换旧的 `RDB` 文件，至此一次快照操作完成。


### AOF方式

`AOF`(`append only file`) 方式可以将 Redis 执行的每一条命写命令追加到硬盘文件中，这一过程显然会降低 Redis 性能。但大部分情况下这个影响是可以接收的，另外使用较快的硬盘可以提高 `AOF` 的性能。


* 开启AOF

默认情况下没有开启AOF，执行如下命令可以开启AOF。默认情况下，AOF文件的保存位置和RDB文件的位置相同，默认的文件名是 `appendonly.aof`。

```
appendonly yes
```

* AOF的实现

AOF 文件的内容正是Redis客户端向Redis发送的原始通信协议的内容。


* 同步硬盘数据

由于操作系统的缓存机制，AOF文件数据并没有直接真正地写入硬盘，而是进入了系统的硬盘缓存。在默认情况下系统每 30 秒会执行一次同步操作，以便将硬盘缓存中的内容真正地写入硬盘。在这30秒内，如果系统异常退出则会导致硬盘缓存中的数据丢失。

一般来讲，启用AOF持久化的应用都无法容忍这样的损失。这就需要 Redis 在写入AOF 文件后主动要求系统将硬盘缓存内容同步到硬盘中。在 Redis 中，可以通过 `appendfsync` 参数设置同步的时机

```
# appendfsync always
appendfsync everysec   //默认
# appendfsync no       //不主动进行同步操作，即交由操作系统处理（每30秒同步一次）
```

## 集群

同时拥有多个 Redis 服务器后，就会面临如果管理集群的问题，包括如何增加节点，故障恢复等操作。


### 复制 replication

为了避免单点故障，通常的做法是将数据库复制多个副本以部署在不同的服务器上。为此，Redis提供了复制 `replication` 功能，可以实现当一台数据库中的数据更新后，自动将更新的数据同步到其他数据库中。

#### 配置

在复制的概念中，数据库分为2种
1. 主数据库（`master`）：可以进行读写操作，当写操作导致数据变化时会自动将数据同步给从数据库
2. 从数据库 （`slave`）：一般是只读的（也可以配置为可写入），并接受主数据库同步过来的数据


Redis中，只需要在从数据库配置文件中加入如下配置，即可完成复制操作，主数据库不需要任何配置。

```
salveof 主数据库地址   主数据库端口
```

#### 示例

1. 启动一个Redis实例作为主数据库，默认端口号是6379

```
$redis-server    //默认端口号是6379
```

2. 启动另一个Redis实例作为从数据库，监听端口号 6380

```
$redis-server   --port 6380 --slaveof 127.0.0.1 6379
```

此时主数据库中任何数据变化，都会自动同步到从数据库中。

3. 打开 `redis-cli` 实例A并连接到主数据库。再打开 `redis-cli` 实例B并连接到从数据库

```
$redis-cli -p 6379
127.0.0.1:6379>

$redis-cli -p 6380
127.0.0.1:6380>
```

4. 使用 `INFO replication` 命令在实例A和实例B中查看复制相关的信息

```
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6380,state=online,offset=336,lag=1
//...
```
可以看到，实例A的角色是主数据库，其已连接的从数据库的个数是1。

```
127.0.0.1:6380> INFO replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
```
可以看到，实例B的角色是从数据库。


5. 在主数据库中添加键值，可以在从数据库中读取

```
# 主数据库写入
127.0.0.1:6379> SET foo bar
OK
```

```
# 从数据库读取
127.0.0.1:6380> GET foo
"bar"
```


#### 图结构

从数据库不仅可以接受主数据库的同步数据，自己也可以同时作为主数据库存在，形成类似图的结构，如下图所示。

主数据A的数据会同步到B和C，而B中的数据会同步到D和E中。向B中写入的数据不会同步到A或C中。

```
graph TB

A[主数据库A]-->B[从数据库B]
A[主数据库A]-->C[从数据库C]

B[从数据库B]-->D[从数据库D]
B[从数据库B]-->E[从数据库E]
```





### 哨兵



#### 面临的问题

为了提高性能，可以通过复制功能建立若干个从数据库，并在从数据库中启用持久化，同时在主数据中禁用持久化。这样可以保证主数据库的性能。
1. 当从数据库崩溃重启后，主数据库会自动将数据同步过来，所以无需担心数据丢失
2. 当主数据库崩溃后，情况就比较复杂了。手动通过从数据库恢复主数据库数据时，需要严格执行如下2步
    * 在从数据库中使用 `SLAVE NO ONE` 命令将从数据库提升为主数据库继续服务
    * 启动之前崩溃的主数据库，然后使用 `SLAVEOF` 命令将其设置成新的主数据库的从数据库，即可将数据同步回来

可见，手动维护主从数据库崩溃后的数据恢复是相当麻烦的。Redis提供了一种自动化方案——哨兵，避免了手工维护的麻烦和容易出错的问题。


#### 哨兵的功能

Redis 2.8中提供了哨兵工具来实现自动化的系统监控和故障恢复功能。

哨兵的作用就是监控 Redis 系统的运行状况，它的功能包括2个
1. 监控主数据库和从数据库是否正常工作
2. 主数据库出现故障时自动将从数据库转换为主数据库


**哨兵是一个独立的进程**，使用哨兵的一个典型架构如下图所示。虚线表示主从复制关系，实线表示哨兵的监控路径。

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/back-end-2020/redis-basic-guard-1.png)


在一个一主多从的Redis系统中，可以使用多个哨兵进行监控任务以保证系统是足够稳健的，如下图所示。此时不仅哨兵会同时监控主从数据库，哨兵之间也会互相监控。

![](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/back-end-2020/redis-basic-guard-2.png)


#### 使用哨兵

1. 创建哨兵配置文件，如 `sentinel.conf`

```
# sentinel monitor master-name  ip redis-port quorum

sentinel monitor mymaster 127.0.0.1 6379 1
```
其中，`mymaster` 是要监控的主数据库的名称，可以自定义一个。后面的参数是数据库的IP地址和端口号。

最后的参数 `1` 表示最低通过票数（`quorum`），即执行故障恢复操作前至少需要几个哨兵节点同意。一般情况下，取 `quorum` 的值为 `N/2 + 1`，其中 `N` 表示哨兵数目，即只有超过一半的哨兵同意后才会进行故障恢复。


2. 启动哨兵进程，并将上述配置文件的路径传递给哨兵

```
$redis-sentinel /path/to/sentinel.conf
```

需要注意的是，配置哨兵监控一个系统时，只需要配置其监控主数据库即可，哨兵会自动发现所有复制该主数据库的从数据库。


### 集群

Redis 3.0版本提供了集群（`Cluster`）特性。集群的特点在于拥有和单机实例同样的性能，同时在网络分区后能够提供一定的可访问性以及对主数据库故障恢复的支持。