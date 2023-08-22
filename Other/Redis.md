# Redis速通

## 介绍

### 什么是Redis

Redis（Remote Dictionary Server的缩写）是一种开源的高性能、非关系型（NoSQL）的键值对存储数据库系统。它以内存作为主要存储介质，但也可以通过持久化方式将数据存储到磁盘上。Redis的设计注重速度和简单性，使其成为许多实时应用程序和缓存场景的理想选择。

### 优势和特点

* **速度快**： Redis在内存中进行数据操作，因此具有极快的读写速度。它适用于需要低延迟和高吞吐量的场景。
* **数据结构丰富**： Redis支持多种数据结构，如字符串、哈希、列表、集合、有序集合等，使开发人员能够更灵活地处理数据。
* **持久化支持**： Redis可以将数据持久化到磁盘，以便在重启后恢复数据。支持两种持久化方式：快照（RDB）和追加文件（AOF）。
* **复制和高可用性**： Redis支持主从复制，可以创建多个副本以提高可用性和负载均衡。
* **发布/订阅模式**： Redis支持发布与订阅模式，使应用程序能够实时订阅特定事件。
* **事务支持**： Redis支持简单的事务操作，可以在一个命令中执行多个操作，并保持原子性。
* **Lua脚本**： Redis允许使用Lua脚本进行复杂的数据处理操作。
* **内置缓存**： Redis作为缓存服务器，可以将频繁访问的数据存储在内存中，加速数据访问。

### Redis的内存模型

![Redis内存模型](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90e65a7c724e408f95b731d6af967a58~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.image)

上面图是执行set hello world时所设计到的数据模型；它的执行流程如下

>Redis是键值对数据库，每个键值对都会有一个dictEntry，里面存储着指向Key和Value的指针；next指向下一个dictEntry，与本Key-Value无关。Key和Value又都有相应的存储结构，每种类型都有至少两种内部编码，这样做的好处在于一方面接口与实现分离，当需要增加或改变内部编码时，用户使用不受影响，另一方面可以根据不同的应用场景切换内部编码，提高效率。

## 基本数据类型概念

### 键

Redis是一个键值存储数据库，其中每个键都与一个值相关联。键和值都是字符串，但值可以是不同的数据类型。

```Powershell
redis 127.0.0.1:6379> COMMAND KEY_NAME
```

Redis keys 命令

|序号|命令|描述|
|--|--|--|
|1|DEL key [key ...]|删除一个或多个指定的键。|
|2|EXISTS key|检查键是否存在。|
|3|RENAME key newkey|重命名键。如果newkey已经存在，那么它将被覆盖。|
|4|RENAMENX key newkey|仅当newkey不存在时，才将键重命名为newkey。|
|5|TYPE key|返回键的数据类型，例如字符串、哈希、列表等。|
|6|RANDOMKEY|返回一个随机键。|
|7|SCAN cursor [MATCH pattern] [COUNT count]|逐步迭代遍历键空间，返回匹配模式的键名。cursor表示游标的起始位置，MATCH用于匹配模式，COUNT指定每次迭代返回的键数量。|
|8|TTL key|返回键的剩余生存时间（以秒为单位），-1表示永不过期，-2表示键不存在。|
|9|EXPIRE key seconds|设置键的生存时间（过期时间）。|
|10|PEXPIRE key milliseconds|以毫秒为单位设置键的生存时间。|
|11|PERSIST key|移除键的过期时间，使其变成永久有效。|
|12|MOVE key db|将当前数据库的 key 移动到给定的数据库 db 当中。|
|13|KEYS pattern|查找所有符合给定模式( pattern)的 key 。|
|14|PTTL key|以毫秒为单位返回 key 的剩余的过期时间。|

### 字符串

Redis 字符串数据类型的相关命令用于管理 redis 字符串值

字符串常用命令

TO-DO

### 哈希

Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于**存储对象**。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

```Powershell
127.0.0.1:6379>  HMSET runoobkey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
OK
127.0.0.1:6379>  HGETALL runoobkey
1) "name"
2) "redis tutorial"
3) "description"
4) "redis basic commands for caching"
5) "likes"
6) "20"
7) "visitors"
8) "23000"
```

在以上实例中，我们设置了 redis 的一些描述信息(name, description, likes, visitors) 到哈希表的 `runoobkey` 中。

下表列出了 redis hash 基本的相关命令：

TO-DO

### 列表

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

```Powershell
redis 127.0.0.1:6379> LPUSH runoobkey redis
(integer) 1
redis 127.0.0.1:6379> LPUSH runoobkey mongodb
(integer) 2
redis 127.0.0.1:6379> LPUSH runoobkey mysql
(integer) 3
redis 127.0.0.1:6379> LRANGE runoobkey 0 10

1) "mysql"
2) "mongodb"
3) "redis"
```

在以上实例中我们使用了 LPUSH 将三个值插入了名为 runoobkey 的列表当中。

下表列出了列表相关的基本命令：

### 集合

Redis 的 Set 是 String 类型的无序集合。集合成员是**唯一的**，这就意味着集合中**不能出现重复的数据**。

集合对象的编码可以是 intset 或者 hashtable。

Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

```Powershell
redis 127.0.0.1:6379> SADD runoobkey redis
(integer) 1
redis 127.0.0.1:6379> SADD runoobkey mongodb
(integer) 1
redis 127.0.0.1:6379> SADD runoobkey mysql
(integer) 1
redis 127.0.0.1:6379> SADD runoobkey mysql
(integer) 0
redis 127.0.0.1:6379> SMEMBERS runoobkey

1) "mysql"
2) "mongodb"
3) "redis"
```

在以上实例中我们通过 SADD 命令向名为 runoobkey 的集合插入的三个元素。

### 有序集合

Redis 有序集合和集合一样也是 string 类型元素的集合,且**不允许重复的成员**。

不同的是每个元素都会**关联一个 double 类型的分数**。redis 正是通过分数来为集合中的成员进行**从小到大的排序**。

有序集合的成员是唯一的,但分数(score)却可以重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

```Powershell
redis 127.0.0.1:6379> ZADD runoobkey 1 redis
(integer) 1
redis 127.0.0.1:6379> ZADD runoobkey 2 mongodb
(integer) 1
redis 127.0.0.1:6379> ZADD runoobkey 3 mysql
(integer) 1
redis 127.0.0.1:6379> ZADD runoobkey 3 mysql
(integer) 0
redis 127.0.0.1:6379> ZADD runoobkey 4 mysql
(integer) 0
redis 127.0.0.1:6379> ZRANGE runoobkey 0 10 WITHSCORES

1) "redis"
2) "1"
3) "mongodb"
4) "2"
5) "mysql"
6) "4"
```

### HyperLogLog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

#### 什么是基数

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

```Powershell
redis 127.0.0.1:6379> PFADD runoobkey "redis"

1) (integer) 1

redis 127.0.0.1:6379> PFADD runoobkey "mongodb"

1) (integer) 1

redis 127.0.0.1:6379> PFADD runoobkey "mysql"

1) (integer) 1

redis 127.0.0.1:6379> PFCOUNT runoobkey

(integer) 3
```

## 功能

### 事务和管道pipline

#### 事务的四大特性ACID

* **原子性(Atomicity)A**：事务是数据库的逻辑工作单位，事务中包括的诸操作要么全做，要么全不做。
* **一致性(Consistency)C**:事务执行的结果必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。
* **隔离性(Isolation)I**:一个事务的执行不能被其他事务干扰。
* **持续性/永久性(Durability)D**: 一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。

#### Redis基本指令

简单聊下redis中的事务，先介绍几个redis指令，即MULTI、EXEC、DISCARD、WATCH、UNWATCH。这五个指令构成了redis事务处理的基础。

> 1. multi用来组装提供事务
> 2. exec执行所有事务块内的命令
> 3. discard取消事务，放弃执行事务块
内的所有命令。
> 4. watch监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。
> 5. unwatch取消 watch 命令对所有 key 的监视。

Redis事务可以一次执行多个命令，会经历三个阶段：开始事务，命令入队，执行事务。并且带有以下三个重要的保证：

1. 批量操作在发送 EXEC 命令前被放入队列缓存。
2. 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
3. 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

* 开始事务
* 命令入队
* 执行事务

以下是一个事务的例子， 它先以 MULTI 开始一个事务， 然后将多个命令入队到事务中， 最后由 EXEC 命令触发事务， 一并执行事务中的所有命令：

```Powershell
redis 127.0.0.1:6379> MULTI
OK

redis 127.0.0.1:6379> SET book-name "Mastering C++ in 21 days"
QUEUED

redis 127.0.0.1:6379> GET book-name
QUEUED

redis 127.0.0.1:6379> SADD tag "C++" "Programming" "Mastering Series"
QUEUED

redis 127.0.0.1:6379> SMEMBERS tag
QUEUED

redis 127.0.0.1:6379> EXEC
1) OK
2) "Mastering C++ in 21 days"
3) (integer) 3
4) 1) "Mastering Series"
   2) "C++"
   3) "Programming"
```

单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

#### Redis管道技术

Redis是一种基于客户端-服务端模型以及请求/响应协议的TCP服务。这意味着通常情况下一个请求会遵循以下步骤：

* 客户端向服务端发送一个查询请求，并监听Socket返回，通常是以阻塞模式，等待服务端响应。
* 服务端处理命令，并将结果返回给客户端。

Redis 管道技术可以在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。

```Powershell
$(echo -en "PING\r\n SET runoobkey redis\r\nGET runoobkey\r\nINCR visitor\r\nINCR visitor\r\nINCR visitor\r\n"; sleep 10) | nc localhost 6379

+PONG
+OK
redis
:1
:2
:3
```

以上实例中我们通过使用 PING 命令查看redis服务是否可用， 之后我们设置了 runoobkey 的值为 redis，然后我们获取 runoobkey 的值并使得 visitor 自增 3 次。

在返回的结果中我们可以看到这些命令一次性向 redis 服务提交，并最终一次性读取所有服务端的响应,管道技术最显著的优势是提高了 redis 服务的性能。

### 持久化

数据持久化技术，也是Redis的一大特色。主要作用是**数据的备份**，将内存中的数据持久化到硬盘上，保证数不会因为服务的退出而造成丢失。Redis是内存数据库，我们需要定期的将redis中的数据以某种形式（数据或者命令）存储在硬盘上，当下次redis重启时，利用持久化的技术可以实现数据的恢复。有时为了进行灾难备份，我们也可以将持久化生成的数据文件拷贝到一个远程位置。

Redis持久化分为RDB持久化和AOF持久化：

* 前者将当前数据保存到硬盘，后者则是将每次执行的写命令保存到硬盘（类似于MySQL的binlog）

* 由于AOF持久化的实时性更好，即当进程意外退出时丢失的数据更少，因此AOF是目前主流的持久化方式，不过RDB持久化仍然有其用武之地。

#### RDB持久化

RDB持久化是将当前进程中的数据生成快照保存到硬盘(因此也称作快照持久化)，保存的文件是经过压缩的二进制文件，后缀是rdb；当Redis重新启动时，可以读取快照文件恢复数据。RDB持久化的触发分为手动触发和自动触发两种。

优点：

* 体积小：相同的数据量rdb数据比aof的小，因为rdb是紧凑型文件；
* 恢复快：因为rdb是数据的快照，数据复制，不需要重新执行命令；
* 性能高:父进程在保存rdb时候只需要fork一个子进程，无需父进程的进行其他io操作，也保证了服务器的性能。

缺点：

* 故障丢失:因为rdb是全量的，我们一般是使用shell脚本实现30分钟或者1小时或者每天对redis进行rdb备份，但是最少也要5分钟进行一次的备份，所以当服务死掉后，最少也要丢失5分钟的数据。
* 耐久性差:相对aof的异步策略来说，因为rdb的复制是全量的，即使是fork的子进程来进行备份，当数据量很大的时候对磁盘的消耗也是不可忽视的，尤其在访问量高的时候，fork的时间会延长，导致cpu吃紧，耐久性相对较差。

#### AOF持久化

AOF持久化(即Append Only File持久化)，是将Redis执行的每次写命令记录到单独的日志文件中（有点像MySQL的binlog）；当Redis重启时再次执行AOF文件中的命令来恢复数据。
它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个写操作，并追加到文件中。我们可以设置不同的 fsync 策略，比如无 fsync ，每秒钟一次 fsync ，或者每次执行写入命令时 fsync 。

AOF 的默认策略为每秒钟 fsync 一次，在这种配置下，Redis 仍然可以保持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据（ fsync 会在后台线程执行，所以主线程可以继续努力地处理命令请求）。

优点：

* 数据保证：我们可以设置不同的fsync策略，一般默认是everysec，也可以设置每次写入追加，服务宕机最多丢失一秒数据
* 文件重写：当aof文件大小到达一定程度的时候，后台会自动的去执行aof重写，此过程不会影响主进程，重写完成后，新的写入将会写到新的aof中，旧的就会被删除掉。

缺点：

* 性能相对较差：恢复数据需要重新执行命令，性能较RDB低；
* 体积相对更大：尽管是将aof文件重写了，依然大；
* 恢复速度更慢；

### 发布订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![频道订阅](https://www.runoob.com/wp-content/uploads/2014/11/pubsub1.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![频道发送消息](https://www.runoob.com/wp-content/uploads/2014/11/pubsub2.png)

以下实例演示了发布订阅是如何工作的，需要开启两个 redis-cli 客户端。

在我们实例中我们创建了订阅频道名为 runoobChat:

```Powershell
redis 127.0.0.1:6379> SUBSCRIBE runoobChat

Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "runoobChat"
3) (integer) 1
```

现在，我们先重新开启个 redis 客户端，然后在同一个频道 runoobChat 发布两次消息，订阅者就能接收到消息。

```Powershell
redis 127.0.0.1:6379> PUBLISH runoobChat "Redis PUBLISH test"

(integer) 1

redis 127.0.0.1:6379> PUBLISH runoobChat "Learn redis by runoob.com"

(integer) 1

# 订阅者的客户端会显示如下消息
 1) "message"
2) "runoobChat"
3) "Redis PUBLISH test"
 1) "message"
2) "runoobChat"
3) "Learn redis by runoob.com"
```

下表列出了 redis 发布订阅常用命令：

TO-DO

## 用途和场景

* 缓存
* 会话存储
* 消息队列
* 排行榜和计数器
* 实时分析
* 地理位置定位
* 分布式锁
* 实时消息传递
* 防止重复操作
* 统一配置存储
* 全局计数器
* 应用缓存

## 高可用性和集群

### 主从复制

> * 主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。
> * 前者称为主节点(master)，后者称为从节点(slave)；数据的复制是单向的，只能由主节点到从节点
> * 默认情况下，每台redis服务器都是主节点；且一个主节点可以有多个从节点（或者没有），但一个从节点只有一个主节点 **_(树状)_**

主从复制的作用主要包括：

* 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
* 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
* 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
* 高可用基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。

主从库采用的是读写分离的方式

![读写分离](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/702a5fcc0c1443bab919ce4246f61460~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

主从复制的步骤如下：

分为**全量复制与增量复制**

> * 全量复制：发生在第一次复制时
> * 增量复制：只会把主从库网络断连期间主库收到的命令，同步给从库

创建主从复制的三个阶段

1. 是主从库间建立连接、协商同步的过程

  > 具体来说，从库给主库发送 psync 命令，表示要进行数据同步，主库根据这个命令的参数来启动复制。psync 命令包含了主库的 runID 和复制进度 offset 两个参数。runID，是每个 Redis 实例启动时都会自动生成的一个随机 ID，用来唯一标记这个实例。当从库和主库第一次复制时，因为不知道主库的 runID，所以将 runID 设为“？”。offset，此时设为 -1，表示第一次复制。主库收到 psync 命令后，会用 FULLRESYNC 响应命令带上两个参数：主库 runID 和主库目前的复制进度 offset，返回给从库。从库收到响应后，会记录下这两个参数。这里有个地方需要注意，FULLRESYNC 响应表示第一次复制采用的全量复制，也就是说，主库会把当前所有的数据都复制给从库。

2. 主库将所有数据同步给从库

  > 主库执行 bgsave 命令，生成 RDB 文件，接着将文件发给从库。从库接收到 RDB 文件后，会先清空当前数据库，然后加载 RDB 文件。这是因为从库在通过 replicaof 命令开始和主库同步前，可能保存了其他数据。为了避免之前数据的影响，从库需要先把当前数据库清空。在主库将数据同步给从库的过程中，主库不会被阻塞，仍然可以正常接收请求。否则，Redis 的服务就被中断了。但是，这些请求中的写操作并没有记录到刚刚生成的 RDB 文件中。为了保证主从库的数据一致性，主库会在内存中用专门的 replication buffer，记录 RDB 文件生成后收到的所有写操作

3. 主库会把第二阶段执行过程中新收到的写命令，再发送给从库

  > 具体的操作是，当主库完成 RDB 文件发送后，就会把此时 replication buffer 中的修改操作发给从库，从库再重新执行这些操作。这样一来，主从库就实现同步了。

### 哨兵机制

哨兵的核心功能是主节点的自动故障转移,下图是一个典型的哨兵集群监控的逻辑图

![哨兵机制逻辑](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20cdab22d9164cdb8b2d6b793feaf1c4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

Redis Sentinel包含了若个Sentinel节点，这样做也带来了两个好处：

1. 对于节点的故障判断是由多个Sentinel节点共同完成，这样可以有效地防止误判
2. 即使个别Sentinel节点不可用，整个Sentinel集群依然是可用的

哨兵实现了一下功能

1. **监控**：每个Sentinel节点会对数据节点（Redis master/slave 节点）和其余Sentinel节点进行监控
2. **通知**：Sentinel节点会将故障转移的结果通知给应用方
3. **故障转移**：实现slave晋升为master，并维护后续正确的主从关系
4. **配置中心**：在Redis Sentinel模式中，客户端在初始化的时候连接的是Sentinel节点集合，从中获取主节点信息

其中，监控和自动故障转移功能，使得哨兵可以及时发现主节点故障并完成转移；而配置中心和通知功能，则需要在与客户端的交互中才能体现。

## 性能优化和最佳实践

* 键值对优化；
* 小数据集合的编码优化；
* 使用对象共享池；
* 使用 Bit 比特位或 byte 级别操作
* 使用 hash 类型优化；
* 内存碎片优化；
* 使用 32 位的 Redis。

## 安全性

### 缓存穿透

#### 问题来源

缓存穿透是指缓存和数据库中都没有的数据，而用户不断发起请求。由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。在流量大时，可能DB就挂掉了，要是有人利用不存在的key频繁攻击我们的应用，这就是漏洞。

如发起为id为“-1”的数据或id为特别大不存在的数据。这时的用户很可能是攻击者，攻击会导致数据库压力过大。

#### 解决方案

1. 接口层增加校验，如用户鉴权校验，id做基础校验，id<=0的直接拦截；
2. 从缓存取不到的数据，在数据库中也没有取到，这时也可以将key-value对写为key-null，缓存有效时间可以设置短点，如30秒（设置太长会导致正常情况也没法使用）。这样可以防止攻击用户反复用同一个id暴力攻击。
3. 布隆过滤器。类似于一个hash set，用于快速判某个元素是否存在于集合中，其典型的应用场景就是快速判断一个key是否存在于某容器，不存在就直接返回。布隆过滤器的关键就在于hash算法和容器大小。

### 缓存击穿

* 问题来源

缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期），这时由于并发用户特别多，同时读缓存没读到数据，又同时去数据库去取数据，引起数据库压力瞬间增大，造成过大压力。

* 解决方案

1. 设置热点数据永远不过期。
2. 接口限流与熔断，降级。重要的接口一定要做好限流策略，防止用户恶意刷接口，同时要降级准备，当接口中的某些服务不可用时候，进行熔断，失败快速返回机制。

### 缓存雪崩

* 问题来源

缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，引起数据库压力过大甚至down机。和缓存击穿不同的是，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。

* 解决方案

1. 缓存数据的过期时间设置随机，防止同一时间大量数据过期现象发生。
2. 如果缓存数据库是分布式部署，将热点数据均匀分布在不同的缓存数据库中。
3. 设置热点数据永远不过期
