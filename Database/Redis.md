# 简介

> [Redis 命令参考 — Redis 命令参考 (redisfans.com)](http://doc.redisfans.com/index.html)

## Redis概述

> Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value 数据库，并提供多种语言的API。
>
> redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

## Redis作用

> - Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
> - Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
> - Redis支持数据的备份，即master-slave模式的数据备份。

## Redis特性

> - 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
> - 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
> - 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
> - 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

# 安装

## Redis安装

**参考博客：**

[Linux安装部署Redis(超级详细) - 长沙大鹏 - 博客园 (cnblogs.com)](https://www.cnblogs.com/hunanzp/p/12304622.html)

`redis/src`:

- redis-benchmark：性能测试工具
- redis-check-aof：修复有问题的AOF文件
- redis-check-dump：修复有问题的dump.rdb文件
- redis-sentinel：Redis集群使用
- **redis-server** ：Redis服务器启动命令
- **redus-cli** ：客户端操作入口

**修改配置文件（先备份）**：修改redis.conf文件中的daemonize，no改成yes，让服务在后台启动。

## Redis启动

- 前台启动：在任何目录下执行`redis-server`(窗口关闭就停止，不推荐)
- 后台启动：在任何目录下执行`redis-server &`
- 启动redis服务时，指定配置文件：`redis-server redis.conf &`

```shell
12580:C 12 Aug 2021 17:20:43.319 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
12580:C 12 Aug 2021 17:20:43.319 # Redis version=6.2.5, bits=64, commit=00000000, modified=0, pid=12580, just started
12580:C 12 Aug 2021 17:20:43.319 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
12580:M 12 Aug 2021 17:20:43.319 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.5 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 12580
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

12580:M 12 Aug 2021 17:20:43.320 # Server initialized
12580:M 12 Aug 2021 17:20:43.320 * Ready to accept connections
```

- 查询是否启动：`ps -ef | grep redis`

```shell
root     12580     1  0 17:20 ?        00:00:00 redis-server *:6379 #redis默认端口号6379
root     12896 12800  0 17:23 pts/1    00:00:00 grep --color=auto redis
```

- `redis-cli`：用客户端访问
  - `auth "2428028346"` 登录 

- `ping`：出现PONG则验证链接成功

## Redis关闭

- 通过kill命令关闭（暴力！容易丢失数据！）：

    - 查询服务端口

  ```shell
  ps -ef |grep redis
  # 得到：12580是主进程 1是副进程
  root     12580     1  0 17:20 ?        00:00:00 redis-server *:6379
  # 这是grep进程本身，无用
  root     12896 12800  0 17:23 pts/1    00:00:00 grep --color=auto redis
  ```

    - kill进程：`kill -9 12580`
    - 再次查询，已经没有redis进程了

- 通过redis命令关闭: `redis-cli shutdown`

# 配置文件

redis.conf配置文件不区分大小写.

```shell
# 默认是在这个目录下 /etc/redis/redis.conf
vim  /etc/redis/redis.conf

# 找到参数  protected-mode 参数更改为no
protected-mode no

# 找到参数 bind 127.0.0.1   设置为	
bind 0.0.0.0
 
# 取消requirepass注释,设置密码
requirepass "123456"
```

开放防火墙端口（CentOS7）

```shell
firewall-cmd --zone=public --add-port=6379/tcp --permanent
firewall-cmd reload
```

## 网络

```bash
bind 127.0.0.1 #允许绑定本机的网卡对应的ip地址访问
protected-mode yes #保护模式，开启后需要bind ip或设置访问密码才能访问服务
port 6379 #端口
```

- **tcp-backlog:**

  设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。

  在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。

  注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值（128），所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果

- **timeout:**

  一个空闲的客户端维持多少秒会关闭，0表示关闭该功能。即永不关闭。

- **tcp-keepalive:**

  对访问客户端的一种心跳检测，每个n秒检测一次。

  单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60

## 通用

```bash
daemonize yes	#是否以守护进程来运行,默认为no，建议开启为yes
pidfile /var/run/redis_6379.pid #如果以后台的方式运行,我们就需要一个pid文件
loglevel notice #日志等级
logfile	#日志的保存路径，有debug、verbose、notice和warning四个级别
databases 16 #默认有16个数据库
always-show-logo no #是否显示logo
```

## 快照

```bash
#如果900秒内,至少有1个 key进行了修改,我们进行持久化操作
save 900 1
#如果300秒内,只要有10个key进行了修改,我们进行持久化操作
save 300 10
#如果60秒内,至少1000个key进行了修改,我们进行持久化操作
save 60 10000
stop-writes-on-bgsave-error yes	
#如果持久化出错,是否还继续工作
rdbcompression yes		
#是否压缩rdb文件,需要消耗cpu资源.
rdbchecksum yes		
#保存rdb文件的时候,进行错误的检查效验
dir ./ 				
#rdb文件保存的目录
```

## 安全

```bash
requirepass 密码 #配置文件设置密码
```

```bash
# 控制台设置
config get requirepass	#获取redis密码
config set requirepass 123	#设置redis密码
auth 123 #当有密码的时候登录时需要密码登录
config set requirepass '' #  取消密码,重启redis服务,重启的时候加载复制的配置文件
```

**密码：dm2190427**

## 内存达到限制值的处理策略

```bash
maxmemory-policy noeviction # 内存达到限制值的处理策略
config set maxmemory-policy volatile-lru #设置为volatile-lru
```

**maxmemory-policy 六种方式：**

**1、volatile-lru：**只对设置了过期时间的key进行LRU（默认值）

**2、allkeys-lru ：** 删除lru算法的key

**3、volatile-random：**随机删除即将过期key

**4、allkeys-random：**随机删除

**5、volatile-ttl ：** 删除即将过期的

**6、noeviction ：** 永不过期，返回错误



# key键操作

- `keys *`：查看当前库中的所有key(匹配：keys *1)

- `exists ket`：判断某个key是否存在

- `type key`：查看key的数据类型

- `del key`：删除指定的key数据

- `unlink key`：根据value选择非阻塞删除（仅将key从keyspace元数据中删除，真正的删除会在后续异步操作中，也就是不会立即删除，会在后续慢慢删除）

- `expire key 10`：为给定的key设置过期时间

- `ttl key`：查看key还有多少秒过期，-1表示永不过期，-2表示已过期

- `select <index>`：切换库，如`select 8`

  Redis默认16个数据库，类似数组下标从0开始，初始默认使用0号库

- `dbsize`查看当前数据库的key的数量

- `flushdb` 清空当前库

- `flushall`通杀所有库

# 常见数据类型

## String

### 简介

String 是最基本的 key-value 结构，key 是唯一标识，value 是具体的值，value其实不仅是字符串， 也可以是数字（整数或浮点数），value 最多可以容纳的数据长度是 `512M`。

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141319613.png)

### 常用命令

|      操作      |                   `命令`                   |                             备注                             |
| :------------: | :----------------------------------------: | :----------------------------------------------------------: |
|  添加/修改值   |            `set <key> <value>`             |              设置键值，相同的key，value会被覆盖              |
|     获取值     |                `get <key>`                 |                            获取值                            |
|     删除值     |                `del <key>`                 |              删除键值对，返回integer 1表示成功               |
|  一次设置多个  | `mset <key1> <value1> <key2> <value2> ...` |                                                              |
|   一次取多个   |          `mget <key1> <key2> ...`          |                                                              |
|  获得值的长度  |               `strlen <key>`               |                                                              |
|     追加值     |           `append <key> <value>`           |               将给定的<value>追加到原值的末尾                |
|      自增      |                `incr <key>`                | 将key中的数字值增加<步长>，默认1 原子性操作<br />`incrby <key> <步长>` |
|      自减      |                `decr <key>`                | 将key中的数字值减少<步长>，默认1 原子性操作<br />`decrby <key> <步长>` |
|  设置存活时间  |    `setex <key> <过期时间(秒)> <value>`    |     携带value的话可以同时设置value<br />`psetex`设置毫秒     |
| 设置不重复键值 |           `setnx <key> <value>`            |               只有在key不存在时，才能设置成功                |

- `msetnx <key1> <value1> <key2> <value2> ...`：

  设置多个key-value，key不重复，一个失败全失败

- `getrang <key> <起始位置> <结束位置>`：

  获得开始位置（包含）到结束位置（包含）的值，索引从0开始

- `setrang <key> <起始位置> <value>`：

  覆写<key>所存储的字符串值，从<起始位置>开始

- `getset <key> <value>`：

  拿到旧值，并设置新值

### 数据结构

Redis 是使用 C 语言编写。但是 Redis 并没有用 char 来表示字符串，而是使用一种称为 **简单动态字符串（Simple Dynamic Strings，SDS）** 来存储字符串和整型数据。

**优点：**

- **高效计算字符串长度，时间复杂度O(1)**

  Redis 定义了一个 `len` 属性，专门用于存储字符串的长度，获取字符串的长度操作的时间复杂度为 `O(1)`，典型的空间换时间。

- **可以保存二进制数据，且二进制安全**

  > 二进制安全：通俗的讲，C 语言中，用 “\0” 表示字符串的结束，如果字符串本身就有 “\0” 字符，字符串就会被截断，即非二进制安全；若通过某种机制，保证读写字符串时不损害其内容，则是二进制安全。

  - 在 SDS 中，使用 `len` 属性而不是空字符串来判断字符串是否结束，确保二进制安全。
  - SDS的所有API都会以处理二进制的方式来处理SDS存放在buf[]数组里的数据，所以SDS不光能存放文本数据，也能保存图片、音频、视频、压缩文件这样的二进制数据

- **Redis 的 SDS API 是安全的，拼接字符串不会造成缓冲区溢出**

  因为 SDS 在拼接字符串之前会检查 SDS 空间是否满足要求，如果空间不够会自动扩容，所以不会导致缓冲区溢出的问题。

### 应用场景

#### 缓存对象

使用 String 来缓存对象有两种方式：

- 直接缓存整个对象的 JSON，命令例子： `SET user:1 '{"name":"xiaolin", "age":18}'`。
- 采用将 key 进行分离为 user:ID:属性，采用 MSET 存储，用 MGET 获取各属性值，命令例子： `MSET user:1:name xiaolin user:1:age 18 user:2:name xiaomei user:2:age 20`。

#### 常规计数

因为 Redis 处理命令是单线程，所以执行命令的过程是原子的。

因此 String 数据类型适合计数场景，比如计算访问次数、点赞、转发、库存数量等等。

比如计算文章的阅读量：

```shell
# 初始化文章的阅读量
> SET aritcle:readcount:1001 0
OK
#阅读量+1
> INCR aritcle:readcount:1001
(integer) 1
#阅读量+1
> INCR aritcle:readcount:1001
(integer) 2
#阅读量+1
> INCR aritcle:readcount:1001
(integer) 3
# 获取对应文章的阅读量
> GET aritcle:readcount:1001
"3"
```

#### 分布式锁

SET 命令有个 NX 参数可以实现「key不存在才插入」，可以用它来实现分布式锁：

- 如果 key 不存在，则显示插入成功，可以用来表示加锁成功；
- 如果 key 存在，则会显示插入失败，可以用来表示加锁失败。

一般而言，还会对分布式锁加上过期时间，分布式锁的命令如下：

```shell
SET lock_key unique_value NX PX 10000
```

- lock_key 就是 key 键；
- unique_value 是客户端生成的唯一的标识；
- NX 代表只在 lock_key 不存在时，才对 lock_key 进行设置操作；
- PX 10000 表示设置 lock_key 的过期时间为 10s，这是为了避免客户端发生异常而无法释放锁。

而解锁的过程就是将 lock_key 键删除，但不能乱删，要保证执行操作的客户端就是加锁的客户端。所以，解锁的时候，我们要先判断锁的 unique_value 是否为加锁客户端，是的话，才将 lock_key 键删除。

可以看到，解锁是有两个操作，这时就需要 Lua 脚本来保证解锁的原子性，因为 Redis 在执行 Lua 脚本时，可以以原子性的方式执行，保证了锁释放操作的原子性。

```lua
// 释放锁时，先比较 unique_value 是否相等，避免锁的误释放
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

这样一来，就通过使用 SET 命令和 Lua 脚本在 Redis 单节点上完成了分布式锁的加锁和解锁。

#### 共享 Session 信息

通常我们在开发后台管理系统时，会使用 Session 来保存用户的会话(登录)状态，这些 Session 信息会被保存在服务器端，但这只适用于单系统应用，如果是分布式系统此模式将不再适用。

例如用户一的 Session 信息被存储在服务器一，但第二次访问时用户一被分配到服务器二，这个时候服务器并没有用户一的 Session 信息，就会出现需要重复登录的问题，问题在于分布式系统每次会把请求随机分配到不同的服务器。

分布式系统单独存储 Session 流程图：

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141110712.png)

因此，我们需要借助 Redis 对这些 Session 信息进行统一的存储和管理，这样无论请求发送到那台服务器，服务器都会去同一个 Redis 获取相关的 Session 信息，这样就解决了分布式系统下 Session 存储的问题。

分布式系统使用同一个 Redis 存储 Session 流程图：

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141110729.png)



## List

### 简介

List 列表是简单的字符串列表，**按照插入顺序排序**，可以从头部或尾部向 List 列表添加元素。

列表的最大长度为 `2^32 - 1`，也即每个列表支持超过 `40 亿`个元素。

它的底层实际上是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

### 常用命令

|       操作       |                   命令                    |                             备注                             |
| :--------------: | :---------------------------------------: | :----------------------------------------------------------: |
|  添加/修改数据   | `lpush/rpush <key> <value1> <value2> ...` |                                                              |
|     弹出数据     |             `lpop/rpop <key>`             | 从左边/右边吐出一个值。数据一旦吐出，原来的列表里就没有了，值在键在，值亡键亡 |
|     获取数据     |       `lrange <key> <start> <stop>`       | 获取start到stop(从左到右)的数据<br />`lrange key 0 -1`获取所有数据 |
| 按照索引获取数据 |          `lindex <key> <index>`           |                按照索引下标获得元素(从左到右)                |
|   获得列表长度   |               `llen <key>`                |                                                              |

- **规定时间内获取并移除数据：**

  - `blpop <key> [timeout]`：从左边弹出并移除数据，不存在则阻塞timeout秒
  - `brpop <key> [timeout]`：从右边弹出并移除数据，不存在则阻塞timeout秒

- `rpoplpush <key1> <key2>`：

  从列表<key1>右边吐出一个值，插到列表<key2>的左边

- `linsert <key> before <value> <newvalue>`：

  在value前面插入newvalue

- `lrem <key> <count> <value>`：

  从左边删除count个value

- `lset <key> <index> <value>`：

  将列表key下标为index的值替换为value

### 数据结构

List 类型的底层数据结构是由**双向链表或压缩列表**实现的：

- 如果列表的元素个数小于 `512` 个（默认值，可由 `list-max-ziplist-entries` 配置），列表每个元素的值都小于 `64` 字节（默认值，可由 `list-max-ziplist-value` 配置），Redis 会使用**压缩列表**作为 List 类型的底层数据结构；
- 如果列表的元素不满足上面的条件，Redis 会使用**双向链表**作为 List 类型的底层数据结构；

**在 Redis 3.2 版本之后，List 数据类型底层数据结构就只由 quicklist 实现了，替代了双向链表和压缩列表**。

也就是将多个ziplist使用双向指针串起来，这样既满足了快速的插入和删除性能，又不会出现太大的空间冗余。

### 应用场景

#### 消息队列

> - 消息保序：使用 LPUSH + RPOP；
> - 阻塞读取：使用 BRPOP；
> - 重复消息处理：生产者自行实现全局唯一 ID；
> - 消息的可靠性：使用 BRPOPLPUSH

消息队列在存取消息时，必须要满足三个需求，分别是：

- **消息保序**

  List 本身就是按先进先出的顺序对数据进行存取的，所以，如果使用 List 作为消息队列保存消息的话，就已经能满足消息保序的需求了。

  List 可以使用 LPUSH + RPOP （或者反过来，RPUSH+LPOP）命令实现消息队列。

  - 生产者使用 `LPUSH key value[value...]` 将消息插入到队列的头部，如果 key 不存在则会创建一个空的队列再插入消息。
  - 消费者使用 `RPOP key` 依次读取队列的消息，先进先出。

  **潜在风险：**生产者生产消息不会通知消费者，消费者需要循环调用RPOP浪费性能；

  **解决方法：**Redis提供了 BRPOP 命令。BRPOP命令也称为阻塞式读取，客户端在没有读到队列数据时，自动阻塞，直到有新的数据写入队列，再开始读取新数据。

  ![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141115203.png)

- **处理重复的消息**

  消费者要实现重复消息的判断，需要 2 个方面的要求：

  - 每个消息都有一个全局的 ID。

  - 消费者要记录已经处理过的消息的 ID。

    当收到一条消息后，消费者程序就可以对比收到的消息 ID 和记录的已处理过的消息 ID，来判断当前收到的消息有没有经过处理。如果已经处理过，那么，消费者程序就不再进行处理了。

  **List 并不会为每个消息生成 ID 号，所以需要自行为每个消息生成一个全局唯一ID**，生成之后，我们在用 LPUSH 命令把消息插入 List 时，需要在消息中包含这个全局唯一ID。

- **保证消息可靠性**

  当消费者程序从 List 中读取一条消息后，List 就不会再留存这条消息了。所以，如果消费者程序在处理消息的过程出现了故障或宕机，就会导致消息没有处理完成，那么，消费者程序再次启动后，就没法再次从 List 中读取消息了。

  为了留存消息，List 类型提供了 `BRPOPLPUSH` 命令，这个命令的**作用是让消费者程序从一个 List 中读取消息，同时，Redis 会把这个消息再插入到另一个 List（可以叫作备份 List）留存**。

  

**List作为消息队列的缺陷：**不支持多个消费者消费同一条消息

## Set

### 简介

**Redis set 是一个无序且唯一的键值集合。**

当需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。

一个集合最多可以存储 `2^32-1` 个元素。概念和数学中个的集合基本类似，可以交集，并集，差集等等，所以 Set 类型除了支持集合内的增删改查，同时还支持多个集合取交集、并集、差集。

Set 类型和 List 类型的区别如下：

- List 可以存储重复元素，Set 只能存储非重复元素；
- List 是按照元素的先后顺序存储元素的，而 Set 则是无序方式存储元素的。

### 常用命令

**Set常用操作：**

- `sadd <key> <value1> <value2> .....`

  将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略

- `smembers <key>`

  取出该集合的所有值。

- `sismember <key> <value>`

  判断集合<key>是否为含有该<value>值，有1，没有0

- `scard<key>`

  返回该集合的元素个数。

- `srem <key> <value1> <value2> ....`

  删除集合中的某个元素。

- `spop <key>`

  随机从该集合中吐出一个值，值吐出就删

- `srandmember <key> <n>`

  随机从该集合中取出n个值。不会从集合中删除 。

**Set运算操作：**

- 交集运算

  ```shell
  # 交集运算
  SINTER key [key ...]
  # 将交集结果存入新集合destination中
  SINTERSTORE destination key [key ...]
  ```

- 并集运算

  ```shell
  # 并集运算
  SUNION key [key ...]
  # 将并集结果存入新集合destination中
  SUNIONSTORE destination key [key ...]
  ```

- 差集运算

  ```shell
  # 差集运算
  SDIFF key [key ...]
  # 将差集结果存入新集合destination中
  SDIFFSTORE destination key [key ...]
  ```

### 数据结构

Set 类型的底层数据结构是由**哈希表或整数集合**实现的：

- 如果集合中的元素都是整数且元素个数小于 `512` （默认值，`set-maxintset-entries`配置）个，Redis 会使用**整数集合**作为 Set 类型的底层数据结构；
- 如果集合中的元素不满足上面条件，则 Redis 使用**哈希表**作为 Set 类型的底层数据结构。

### 应用场景

集合的主要几个特性，无序、不可重复、支持并交差等操作。

因此 Set 类型比较适合用来数据去重和保障数据的唯一性，还可以用来统计多个集合的交集、错集和并集等，当存储的数据是无序并且需要去重的情况下，比较适合使用集合类型进行存储。

**潜在风险：**

**Set 的差集、并集和交集的计算复杂度较高，在数据量较大的情况下，如果直接执行这些计算，会导致 Redis 实例阻塞**。

在主从集群中，为了避免主库因为 Set 做聚合计算（交集、差集、并集）时导致主库被阻塞，我们可以选择一个从库完成聚合统计，或者把数据返回给客户端，由客户端来完成聚合统计

#### 点赞

Set 类型可以保证一个用户只能点一个赞

举例：key是文章id，value是用户id

- `uid:1` 、`uid:2`、`uid:3` 三个用户分别对 article:1 文章点赞

```shell
SADD article:1 uid:1
SADD article:1 uid:2
SADD article:1 uid:3
```

- `uid:1` 取消了对 article:1 文章点赞

```shell
SREM article:1 uid:1
```

- 获取 article:1 文章所有点赞用户

```shell
SMEMBERS article:1
```

- 获取 article:1 文章的点赞用户数量

```
SCARD article:1
```

- 判断用户 `uid:1` 是否对文章 article:1 点赞

```
SISMEMBER article:1 uid:1
```

#### 共同关注

Set 类型支持交集运算，所以可以用来计算共同关注的好友、公众号等。

举例：key 是用户id，value是已关注的公众号的id。

- `uid:1` 用户关注公众号 id 为 5、6、7、8、9

  `uid:2` 用户关注公众号 id 为 7、8、9、10、11

  ```shell
  SADD uid:1 5 6 7 8 9
  SADD uid:1 7 8 9 10 11
  ```

- `uid:1` 和 `uid:2` 共同关注的公众号

  ```shell
  SINTER uid:1 uid:2
  # 运行结果
  1) "7"
  2) "8"
  3) "9"
  ```

- 给 `uid:2` 推荐 `uid:1` 关注的公众号

  ```shell
  SDIFF uid:1 uid:2
  # 运行结果
  1) "5"
  2) "6"
  ```

- 验证某个公众号是否同时被 `uid:1` 或 `uid:2` 关注

  ```shell
  SISMEMBER uid:1 5
  (integer) 1 # 返回0，说明关注了
  
  SISMEMBER uid:2 5
  (integer) 0 # 返回0，说明没关注
  ```

#### 抽奖活动

存储某活动中中奖的用户名 ，Set 类型因为有去重功能，可以保证同一个用户不会中奖两次。

key（`lucky`）为抽奖活动名，value为员工名称

- 把所有员工名称放入抽奖箱 

  ```shell
  SADD lucky Tom Jerry John Sean Marry Lindy Sary Mark
  ```

- 如果允许重复中奖，可以使用 SRANDMEMBER 命令

  ```shell
  # 抽取 1 个一等奖：
  SRANDMEMBER lucky 1
  1) "Tom"
  # 抽取 2 个二等奖：
  SRANDMEMBER lucky 2
  1) "Mark"
  2) "Jerry"
  # 抽取 3 个三等奖：
  SRANDMEMBER lucky 3
  1) "Sary"
  2) "Tom"
  3) "Jerry"
  ```

- 如果不允许重复中奖，可以使用 SPOP 命令

  ```shell
  # 抽取一等奖1个
  SPOP lucky 1
  1) "Sary"
  # 抽取二等奖2个
  SPOP lucky 2
  1) "Jerry"
  2) "Mark"
  # 抽取三等奖3个
  SPOP lucky 3
  1) "John"
  2) "Sean"
  3) "Lindy"
  ```

## Hash

### 简介

Redis hash 是一个键值对集合。

Redis hash是一个string类型的field和value的映射表，特别适合用于存储对象。

类似Java里面的`Map<String,Object>`，一个存储空间(hash)保存多个键值对<filed,value> 

使用hash值获取对应的key-value集合，根据key获取其中的value

用户ID为查找的key，存储的value用户对象包含姓名，年龄，生日等信息，如果用普通的key/value结构来存储：

![](https://gitee.com/dong-dameng/images/raw/master/img/QQ截图20210813093059.png)

**hash类型十分贴近对象的数据存储形式，但不可以滥用**

### 常用命令

|       操作       |             命令             |                      备注                       |
| :--------------: | :--------------------------: | :---------------------------------------------: |
|     添加数据     | `hset <key> <field> <value>` |         给key集合中的 field键赋值value          |
|     获取数据     |     `hget <key> <field>`     |           从key集合取出field对应value           |
|   获取所有数据   |       `hgetall <key>`        | 从key集合取出所有value；内部field过多会影响效率 |
|   获取字段数量   |         `hlen <key>`         |              获取key集合中数据数量              |
| 查看字段是否存在 |   `hexists <key1> <field>`   |    查看哈希表 key 中，给定域 field 是否存在     |
|    获取所有键    |        `hkeys <key>`         |            列出该hash集合的所有field            |
|    获取所有值    |        `hvals <key>`         |            列出该hash集合的所有value            |

- `hmset <key1> <field1> <value1> <field2> <value2>...`

  批量设置hash的值

- `hincrby <key> <field> <increment>`

  为哈希表 key 中的域 field 的值增加步长

- `hdecrby <key> <field> <increment>`

  为哈希表 key 中的域 field 的值减去步长

- `hsetnx <key> <field> <value>`

  将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在

### 数据结构

Hash类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。

- 如果哈希类型元素个数小于 `512` 个（默认值，可由 `hash-max-ziplist-entries` 配置），所有值小于 `64` 字节（默认值，可由 `hash-max-ziplist-value` 配置）的话，Redis 会使用**压缩列表**作为 Hash 类型的底层数据结构；
- 如果哈希类型元素不满足上面条件，Redis 会使用**哈希表**作为 Hash 类型的 底层数据结构。

**在 Redis 7.0 中，压缩列表数据结构已经废弃了，交由 listpack 数据结构来实现了**。

### 应用场景

**缓存对象：**

一般对象用 String + Json 存储，对象中某些频繁变化的属性可以考虑抽出来用 Hash 类型存储。

比如购物车，用户id为key，商品id为field，商品数量为value

```shell
## 添加商品：
HSET cart:{用户id} {商品id} 1
## 添加数量
HINCRBY cart:{用户id} {商品id} 1
## 商品总数：
HLEN cart:{用户id}
## 删除商品：
HDEL cart:{用户id} {商品id}
## 获取购物车所有商品：
HGETALL cart:{用户id}
```

## Zset

### 简介

Zset 类型（有序集合类型）相比于 Set 类型多了一个排序属性 score（分值），对于有序集合 ZSet 来说，每个存储元素相当于有两个值组成的，一个是有序集合的元素值，一个是排序值。

有序集合保留了集合不能有重复成员的特性（分值可以重复），但不同的是，有序集合中的元素可以排序。

### 常用命令

- `ZADD key score member [[score member]...]`

  将一个或多个 member 元素及其 score 值加入到有序集 key 当中。

- `ZREM key member [member...]`

  往有序集合key中删除元素

- `ZSCORE key member`

  返回有序集合key中元素member的分值

- `ZCARD key`

  返回有序集合key中元素个数

- `ZINCRBY key increment member`

  为有序集合key中元素member的分值加上increment

- ```shell
  # 正序获取有序集合key从start下标到stop下标的元素
  ZRANGE key start stop [WITHSCORES]
  # 倒序获取有序集合key从start下标到stop下标的元素
  ZREVRANGE key start stop [WITHSCORES]
  ```

- ```shell
  # 返回有序集合中指定分数区间内的成员，分数由低到高排序。
  ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
  ```

- ```shell
  # 返回指定成员区间内的成员，按字典正序排列, 分数必须相同。
  ZRANGEBYLEX key min max [LIMIT offset count]
  # 返回指定成员区间内的成员，按字典倒序排列, 分数必须相同
  ZREVRANGEBYLEX key max min [LIMIT offset count]
  ```

**ZSet运算操作：**相比Set类型，ZSet类型不支持差集运算

```shell
# 并集计算(相同元素分值相加)，numberkeys:一共多少个key;WEIGHTS:每个key对应的分值乘积
ZUNIONSTORE destkey numberkeys key [key...] 

# 交集计算(相同元素分值相加)，numberkeys一共多少个key，WEIGHTS每个key对应的分值乘积
ZINTERSTORE destkey numberkeys key [key...]
```

### 数据结构

SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构Map<String, Double>
，可以给每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。

Zset 类型的底层数据结构是由**压缩列表或跳表**实现的：

- 如果有序集合的元素个数小于 `128` 个，并且每个元素的值小于 `64` 字节时，Redis 会使用**压缩列表**作为 Zset 类型的底层数据结构；
- 如果有序集合的元素不满足上面的条件，Redis 会使用**跳表**作为 Zset 类型的底层数据结构；

# 新数据类型

## BitMap

### 简介

Bitmap，即位图，是一串连续的二进制数组（0和1），可以通过偏移量（offset）定位元素。BitMap通过最小的单位bit来进行`0|1`的设置，表示某个元素的值或者状态，时间复杂度为O(1)。

由于 bit 是计算机中最小的单位，使用它进行储存将非常节省空间，特别适合一些数据量大且使用**二值统计的场景**。

![image-20210813105348126](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141406829.png)

### 常用命令

**bitmap 基本操作：**

```shell
# 设置值，其中value只能是 0 和 1
SETBIT key offset value
# 获取值
GETBIT key offset
# 获取指定范围内值为 1 的个数
# start 和 end 以字节为单位
BITCOUNT key start end
```

**bitmap 运算操作：**

```shell
# BitMap间的运算
# operations 位移操作符，枚举值
  AND 与运算 &
  OR 或运算 |
  XOR 异或 ^
  NOT 取反 ~
# result 计算的结果，会存储在该key中
# key1 … keyn 参与运算的key，可以有多个，空格分割，not运算只能一个key
# 当 BITOP 处理不同长度的字符串时，较短的那个字符串所缺少的部分会被看作0。返回值是保存到 destkey 的字符串的长度（以字节byte为单位），和输入 key 中最长的字符串长度相等。

BITOP [operations] [result] [key1] [keyn…]

# 返回指定key中第一次出现指定value(0/1)的位置
BITPOS [key] [value]
```

### 数据结构

Bitmap 本身是用 String 类型作为底层数据结构实现的一种统计二值状态的数据类型。

String 类型是会保存为二进制的字节数组，所以，Redis 就把字节数组的每个 bit 位利用起来，用来表示一个元素的二值状态。可以把 Bitmap 看作是一个 bit 数组。

### 应用场景

Bitmap 类型非常适合二值状态统计的场景，这里的二值状态就是指集合元素的取值就只有 0 和 1 两种，在记录海量数据时，Bitmap 能够有效地节省内存空间。

签到统计时，每个用户一天的签到用 1 个 bit 位就能表示，一个月（假设是 31 天）的签到情况用 31 个 bit 位，而一年的签到也只需要用 365 个 bit 位，根本不用太复杂的集合类型。

**统计签到情况**

假设要统计 ID 100 的用户在 2022 年 6 月份的签到情况，就可以按照下面的步骤进行操作。

1. 记录该用户6月3号已签到

   `SETBIT uid:sign:100:202206 2 1`

2. 检查该用户 6 月 3 日是否签到。

   `GETBIT uid:sign:100:202206 2 `

3. 统计该用户在 6 月份的签到次数

   `BITCOUNT uid:sign:100:202206`

**统计这个月的首次打卡时间：**

> Redis 提供了 `BITPOS key bitValue [start] [end]`指令，返回数据表示 Bitmap 中第一个值为 `bitValue` 的 offset 位置。
>
> 在默认情况下， 命令将检测整个位图， 用户可以通过可选的 `start` 参数和 `end` 参数指定要检测的范围。

获取 userID = 100 在 2022 年 6 月份**首次打卡**日期：

`BITPOS uid:sign:100:202206 1`

因为 offset 从 0 开始的，所以我们需要将返回的 value + 1



**判断用户登录态**

> Bitmap 提供了 `GETBIT、SETBIT` 操作，通过一个偏移值 offset 对 bit 数组的 offset 位置的 bit 位进行读写操作，需要注意的是 offset 从 0 开始。
>
> 只需要一个 key = login_status 表示存储用户登陆状态集合数据， 将用户 ID 作为 offset，在线就设置为 1，下线设置 0。通过 `GETBIT`判断对应的用户是否在线。 
>
> 5000 万用户只需要 6MB 的空间。

1. 执行以下命令，表示用户已登录

   `SETBIT login_status 10086 1`

2. 检查用户是否登录，返回值1表示已登录

   `GETBIT login_status 10086`

3. 登出，将offset对应的value设置成0

   `SETBIT login_status 10086 0`



**连续签到用户总数：**

> 如何统计出这连续 7 天连续打卡用户总数呢？
>
> - 以每天的日期为key，userId为offerset，若是打卡则将offset位置的bit设置成1
>
>   连续七天，则这样的Bitmap要有7个
>
> - 相同userId对应相同的offset，对这7个集合进行进行【与】运算，并将结果保存到一个新的Bitmap中
>
>   如果新集合中对应offset位置的bit为1，则说明该offset对应的用户连续七天打卡
>
> - 通过 `BITCOUNT` 统计 bit = 1 的个数便得到了连续打卡 7 天的用户总数
>
> Redis 提供了 `BITOP operation destkey key [key ...]`这个指令用于对一个或者多个 key 的 Bitmap 进行位元操作。
>
> - `operation` 可以是 `and`、`OR`、`NOT`、`XOR`。
>
>   当 BITOP 处理不同长度的字符串时，较短的那个字符串所缺少的部分会被看作 `0` 。空的 `key` 也被看作是包含 `0` 的字符串序列

假设要统计 3 天连续打卡的用户数，则是将三个 bitmap 进行 AND 操作，并将结果保存到 destmap 中，接着对 destmap 执行 BITCOUNT 统计，如下命令：

```shell
# 与操作
BITOP AND destmap bitmap:01 bitmap:02 bitmap:03
# 统计 bit 位 =  1 的个数
BITCOUNT destmap
```

即使一天产生一个亿的数据，Bitmap占用的内存也不大，大约占12MB的内存（10^8/8/1024/1024），7天的Bitmap的内存开销约为84MB。

同时最好给Bitmap设置过期时间，让Redis删除过期的打卡数据，节省内存。

## HyperLogLog

### 简介

Redis HyperLogLog 是 Redis 2.8.9 版本新增的数据类型，是一种用于「统计基数」的数据集合类型，基数统计就是指统计一个集合中不重复的元素个数。但要注意，HyperLogLog 是统计规则是基于概率完成的，不是非常准确，标准误算率是 0.81%。

所以，简单来说 HyperLogLog **提供不精确的去重计数**。

HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的内存空间总是固定的、并且是很小的。

在 Redis 里面，**每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 `2^64` 个不同元素的基数**，和元素越多就越耗费内存的 Set 和 Hash 类型相比，HyperLogLog 就非常节省空间。

> **什么是基数?**
>
> 比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}。

基数估计就是在误差可接受的范围内，快速计算基数。

### 常用命令

```shell
# 添加指定元素到 HyperLogLog 中
PFADD key element [element ...]

# 返回给定 HyperLogLog 的基数估算值。
PFCOUNT key [key ...]

# 将多个 HyperLogLog 合并为一个 HyperLogLog
PFMERGE destkey sourcekey [sourcekey ...]
```

### 数据结构

费脑子：

https://en.wikipedia.org/wiki/HyperLogLog

### 应用场景

**百万级网页UV计数：**

Redis HyperLogLog 优势在于只需要花费 12 KB 内存，就可以计算接近 2^64 个元素的基数，和元素越多就越耗费内存的 Set 和 Hash 类型相比，HyperLogLog 就非常节省空间。

所以，非常适合统计百万级以上的网页 UV 的场景。

- 在统计 UV 时，可以用 PFADD 命令（用于向 HyperLogLog 中添加新元素）把访问页面的每个用户都添加到 HyperLogLog 中。

  `PFADD page1:uv user1 user2 user3 user4 user5`

- 接下来，就可以用 PFCOUNT 命令直接获得 page1 的 UV 值了，这个命令的作用就是返回 HyperLogLog 的统计结果。

  `PFCOUNT page1:uv`

## Geo

### 简介

Redis GEO 是 Redis 3.2 版本新增的数据类型，主要用于存储地理位置信息，并对存储的信息进行操作。

该类型，就是元素的2维坐标，在地图上就是经纬度。redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作。

### 常用命令

```shell
# 存储指定的地理空间位置，可以将一个或多个经度(longitude)、纬度(latitude)、位置名称(member)添加到指定的 key 中。
GEOADD key longitude latitude member [longitude latitude member ...]

# 从给定的 key 里返回所有指定名称(member)的位置（经度和纬度），不存在的返回 nil。
GEOPOS key member [member ...]

# 返回两个给定位置之间的距离。
GEODIST key member1 member2 [m|km|ft|mi]

# 根据用户给定的经纬度坐标来获取指定范围内的地理位置集合。
GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
```

### 数据结构

GEO 本身并没有设计新的底层数据结构，而是直接使用了 Sorted Set 集合类型。

GEO 类型使用 GeoHash 编码方法实现了经纬度到 Sorted Set 中元素权重分数的转换，这其中的两个关键机制就是「对二维地图做区间划分」和「对区间进行编码」。一组经纬度落在某个区间后，就用区间的编码值来表示，并把编码值作为 Sorted Set 元素的权重分数。

这样一来，就可以把经纬度保存到 Sorted Set 中，利用 Sorted Set 提供的“按权重进行有序范围查找”的特性，实现 LBS 服务中频繁使用的“搜索附近”的需求。

### 应用场景

以滴滴叫车场景为例

假设车辆 ID 是 33，经纬度位置是（116.034579，39.030452），可以用一个 GEO 集合保存所有车辆的经纬度，集合 key 是 cars:locations。

执行下面的这个命令，就可以把 ID 号为 33 的车辆的当前经纬度位置存入 GEO 集合中：

```shell
GEOADD cars:locations 116.034579 39.030452 33
```

当用户想要寻找自己附近的网约车时，LBS 应用就可以使用 GEORADIUS 命令。

例如，LBS 应用执行下面的命令时，Redis 会根据输入的用户的经纬度信息（116.054579，39.030452 ），查找以这个经纬度为中心的 5 公里内的车辆信息，并返回给 LBS 应用。

```shell
GEORADIUS cars:locations 116.054579 39.030
```

## Stream

### 简介

Redis Stream 是 Redis 5.0 版本新增加的数据类型，Redis 专门为消息队列设计的数据类型。

在 Redis 5.0 Stream 没出来之前，消息队列的实现方式都有着各自的缺陷，例如：

- 发布订阅模式，不能持久化也就无法可靠的保存消息，并且对于离线重连的客户端有不能读取历史消息的缺陷；
- List 实现消息队列的方式不能重复消费，一个消息消费完就会被删除，而且生产者需要自行实现全局唯一 ID。

基于以上问题，Redis 5.0 便推出了 Stream 类型也是此版本最重要的功能，用于完美地实现消息队列，它支持消息的持久化、支持自动生成全局唯一 ID、支持 ack 确认消息的模式、支持消费组模式等，让消息队列更加的稳定和可靠。

### 常用命令

Stream 消息队列操作命令：

- `XADD`：插入消息，保证有序，可以自动生成全局唯一 ID；
- `XLEN` ：查询消息长度；
- `XREAD`：用于读取消息，可以按 ID 读取数据；
- `XDEL` ： 根据消息 ID 删除消息；
- `DEL` ：删除整个 Stream；
- `XRANGE` ：读取区间消息
- `XREADGROUP`：按消费组形式读取消息；
- `XPENDING` 和 `XACK`：
  - `XPENDING` 命令可以用来查询每个消费组内所有消费者「已读取、但尚未确认」的消息；
  - `XACK` 命令用于向消息队列确认消息处理已完成；

### 应用场景

> - 消息保序：XADD/XREAD
> - 阻塞读取：XREAD block
> - 重复消息处理：Stream 在使用 XADD 命令，会自动生成全局唯一 ID；
> - 消息可靠性：内部使用 PENDING List 自动保存消息，使用 XPENDING 命令查看消费组已经读取但是未被确认的消息，消费者使用 XACK 确认消息；
> - 支持消费组形式消费数据

#### 消息队列

> Stream 的基础方法，使用 xadd 存入消息和 xread 循环阻塞读取消息的方式可以实现简易版的消息队列，交互流程如下图所示：
>
> ![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306141451162.png)

- 生产者通过 XADD 命令插入一条消息：

  ```shell
  # * 表示让 Redis 为插入的数据自动生成一个全局唯一的 ID
  # 往名称为 mymq 的消息队列中插入一条消息，消息的键是 name，值是 xiaolin
  > XADD mymq * name xiaolin
  "1654254953808-0"
  ```

  插入成功后会返回全局唯一的 ID："1654254953808-0"。

  消息的全局唯一 ID 由两部分组成：

  - 第一部分“1654254953808”是数据插入时，以毫秒为单位计算的当前服务器时间；
  - 第二部分表示插入消息在当前毫秒内的消息序号，这是从 0 开始编号的。例如，“1654254953808-0”就表示在“1654254953808”毫秒内的第 1 条消息

- 消费者通过 XREAD 命令从消息队列中读取消息时，可以指定一个消息 ID，并从这个消息 ID 的下一条消息开始进行读取（注意是输入消息 ID 的下一条信息开始读取，不是查询输入ID的消息）。

  ```shell
  # 从 ID 号为 1654254953807-0 的消息开始，读取后续的所有消息（示例中一共 1 条）。
  > XREAD STREAMS mymq 1654254953807-0
  1) 1) "mymq"
     2) 1) 1) "1654254953808-0"
           2) 1) "name"
              2) "xiaolin"
  ```

  如果**想要实现阻塞读（当没有数据时，阻塞住），可以调用 XRAED 时设定 BLOCK 配置项**，实现类似于 BRPOP 的阻塞读取操作。

  比如，下面这命令，设置了 BLOCK 10000 的配置项，10000 的单位是毫秒，表明 XREAD 在读取最新消息时，如果没有消息到来，XREAD 将阻塞 10000 毫秒（即 10 秒），然后再返回。

  ```shell
  # 命令最后的“$”符号表示读取最新的消息
  > XREAD BLOCK 10000 STREAMS mymq $
  (nil)
  (10.00s)
  ```

  

#### 消费组消费

Stream 可以以使用 XGROUP 创建消费组，创建消费组之后，Stream 可以使用 XREADGROUP 命令让消费组内的消费者读取消息。

- 创建两个消费组，这两个消费组消费的消息队列是 mymq，都指定从第一条消息开始读取：

  ```shell
  # 创建一个名为 group1 的消费组，0-0 表示从第一条消息开始读取。
  > XGROUP CREATE mymq group1 0-0
  OK
  # 创建一个名为 group2 的消费组，0-0 表示从第一条消息开始读取。
  > XGROUP CREATE mymq group2 0-0
  OK
  ```

- 消费组 group1 内的消费者 consumer1 从 mymq 消息队列中读取所有消息的命令如下：

  ```shell
  # 命令最后的参数“>”，表示从第一条尚未被消费的消息开始读取。
  > XREADGROUP GROUP group1 consumer1 STREAMS mymq >
  1) 1) "mymq"
     2) 1) 1) "1654254953808-0"
           2) 1) "name"
              2) "xiaolin"
  ```

  > **消息队列中的消息一旦被消费组里的一个消费者读取了，就不能再被该消费组内的其他消费者读取了，即同一个消费组里的消费者不能消费同一条消息**。
  >
  > 但是，**不同消费组的消费者可以消费同一条消息（但是有前提条件，创建消息组的时候，不同消费组指定了相同位置开始读取消息）**。

- 消费组 group2 内的消费者 consumer1 消费信息

  ```
  > XREADGROUP GROUP group2 consumer1 STREAMS mymq >
  1) 1) "mymq"
     2) 1) 1) "1654254953808-0"
           2) 1) "name"
              2) "xiaolin"
  ```

> 使用消费组的目的是让组内的多个消费者共同分担读取消息，所以，通常会让每个消费者读取部分消息，从而实现消息读取负载在多个消费者间是均衡分布的。

- 执行下列命令，让 group2 中的 consumer1、2、3 各自读取一条消息

  ```shell
  # 让 group2 中的 consumer1 从 mymq 消息队列中消费一条消息
  > XREADGROUP GROUP group2 consumer1 COUNT 1 STREAMS mymq >
  1) 1) "mymq"
     2) 1) 1) "1654254953808-0"
           2) 1) "name"
              2) "xiaolin"
  # 让 group2 中的 consumer2 从 mymq 消息队列中消费一条消息
  > XREADGROUP GROUP group2 consumer2 COUNT 1 STREAMS mymq >
  1) 1) "mymq"
     2) 1) 1) "1654256265584-0"
           2) 1) "name"
              2) "xiaolincoding"
  # 让 group2 中的 consumer3 从 mymq 消息队列中消费一条消息
  > XREADGROUP GROUP group2 consumer3 COUNT 1 STREAMS mymq >
  1) 1) "mymq"
     2) 1) 1) "1654256271337-0"
           2) 1) "name"
              2) "Tom"
  ```

#### 宕机恢复

> 基于 Stream 实现的消息队列，如何保证消费者在发生故障或宕机再次重启后，仍然可以读取未处理完的消息？

Streams 会自动使用内部队列（也称为 PENDING List）留存消费组里每个消费者读取的消息，直到消费者使用 XACK 命令通知 Streams“消息已经处理完成”。

消费确认增加了消息的可靠性，一般在业务处理完成之后，需要执行 XACK 命令确认消息已经被消费完成，整个流程的执行如下图所示：

![img](https://cdn.xiaolincoding.com/gh/xiaolincoder/redis/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B/%E6%B6%88%E6%81%AF%E7%A1%AE%E8%AE%A4.png)

如果消费者没有成功处理消息，它就不会给 Streams 发送 XACK 命令，消息仍然会留存。此时，**消费者可以在重启后，用 XPENDING 命令查看已读取、但尚未确认处理完成的消息**。

# 底层数据结构

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306151330770.jpeg)

## 全局哈希表

为了实现从键到值的快速访问，Redis使用了一个哈希表来保存所有键值对，从而在查找时实现O(1)的时间复杂度。

**哈希桶中的元素保存的不是值本身，而是指向具体值的指针。**也就是说，不管值是String还是其他集合类型，哈希桶中的元素都是指向他们的指针。

哈希冲突采用拉链法解决。

![img](https://shuaidi-picture-1257337429.cos.ap-guangzhou.myqcloud.com/img/1cc8eaed5d1ca4e3cdbaa5a3d48dfb5f.jpg)

## RedisObject

Redis的数据类型有很多，并且不同的数据类型有相同的元数据要记录（比如最后一次访问的时间、被引用的次数等），Redis会用一个RedisObject结构体来统一记录一些元数据，同时指向实际数据。

一个RedisObject包含了8字节的元数据和一个8字节的指针域，指针域指向具体数据类型的实际数据所在。

- **元数据**

  - 值对象的数据类型
  - 值对象的底层编码类型
  - 最后一次使用此对象的时间等信息
  - 对象的引用次数

- **指针域**

  数据存放的实际地址

Redis 是 key-value 存储系统，其中key类型一般为字符串，value 类型则为RedisObject对象

Redis数据类型就是采用不同的数据类型，组织value，也就是组织RedisObject对象。

## SDS

在String数据类型中，采用了SDS（简单动态字符串）的方式保存数据

SDS包括三部分：

- **buf：**字节数组，保存实际数据。Redis会自动在数组最后加一个"\0"，表示字节数组的结束（额外占用1个字节的开销）
- **len：**占用4个字节，表示buf的已用长度
- **alloc：**占4个字节，表示buf的实际分配长度，一般大于len

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306151315856.jpeg)

- **int编码**：64位有符号整数，此时数据直接保存在RedisObject的指针域中。
- **embstr编码**：当保存的是字符串数据，并且字符串小于等于44字节时，RedisObject中的元数据、指针域和SDS是保存在一块连续的内存区域。
- **raw编码**：当字符串大于44字节时，Redis会给SDS分配独立的空间，并用指针指向SDS结构。

**优点：**

- **高效计算字符串长度，时间复杂度O(1)**
- **可以保存二进制数据，且二进制安全**
- **Redis 的 SDS API 是安全的，拼接字符串不会造成缓冲区溢出**

## ZipList

**压缩列表（Ziplist）**

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306151343855.jpeg)

- **表头：**

  - zlbytes：列表长度

  - zltail：列表尾的偏移量

  - zllen：列表中的entry个数

- **Entry：**

  - 

- **表尾：zlend，表示列表结束**



# 发布和订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

 1.客户端可以订阅频道如下图

![image-20210814092714133](https://gitee.com/dong-dameng/images/raw/master/img/20210814092714.png)

 2.当给这个频道发布消息后，消息就会发送给订阅的客户端

![image-20210814092747618](https://gitee.com/dong-dameng/images/raw/master/img/20210814092747.png)

**实现：**

1、 打开一个客户端订阅channel1

SUBSCRIBE channel1

​         ![image-20210814092527278](https://gitee.com/dong-dameng/images/raw/master/img/20210814092527.png)

2、打开另一个客户端，给channel1发布消息hello

publish channel1 hello

![image-20210814092540722](https://gitee.com/dong-dameng/images/raw/master/img/20210814092540.png)

返回的1是订阅者数量

3、打开第一个客户端可以看到发送的消息

![image-20210814092558449](https://gitee.com/dong-dameng/images/raw/master/img/20210814092558.png)

注：发布的消息没有持久化，如果在订阅的客户端收不到hello，只能收到订阅后发布的消息



# Jedis

## 引入依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
```

## 测试连接

```java
public class JedisDemo {
    public static void main(String[] args) {
        // 创建Jedis对象
        Jedis jedis = new Jedis("106.15.106.110", 6379);
        // 测试
        jedis.auth("bPm7BcpeWNqXbr7i");
        String ping = jedis.ping();
        jedis.set("name1","lucy");
        jedis.set("name2","make");
        String name1 = jedis.get("name1");
        System.out.println(name1);
        System.out.println(ping);
        jedis.close();
    }
}
```

结果：

```shell
lucy
PONG
```

**Jedis提供了丰富的API，和Redis命令一一对应，其他Redis操作都封装进Jedis中了！**

## 案例：验证码

需求分析：

1. 输入手机号，点击发送后随机生成6位数字码，2分钟有效
2. 输入验证码，点击验证，返回成功或失败
3. 每个手机号每天只能输入3次

流程：

1. 生成随机6位数字验证码：Random
2. 验证码两分钟内有效：把验证码放到redis里，设置过期时间120秒
3. 判断验证码是否一致：从redis获取验证码和输入的验证码进行比较
4. 每个手机每天只能发送3次验证码：用incr，每次发送后+1，大于2时提交不能再发送

代码简单实现：

```java
/**
 * @author Chongming
 * @description
 * @date 2021年08月13日 15:45
 */
public class JedisDemo {
    public static void main(String[] args) {
       // 模拟验证码发送
        String phone = "16635830582";
        String code = verifyCode(phone);
        if (code == null) {
            System.out.println("结束!");
            System.exit(0);
        }
        checkCode(phone,code);
    }

    /**
     * 生成6位数字验证码
     * @return
     */
    public static String getCode() {
        Random random = new Random();
        StringBuffer code = new StringBuffer();
        for (int i = 0; i < 6; i++) {
            code.append(random.nextInt(10));
        }
        return code.toString();
    }

    /**
     * 手机每天只能发送三次，验证码放到redis里，设置过期时间
     */
    public static String verifyCode(String phone) {
        // 连接Jedis对象
        Jedis jedis = new Jedis("106.15.106.110", 6379);
        jedis.auth("bPm7BcpeWNqXbr7i");

        // 手机发送次数key
        String countKey = "VerifyCode" + phone + ":count";
        // 验证码key
        String codeKey = "VerifyCode" + phone + ":code";

        // 每个手机每天只能发送三次
        String count = jedis.get(countKey);
        if (count == null) {
            // 没有发送次数，第一次发送
            // 设置发送次数是1
            jedis.setex(countKey,24*60*60,"1");
        } else if (Integer.parseInt(count) <= 2 ) {
            jedis.incr(countKey);
        } else {
            System.out.println("今天已经发送三次，不能再发送了");
            jedis.close();
            return null;
        }

        // 将发送的验证码放入redis中
        String vcode = getCode();
        jedis.setex(codeKey, 60*2, vcode);
        jedis.close();
        return vcode;
    }

    /**
     * 校验验证码
     * @param phone
     * @param code
     */
    public static void checkCode(String phone, String code) {
        // 连接Jedis对象
        Jedis jedis = new Jedis("106.15.106.110", 6379);
        jedis.auth("xxxxxx");
        // 手机发送次数key
        String countKey = "VerifyCode" + phone + ":count";
        // 验证码key
        String codeKey = "VerifyCode" + phone + ":code";
        // 从redis中获取验证码
        String redisCode = jedis.get(codeKey);
        if (redisCode.equals(code)) {
            System.out.println("成功");
        } else {
            System.out.println("失败");
        }
        jedis.close();
    }

    @Test
    public void test() {
        Jedis jedis = new Jedis("106.15.106.110", 6379);
        jedis.auth("xxxxxx");
        jedis.flushAll();
        jedis.close();
    }
}
```

执行四次结果：

```shell
成功
成功
成功
失败
今天已经发送三次，不能再发送了
```

# SpringBoot整合

## 引入依赖

```xml
<!-- redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!-- spring2.X集成redis所需common-pool2-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.6.0</version>
</dependency>
```

## 编写配置文件

application.properties中配置redis配置

```properties
#Redis服务器地址
spring.redis.host=106.15.106.110
#Redis服务器连接端口
spring.redis.port=6379
#Redis服务访问密码
password=2428028346
#Redis数据库索引（默认为0）
spring.redis.database= 0
#连接超时时间（毫秒）
spring.redis.timeout=1800000
#连接池最大连接数（使用负值表示没有限制）
spring.redis.lettuce.pool.max-active=20
#最大阻塞等待时间(负数表示没限制)
spring.redis.lettuce.pool.max-wait=-1
#连接池中的最大空闲连接
spring.redis.lettuce.pool.max-idle=5
#连接池中的最小空闲连接
spring.redis.lettuce.pool.min-idle=0
```

application.yml版配置

```yaml
spring:
  redis:
    host: 106.15.106.110
    port: 6379
    password: bPm7BcpeWNqXbr7i
    database: 0
    timeout: 1800000
    lettuce:
      pool:
        max-active: 20
        max-wait: 1
        max-idle: 5
        min-idle: 0
```

## 编写配置类

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance ,
                ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.PROPERTY);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
        //key序列化方式
        template.setKeySerializer(redisSerializer);
        //value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
        //value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance ,
                ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.PROPERTY);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```

## 测试

```java
@RestController
@RequestMapping("/redisTest")
public class RedisTestController {
    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping
    public String testRedis() {
        //设置值到redis
        redisTemplate.opsForValue().set("name","lucy");
        //从redis获取值
        String name = (String)redisTemplate.opsForValue().get("name");
        return name;
    }
}
```

# 持久化

> **什么是持久化**
>
> 利用永久性存储介质将数据进行保存，在特定的时间将保存的数据进行恢复的工作机制称为持久化。
>
> **为什么要持久化**
>
> 防止数据的意外丢失，确保数据安全性
>
> **持久化过程保存什么**
>
> - 将当前数据状态进行保存，快照形式，存储数据结果，存储格式简单，关注点在数据（RDB）
> - 将数据的操作过程进行保存，日志形式，存储操作过程，存储格式复杂，关注点在数据的操作过程(AOF)

## RDB





# 事务和锁

## 事务的定义

> Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
>
> Redis事务的主要作用就是串联多个命令防止别的命令插队。

## 事务流程

**流程：**

- Multi
- Exec
- discard

从输入Multi命令开始，输入的命令都会依次进入命令队列中，但不会执行，直到输入Exec后，Redis会将之前的命令队列中的命令依次执行。

组队的过程中可以通过discard来放弃组队。

![image-20210814093211445](https://gitee.com/dong-dameng/images/raw/master/img/20210814093211.png)

**举例：**

![image-20210814093533666](https://gitee.com/dong-dameng/images/raw/master/img/20210814093533.png)

组队成功，提交成功

![image-20210814093551203](https://gitee.com/dong-dameng/images/raw/master/img/20210814093551.png)

组队阶段报错，提交失败

![image-20210814093603750](https://gitee.com/dong-dameng/images/raw/master/img/20210814093603.png)

## 事务的错误处理

组队中某个命令出现了报告错误，执行时整个的所有队列都会被取消。

![image-20210814093845236](https://gitee.com/dong-dameng/images/raw/master/img/20210814093845.png)

如果执行阶段某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，不会回滚。

![image-20210814093911893](https://gitee.com/dong-dameng/images/raw/master/img/20210814093911.png)

## 乐观锁和悲观锁

### 悲观锁

**悲观锁(Pessimistic Lock)**, 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。**
传统的关系型数据库里边就用到了很多这种锁机制**，比如**行锁**，**表锁**等，**读锁**，**写锁**等，都是在做操作之前先上锁。

缺点：效率很低

### 乐观锁

**乐观锁(Optimistic Lock),** 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。**
乐观锁适用于多读的应用类型，这样可以提高吞吐量**。Redis就是利用这种check-and-set机制实现事务的。

### 演示乐观锁

- `WATCH key [key ...]`:

  在执行multi之前，先执行`watch key1 [key2]`,可以监视一个(或多个) key ，如果在事务**执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。**

```shell
#A用户
watch balance
OK
multi
OK
127.0.0.1:6379(TX)> incrby balance 10
QUEUED

#B用户
watch balance
OK
multi
OK
127.0.0.1:6379(TX)> incrby balance 20
QUEUED

#A用户先提交，成功
exec
1) (integer) 30

#B用户后提交，失败
exec
(nil)
```

- `unwatch`:

  取消 WATCH 命令对所有 key 的监视。

  如果在执行 WATCH 命令之后，EXEC 命令或DISCARD 命令先被执行了的话，那么就不需要再执行UNWATCH 了。

## 事务三特性

- **单独的隔离操作**

  事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。

- **没有隔离级别的概念**

  队列中的命令没有提交之前都不会实际被执行，因为事务提交前任何指令都不会被实际执行

- **不保证原子性**

  事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚
  
   

 

