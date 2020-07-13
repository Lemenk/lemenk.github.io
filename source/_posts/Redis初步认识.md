---
title: Redis初步认识
date: 2019-05-07 14:43:29
tags:
- 数据库
categories:
- Redis
---

**Redis数据库学习笔记**

<!--more-->

# 一、Redis基础

## 1.1 Redis简介

Redis(REmote DIctionary Server) 是一个由Salvatore Sanfilippo写的key-value存储系统。是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## 1.2 NoSQL数据库简介

NoSQL数据库，泛指非关系型数据库。

由于网络的循序发展，传统的关系型数据库对超大规模的数据和高并发的动态网站暴露出了许多问题。

**NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题**。

### 1.2.1 NoSQL数据库的四大分类

**键值([*Key-Value*](https://baike.baidu.com/item/Key-Value))存储数据库**

这一类数据库主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据。Key/value模型对于IT系统来说的优势在于简单、易部署。但是如果DBA只对部分值进行查询或更新的时候，Key/value就显得效率低下了。[3]  举例如：Tokyo Cabinet/Tyrant, **Redis**, Voldemort, Oracle BDB.

**列存储数据库**

这部分数据库通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。这些列是由列家族来安排的。如：Cassandra, HBase, Riak.

**文档型数据库**

文档型数据库的灵感是来自于Lotus Notes办公软件的，而且它同第一种键值存储相类似。该类型的数据模型是版本化的文档，半结构化的文档以特定的格式存储，比如JSON。文档型数据库可 以看作是键值数据库的升级版，允许之间嵌套键值。而且文档型数据库比键值数据库的查询效率更高。如：CouchDB, **MongoDb**. 国内也有文档型数据库SequoiaDB，已经开源。

**图形(Graph)数据库**

图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上。NoSQL数据库没有标准的查询语言(SQL)，因此进行数据库查询需要制定数据模型。许多NoSQL数据库都有REST式的数据接口或者查询API。[2]  如：Neo4J, InfoGrid, Infinite Graph.

**总结**：

NoSQL数据库适用于：

1. 数据模型比较简单；
2. 需要灵活性更强的IT系统；
3. 对数据库性能要求较高；
4. 不需要高度的数据一致性；
5. 对于给定key，比较容易映射复杂值的环境。



## 1.3 Redis的特点

### 1.3.1 特点

1. Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
2. Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
3. Redis支持数据的备份，即master-slave模式的数据备份。

注：key-value分布式存储系统查询速度快、存放数据量大、支持高并发，非常适合通过主键进行查询，但不能进行复杂的条件查询。如果辅以Real-Time Search Engine（实时搜索引擎）进行复杂条件检索、全文检索，就可以替代并发性能较低的MySQL等关系型数据库，达到高并发、高性能，节省几十倍服务器数 量的目的。以MemcacheDB、Tokyo Tyrant为代表的key-value分布式存储，在上万并发连接下，轻松地完成高速查询。而MySQL，在几百个并发连接下，就基本上崩溃了。

### 1.3.2  优势

1. 性能高。Redis能读的速度是110000次/s,写的速度是81000次/s 。
2. 数据类型丰富，支持Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作
3. 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

### 1.3.3 与其他key-value的不同

1. Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
2. Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。



### 1.3.4 Redis的数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

- String（字符串）：它是最基本的数据类型，可以包含任何数据。string类型的值最大能存储512MB。

- Hash（哈希）：它是键值（key-->value）对集合。一般用于存储对象。

  ~~~shell
  127.0.0.1:6379> HMSET first field1 "Hello" field2 "World" 
  OK
  127.0.0.1:6379> HGET first field1
  "Hello"
  127.0.0.1:6379> HGET first field2
  "World"
  ~~~

  使用了 Redis **HMSET, HGET** 命令，**HMSET** 设置了两个 **field=>value** 对, HGET 获取对应 **field** 对应的 **value**。

- List（列表）：是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

  ~~~shell
  127.0.0.1:6379> lpush first redi5
  (integer) 1
  127.0.0.1:6379> lpush first mongodb
  (integer) 2
  127.0.0.1:6379> lpush first rabitmq
  (integer) 3
  127.0.0.1:6379> lpush first 0 10
  (integer) 5
  127.0.0.1:6379> lrange first 0 10
  1) "10"
  2) "0"
  3) "rabitmq"
  4) "mongodb"
  5) "redi5"
  ~~~

  **lpush**用于存储，**lrange**用于取值

- Set（集合）： 是 string 类型的无序集合。

  集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

  ~~~shell
  127.0.0.1:6379> sadd set1 redis
  (integer) 1
  127.0.0.1:6379> sadd set1 mongodv
  (integer) 1
  127.0.0.1:6379> 
  127.0.0.1:6379> sadd set1 rabirmq
  (integer) 1
  127.0.0.1:6379> smembers set1
  1) "rabirmq"
  2) "redis"
  3) "mongodv"
  ~~~

  **sadd 命令**添加一个 string 元素到 key 对应的 set 集合中，成功返回 1，如果元素已经在集合中返回 0。

- zset（有序集合）：也是string类型元素的集合,且不允许重复的成员。

  与set不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

  zset的成员是唯一的,但分数(score)却可以重复。

  ~~~shell
  127.0.0.1:6379> zadd set2 0 redis
  (integer) 1
  127.0.0.1:6379> zadd set2 0 mongodb
  (integer) 1
  127.0.0.1:6379> zadd set2 0 rabitmq
  (integer) 1
  127.0.0.1:6379> zadd set2 0 rabitmq
  (integer) 0
  127.0.0.1:6379> ZRANGEBYSCORE set2 0 100
  1) "mongodb"
  2) "rabitmq"
  3) "redis"
  ~~~

| 类型                 | **简介**                                               | **特性**                                                     | **场景**                                                     |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| String(字符串)       | 二进制安全                                             | 可以包含任何数据,比如jpg图片或者序列化的对象,一个键最大能存储512M |                                                              |
| Hash(字典)           | 键值对集合,即编程语言中的Map类型                       | 适合存储对象,并且可以像数据库中update一个属性一样只修改某一项属性值(Memcached中需要取出整个字符串反序列化成对象修改完再序列化存回去) | 存储、读取、修改用户属性                                     |
| List(列表)           | 链表(双向链表)                                         | 增删快,提供了操作某一段元素的API                             | 1,最新消息排行等功能(比如朋友圈的时间线) 2,消息队列          |
| Set(集合)            | 哈希表实现,元素不重复                                  | 1、添加、删除,查找的复杂度都是O(1) 2、为集合提供了求交集、并集、差集等操作 | 1、共同好友 2、利用唯一性,统计访问网站的所有独立ip 3、好友推荐时,根据tag求交集,大于某个阈值就可以推荐 |
| Sorted Set(有序集合) | 将Set中的元素增加一个权重参数score,元素按score有序排列 | 数据插入集合时,已经进行天然排序                              | 1、排行榜 2、带权重的消息队列                                |



### 1.3.5 用途

1. 缓存，毫无疑问这是Redis当今最为人熟知的使用场景。再提升服务器性能方面非常有效；
2. 排行榜，如果使用传统的关系型数据库来做这个事儿，非常的麻烦，而利用Redis的SortSet数据结构能够非常方便搞定；
3. 计算器/限速器，利用Redis中原子性的自增操作，我们可以统计类似用户点赞数、用户访问数等，这类操作如果用MySQL，频繁的读写会带来相当大的压力；限速器比较典型的使用场景是限制某个用户访问某个API的频率，常用的有抢购时，防止用户疯狂点击带来不必要的压力；
4. 好友关系，利用集合的一些命令，比如求交集、并集、差集等。可以方便搞定一些共同好友、共同爱好之类的功能；
5. 简单消息队列，除了Redis自身的发布/订阅模式，我们也可以利用List来实现一个队列机制，比如：到货通知、邮件发送之类的需求，不需要高可靠，但是会带来非常大的DB压力，完全可以用List来完成异步解耦；
6. Session共享，默认Session是保存在服务器的文件中，即当前服务器，如果是集群服务，同一个用户过来可能落在不同机器上，这就会导致用户频繁登陆；采用Redis保存Session后，无论用户落在那台机器上都能够获取到对应的Session信息。



### 1.3.6 **Redis总结**

- redis单个key 存入512M大小

- redis支持多种类型的数据结构(string,list,hash.set.zset)

- redis 是单线程   原子性    

- redis可以持久化  因为使用了 RDB和AOF机制  

- redis支持集群   而且redis 支持库(0-15) 16个库 

- redis 还可以做消息队列  比如聊天室  IM 

  

  

  企业级开发中:可以用作数据库、缓存(热点数据（经常会被查询，但是不经常被修改或者删除的数据)和消息中间件等大部分功能。

**优点：**  

1. 丰富的数据结构
2. 高速读写，redis使用自己实现的分离器，代码量很短，没有使用lock（MySQL），因此效率非常高。

**缺点：**  

1. 持久化。Redis直接将数据存储到内存中，要将数据保存到磁盘上，Redis可以使用两种方式实现持久化过程。定时快照（snapshot）：每隔一段时间将整个数据库写到磁盘上，每次均是写全部数据，代价非常高。第二种方式基于语句追加（aof）：只追踪变化的数据，但是追加的log可能过大，同时所有的操作均重新执行一遍，回复速度慢。 
2.  耗内存，占用内存过高。 

## 1.4 安装

### 1.4.1 Windows安装

[链接](https://www.runoob.com/redis/redis-install.html)

### 1.4.2 Linux安装

使用Docker安装Rrdis

## 1.5 Redis基础命令

一下命令在Linux的客户端命令行中。启动redis客户端。

切换数据库：

~~~shell
# 切换到第0号数据库
select 0
~~~

1. 查找key

   ~~~shell
   # 查找所有符合指定表达式的key
   127.0.0.1:6379>keys pattern
   
   # 此时为空
   keys *
   (empty list or set)
   # ？为占位符，标识一个字符
   ~~~

2. 删除key

   ~~~shell
   # a为key值，此时redis中不存在此key，则执行结果为0
   127.0.0.1:6379> del a
   (integer) 0
   ~~~

3. 生成key

   ~~~shell
   # key为a，value为123
   127.0.0.1:6379>set a 123
   OK
   127.0.0.1:6379> keys *
   1) "a"
   ~~~

4. 查看指定key序列化的值

   ~~~shell
   127.0.0.1:6379> dump a
   "\x00\xc0{\t\x00\x98\x10\x02-T\xfc\x04\b"
   ~~~

5. 判断key是否存在

   ~~~shell
   127.0.0.1:6379> exists a
   (integer) 1
   127.0.0.1:6379> exists b
   (integer) 0
   ~~~

6. 查看key过期时间

   ~~~shell
   # 返回值为-1，则为永久。
   127.0.0.1:6379> ttl a
   (integer) -1
   ~~~

7. 为指定key设置过期时间，以秒计时

   ~~~shell
   # 设置b的过期时间为10秒
   127.0.0.1:6379> expire b 10
   (integer) 1
   ~~~

   ~~~shell
   # 返回值为-2，则表明已过期
   127.0.0.1:6379>ttl b
   (integer) -2
   
   ~~~

8. 移除key的过期时间，变为永久有效

   ~~~shell
   127.0.0.1:6379>persist b
   (integer) 1
   ~~~

9. 修改key的名称

   ~~~shell
   # 设置key为a的名称为abc
   127.0.0.1:6379> rename a abc
   OK
   ~~~

10. 移动当前数据库中的数据到以他数据库中

    ~~~shell
    # 把c移动到1号数据库中
    move c 1
    ~~~

11. 查看key的类型

    ~~~shell
    # 查看key的类型
    type a
    ~~~

更多命令请参考：https://redis.io/commands

#### 1.5.1 字符串

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **[SET key value](https://www.runoob.com/redis/strings-set.html)** 设置指定 key 的值 |
| 2    | **[GET key](https://www.runoob.com/redis/strings-get.html)** 获取指定 key 的值。 |
| 3    | [GETRANGE key start end](https://www.runoob.com/redis/strings-getrange.html) 返回 key 中字符串值的子字符 |
| 4    | [GETSET key value](https://www.runoob.com/redis/strings-getset.html) 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。 |
| 5    | [GETBIT key offset](https://www.runoob.com/redis/strings-getbit.html) 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。 |
| 6    | [MGET key1 [key2..\]](https://www.runoob.com/redis/strings-mget.html) 获取所有(一个或多个)给定 key 的值。 |
| 7    | [SETBIT key offset value](https://www.runoob.com/redis/strings-setbit.html) 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。 |
| 8    | [SETEX key seconds value](https://www.runoob.com/redis/strings-setex.html) 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| 9    | **[SETNX key value](https://www.runoob.com/redis/strings-setnx.html)** 只有在 key 不存在时设置 key 的值。 |
| 10   | [SETRANGE key offset value](https://www.runoob.com/redis/strings-setrange.html) 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。 |
| 11   | [STRLEN key](https://www.runoob.com/redis/strings-strlen.html) 返回 key 所储存的字符串值的长度。 |
| 12   | [MSET key value [key value ...\]](https://www.runoob.com/redis/strings-mset.html) 同时设置一个或多个 key-value 对。 |
| 13   | [MSETNX key value [key value ...\]](https://www.runoob.com/redis/strings-msetnx.html) 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
| 14   | [PSETEX key milliseconds value](https://www.runoob.com/redis/strings-psetex.html) 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
| 15   | **[INCR key](https://www.runoob.com/redis/strings-incr.html)** 将 key 中储存的数字值增一。 |
| 16   | **[INCRBY key increment](https://www.runoob.com/redis/strings-incrby.html)** 将 key 所储存的值加上给定的增量值（increment） 。 |
| 17   | [INCRBYFLOAT key increment](https://www.runoob.com/redis/strings-incrbyfloat.html) 将 key 所储存的值加上给定的浮点增量值（increment） 。 |
| 18   | **[DECR key](https://www.runoob.com/redis/strings-decr.html)** 将 key 中储存的数字值减一。 |
| 19   | **[DECRBY key decrement](https://www.runoob.com/redis/strings-decrby.htmlkey)** 所储存的值减去给定的减量值（decrement） 。 |
| 20   | **[APPEND key value](https://www.runoob.com/redis/strings-append.html)** 如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。 |

#### 1.5.2  哈希

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [HDEL key field1 field2\]](https://www.runoob.com/redis/hashes-hdel.html) 删除一个或多个哈希表字段 |
| 2    | [HEXISTS key field](https://www.runoob.com/redis/hashes-hexists.html) 查看哈希表 key 中，指定的字段是否存在。 |
| 3    | [HGET key field](https://www.runoob.com/redis/hashes-hget.html) 获取存储在哈希表中指定字段的值。 |
| 4    | [HGETALL key](https://www.runoob.com/redis/hashes-hgetall.html) 获取在哈希表中指定 key 的所有字段和值 |
| 5    | [HINCRBY key field increment](https://www.runoob.com/redis/hashes-hincrby.html) 为哈希表 key 中的指定字段的整数值加上增量 increment 。 |
| 6    | [HINCRBYFLOAT key field increment](https://www.runoob.com/redis/hashes-hincrbyfloat.html) 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。 |
| 7    | [HKEYS key](https://www.runoob.com/redis/hashes-hkeys.html) 获取所有哈希表中的字段 |
| 8    | [HLEN key](https://www.runoob.com/redis/hashes-hlen.html) 获取哈希表中字段的数量 |
| 9    | [HMGET key field1 field2\]](https://www.runoob.com/redis/hashes-hmget.html) 获取所有给定字段的值 |
| 10   | [HMSET key field1 value1 field2 value2 \]](https://www.runoob.com/redis/hashes-hmset.html) 同时将多个 field-value (域-值)对设置到哈希表 key 中。 |
| 11   | [HSET key field value](https://www.runoob.com/redis/hashes-hset.html) 将哈希表 key 中的字段 field 的值设为 value 。 |
| 12   | [HSETNX key field value](https://www.runoob.com/redis/hashes-hsetnx.html) 只有在字段 field 不存在时，设置哈希表字段的值。 |
| 13   | [HVALS key](https://www.runoob.com/redis/hashes-hvals.html) 获取哈希表中所有值 |
| 14   | HSCAN key cursor [MATCH pattern] [COUNT count] 迭代哈希表中的键值对。 |

#### 1.5.3 列表

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [BLPOP key1 key2 \] timeout](https://www.runoob.com/redis/lists-blpop.html) 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 2    | [BRPOP key1 key2 \] timeout](https://www.runoob.com/redis/lists-brpop.html) 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 3    | [BRPOPLPUSH source destination timeout](https://www.runoob.com/redis/lists-brpoplpush.html) 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4    | [LINDEX key index](https://www.runoob.com/redis/lists-lindex.html) 通过索引获取列表中的元素 |
| 5    | [LINSERT key BEFORE\|AFTER pivot value](https://www.runoob.com/redis/lists-linsert.html) 在列表的元素前或者后插入元素 |
| 6    | [LLEN key](https://www.runoob.com/redis/lists-llen.html) 获取列表长度 |
| 7    | [LPOP key](https://www.runoob.com/redis/lists-lpop.html) 移出并获取列表的第一个元素 |
| 8    | [LPUSH key value1 value2\]](https://www.runoob.com/redis/lists-lpush.html) 将一个或多个值插入到列表头部 |
| 9    | [LPUSHX key value](https://www.runoob.com/redis/lists-lpushx.html) 将一个值插入到已存在的列表头部 |
| 10   | [LRANGE key start stop](https://www.runoob.com/redis/lists-lrange.html) 获取列表指定范围内的元素 |
| 11   | [LREM key count value](https://www.runoob.com/redis/lists-lrem.html) 移除列表元素 |
| 12   | [LSET key index value](https://www.runoob.com/redis/lists-lset.html) 通过索引设置列表元素的值 |
| 13   | [LTRIM key start stop](https://www.runoob.com/redis/lists-ltrim.html) 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| 14   | [RPOP key](https://www.runoob.com/redis/lists-rpop.html) 移除列表的最后一个元素，返回值为移除的元素。 |
| 15   | [RPOPLPUSH source destination](https://www.runoob.com/redis/lists-rpoplpush.html) 移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| 16   | [RPUSH key value1 value2\]](https://www.runoob.com/redis/lists-rpush.html) 在列表中添加一个或多个值 |
| 17   | [RPUSHX key value](https://www.runoob.com/redis/lists-rpushx.html) 为已存在的列表添加值 |

#### 1.5.4集合

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [SADD key member1 member2\]](https://www.runoob.com/redis/sets-sadd.html) 向集合添加一个或多个成员 |
| 2    | [SCARD key](https://www.runoob.com/redis/sets-scard.html) 获取集合的成员数 |
| 3    | [SDIFF key1 key2\]](https://www.runoob.com/redis/sets-sdiff.html) 返回给定所有集合的差集 |
| 4    | [SDIFFSTORE destination key1 key2\]](https://www.runoob.com/redis/sets-sdiffstore.html) 返回给定所有集合的差集并存储在 destination 中 |
| 5    | [SINTER key1 key2\]](https://www.runoob.com/redis/sets-sinter.html) 返回给定所有集合的交集 |
| 6    | [SINTERSTORE destination key1 key2\]](https://www.runoob.com/redis/sets-sinterstore.html) 返回给定所有集合的交集并存储在 destination 中 |
| 7    | [SISMEMBER key member](https://www.runoob.com/redis/sets-sismember.html) 判断 member 元素是否是集合 key 的成员 |
| 8    | [SMEMBERS key](https://www.runoob.com/redis/sets-smembers.html) 返回集合中的所有成员 |
| 9    | [SMOVE source destination member](https://www.runoob.com/redis/sets-smove.html) 将 member 元素从 source 集合移动到 destination 集合 |
| 10   | [SPOP key](https://www.runoob.com/redis/sets-spop.html) 移除并返回集合中的一个随机元素 |
| 11   | [SRANDMEMBER key count\]](https://www.runoob.com/redis/sets-srandmember.html) 返回集合中一个或多个随机数 |
| 12   | [SREM key member1 member2\]](https://www.runoob.com/redis/sets-srem.html) 移除集合中一个或多个成员 |
| 13   | [SUNION key1 key2\]](https://www.runoob.com/redis/sets-sunion.html) 返回所有给定集合的并集 |
| 14   | [SUNIONSTORE destination key1 key2\]](https://www.runoob.com/redis/sets-sunionstore.html) 所有给定集合的并集存储在 destination 集合中 |
| 15   | [SSCAN key cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/sets-sscan.html) 迭代集合中的元素 |

#### 1.5.5 有序集合

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [ZADD key score1 member1 score2 member2\]](https://www.runoob.com/redis/sorted-sets-zadd.html) 向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
| 2    | [ZCARD key](https://www.runoob.com/redis/sorted-sets-zcard.html) 获取有序集合的成员数 |
| 3    | [ZCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zcount.html) 计算在有序集合中指定区间分数的成员数 |
| 4    | [ZINCRBY key increment member](https://www.runoob.com/redis/sorted-sets-zincrby.html) 有序集合中对指定成员的分数加上增量 increment |
| 5    | [ZINTERSTORE destination numkeys key key ...\]](https://www.runoob.com/redis/sorted-sets-zinterstore.html) 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| 6    | [ZLEXCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zlexcount.html) 在有序集合中计算指定字典区间内成员数量 |
| 7    | [ZRANGE key start stop WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrange.html) 通过索引区间返回有序集合指定区间内的成员 |
| 8    | [ZRANGEBYLEX key min max LIMIT offset count\]](https://www.runoob.com/redis/sorted-sets-zrangebylex.html) 通过字典区间返回有序集合的成员 |
| 9    | [ZRANGEBYSCORE key min max [WITHSCORES\] [LIMIT]](https://www.runoob.com/redis/sorted-sets-zrangebyscore.html) 通过分数返回有序集合指定区间内的成员 |
| 10   | [ZRANK key member](https://www.runoob.com/redis/sorted-sets-zrank.html) 返回有序集合中指定成员的索引 |
| 11   | [ZREM key member member ...\]](https://www.runoob.com/redis/sorted-sets-zrem.html) 移除有序集合中的一个或多个成员 |
| 12   | [ZREMRANGEBYLEX key min max](https://www.runoob.com/redis/sorted-sets-zremrangebylex.html) 移除有序集合中给定的字典区间的所有成员 |
| 13   | [ZREMRANGEBYRANK key start stop](https://www.runoob.com/redis/sorted-sets-zremrangebyrank.html) 移除有序集合中给定的排名区间的所有成员 |
| 14   | [ZREMRANGEBYSCORE key min max](https://www.runoob.com/redis/sorted-sets-zremrangebyscore.html) 移除有序集合中给定的分数区间的所有成员 |
| 15   | [ZREVRANGE key start stop WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrevrange.html) 返回有序集中指定区间内的成员，通过索引，分数从高到低 |
| 16   | [ZREVRANGEBYSCORE key max min WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrevrangebyscore.html) 返回有序集中指定分数区间内的成员，分数从高到低排序 |
| 17   | [ZREVRANK key member](https://www.runoob.com/redis/sorted-sets-zrevrank.html) 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| 18   | [ZSCORE key member](https://www.runoob.com/redis/sorted-sets-zscore.html) 返回有序集中，成员的分数值 |
| 19   | [ZUNIONSTORE destination numkeys key key ...\]](https://www.runoob.com/redis/sorted-sets-zunionstore.html) 计算给定的一个或多个有序集的并集，并存储在新的 key 中 |
| 20   | [ZSCAN key cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/sorted-sets-zscan.html) 迭代有序集合中的元素（包括元素成员和元素分值） |

# 二 、应用

## 1 Java连接Redis

