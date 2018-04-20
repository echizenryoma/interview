## Redis

### 概述

1. 速度快，数据存储在内存中，查找和操作的时间复杂度都是$$O(1)$$
2. 支持丰富数据类型（字符串、散列、列表、集合、有序集合）
3. 支持事务，操作都是原子性
4. 丰富的特性：可用于缓存、消息队列、按主键设置过期时间

### 底层数据结构

1. 简单动态字符串（simple dynamic string，SDS）
2. 链表
3. 字典
4. 跳跃表
5. 整数集合
6. 压缩列表
7. 对象

#### 简单动态字符串

```c
struct sdshdr {
    // buf 中已占用空间的长度
    int len;

    // buf 中剩余可用空间的长度
    int free;

    // 数据空间  
    char buf[];
};
```

#### 链表

```c
struct list{
    //表头节点
    listNode  * head;
    
    //表尾节点
    listNode  * tail;
    
    //链表长度
    unsigned long len;
};
```

#### 字典

##### 哈希表

```c
struct dictht {
   //哈希表数组
   dictEntry **table;
   
   //哈希表大小
   unsigned long size;
   
   //哈希表大小掩码，用于计算索引值
   unsigned long sizemask;
   
   //该哈希表已有节点的数量
   unsigned long used;
}
```

##### 哈希表节点

```c
struct dictEntry{
   //键
   void *key;
   
   //值
   union{
      void *val;
      uint64_t u64;
      int64_t s64;
   }
   
   struct dictEntry *next;
}
```

##### 字典

```c
struct dict {
    // 类型特定函数
    dictType *type;
    
    // 私有数据
    void *privedata;
    
    // 哈希表
    dictht ht[2];
    
    // rehash 索引
    in trehashidx;
}
```

### 分布式

Redis支持主从的模式。

#### 原则

> 主机（Master）会将数据同步到从机（Slave），而从机不会将数据同步到主机。Slave启动时会连接master来同步数据。

这是一个典型的分布式读写分离模型。我们可以利用主机来插入数据，从机提供检索服务。这样可以有效减少单个机器的并发访问数量。

#### 读写分离模型

通过增加Slave DB的数量，读的性能可以线性增长。为了避免Master DB的单点故障，集群一般都会采用两台Master DB做双机热备，所以整个集群的读和写的可用性都非常高。   
读写分离架构的缺陷在于，不管是Master还是Slave，每个节点都必须保存完整的数据，如果在数据量很大的情况下，集群的扩展能力还是受限于单个节点的存储能力，而且对于Write-intensive类型的应用，读写分离架构并不适合。

#### 数据分片模型

为了解决读写分离模型的缺陷，可以将数据分片模型应用进来。

可以将每个节点看成都是独立的Master，然后通过业务实现数据分片。

结合上面两种模型，可以将每个Master设计成由一个Master和多个Slave组成的模型。

### 数据淘汰策略

1. `voltile-lru`从已设置过期时间的数据集（service.db\[i\].expires）中挑选最近最少使用的数据淘汰。
2. `volatile-ttl`从已设置过期时间的数据集（service.db\[i\].expires）中挑选将要过期数据淘汰。
3. `volatile-random`从已设置过期时间的数据集（service.db\[i\].expires）中任意选择数据淘汰。
4. `allkeys-lru`从数据集（service.db\[i\].dict）中挑选最少使用的数据淘汰。
5. `allkeys-random`从数据集（service.db\[i\].dict）中任意选择数据淘汰。
6. `no-enviction`禁止驱逐数据

### 适用场景

* `会话缓存（Session Cache）`最常用就是使用Redis做会话缓存。Redis相比与其他存储的优势在于可持久化。
* `全页缓存（FPC）`除基本的会话Token之外，Redis还提供简便的FPC平台。即使重启了Redis实例，因为有磁盘的持久化，用户也不会看到页面加载速度下降。
* `队列`Redis在内存存储引擎领域的最大一个优点就是提供list和set操作，这使得Redis能做为一个很好的消息队列平台来使用。
* `排行榜/计数器`Redis在内存中对数字进行递增或递减的操作实现的非常好。集合（set）和有序集合（Sorted Set）也使得我们在执行这些操作时变得非常简单，Redis只是正好提供了这两种数据结构。
* `发布/订阅`Redis自带的发布/订阅功能可使用场景非常多。可以在社交网络连接中使用，还可以作为基于发布/订阅的脚本触发器，甚至可以用来建立聊天系统。

**答疑一**：为什么Redis采用单进程单线程？

Redis利用队列技术将并发访问变为串行访问，消除了传统数据库串行控制的开销。
