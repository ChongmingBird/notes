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

- 查询是否启动：`ps -ef|grep redis`

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

String是Redis最基本的数据类型，它是**二进制安全的**，也就是说Redis的String可以包含任何数据，比如jpg图片或者序列化的对象。

String类型是Redis最基本的数据类型，一个Redis中字符串value最多可以是512MB

### 常用命令

|      操作      |                    命令                    |                             备注                             |
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

String的数据结构为简单的动态字符串，是可以修改的字符串，内部结构上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。

内部分配的空间capacity一般要高于实际字符串长度len，当字符串长度小于1M时，扩容都是加倍当前空间，如果超过1M，每次只扩容1M，并且最大长度不会超过512M

## List

### 简介

Redis列表是简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部（左边）或尾部（右边）。

它的底层实际上是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

### 常用命令

|       操作       |                   命令                    |                             备注                             |
| :--------------: | :---------------------------------------: | :----------------------------------------------------------: |
|  添加/修改数据   | `lpush/rpush <key> <value1> <value2> ...` |                                                              |
|     弹出数据     |             `lpop/rpop <key>`             | 从左边/右边吐出一个值。数据一旦吐出，原来的列表里就没有了，值在键在，值亡键亡 |
|     获取数据     |       `lrange <key> <start> <stop>`       | 获取start到stop(从左到右)的数据<br />`lrange key 0 -1`获取所有数据 |
| 按照索引获取数据 |          `lindex <key> <index>`           |                按照索引下标获得元素(从左到右)                |
|   获得列表长度   |               `llen <key>`                |                                                              |
|                  |                                           |                                                              |
|                  |                                           |                                                              |
|                  |                                           |                                                              |
|                  |                                           |                                                              |

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

List的数据结构为快速链表quickList

首先列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表。

它将所有的元素紧挨着一起存储，分配的是一块连续的内存，当数据量比较多的时候才会改成quicklist

因为普通的链表需要的附加指针空间太大，会比较浪费空间。

所以Redis将链表和ziplist结合，也就是将多个ziplist使用双向指针串起来，这样既满足了快速的插入和删除性能，又不会出现太大的空间冗余。

## Set

### 简介

Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的。

当需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。

Redis的Set是string类型的无序集合。它底层其实是一个value为null的hash表，所以添加，删除，查找的**复杂度都是O(1)**。

一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变

### 常用命令

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

- `smove <source> <destination> <value>`

  把集合中一个值<value>从一个集合<source>移动到另一个集合<destination>

- `sinter <key1> <key2>`

  返回两个集合的交集元素。

- `sunion <key1> <key2>`

  返回两个集合的并集元素。

- `sdiff <key1> <key2>`

  返回两个集合的**差集**元素(key1中的，不包含key2中的)

### 数据结构

Set数据结构是dict字典，字典是用哈希表实现的。

Java中HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也是一样，它的内部也使用hash结构，所有的value都指向同一个内部值。

## Hash

### 简介

Redis hash 是一个键值对集合。

Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

类似Java里面的`Map<String,Object>`，一个存储空间(hash)保存多个键值对<filed,value> 

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

  将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在 .

### 数据结构

Hash类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。

当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable。

## Zset

### 简介

Redis有序集合zset与普通集合set非常相似，是一个没有重复元素的字符串集合。

不同之处是有序集合的每个成员都关联了一个**评分（score)**,这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了 。

因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。

访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

### 常用命令

- `zadd <key> <score1> <value1> <score2> <value2> …`

  将一个或多个 member 元素及其 score 值加入到有序集 key 当中。

- `zrange <key> <start> <stop> [WITHSCORES]`

  返回有序集 key 中，下标在<start><stop>之间的元素

  带WITHSCORES，可以让分数一起和值返回到结果集。

- `zrangebyscore <key> minmax [withscores] [limit offset count]`

  返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。

  可选的 `LIMIT` 参数指定返回结果的数量及区间(limit 3,4代表返回三到四条数据）

- `zrevrangebyscore key maxmin [withscores] [limit offset count]`

  同上，改为从大到小排列。

- `zincrby <key> <increment> <value>`

  为元素的score加上增量

- `zrem <key> <value>`删除该集合下，指定值的元素

- `zcount <key> <min> <max>`统计该集合，分数区间内的元素个数

- `zrank <key> <value>`返回该值在集合中的排名，从0开始。

### 数据结构

SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构Map<String, Double>
，可以给每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。

zset底层使用了两个数据结构：

（1）hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。

（2）跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。

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

# 三大新数据类型

## Bitmaps

### 简介

现代计算机用二进制（位）作为信息的基础单位，1个字节等于8位，例如“abc”字符串是由3个字节组成， 但实际在计算机存储时将其用二进制表示， “abc”分别对应的ASCII码分别是97、 98、 99， 对应的二进制分别是01100001、
01100010和01100011，如下图：

![image-20210813105325369](https://gitee.com/dong-dameng/images/raw/master/img/20210813105325.png)

合理地使用操作位能够有效地提高内存使用率和开发效率。

Redis提供了Bitmaps这个“数据类型”可以实现对位的操作：

（1） Bitmaps本身不是一种数据类型，实际上它就是字符串（key-value），但是它可以对字符串的位进行操作。

（2） Bitmaps单独提供了一套命令， 所以在Redis中使用Bitmaps和使用字符串的方法不太相同。 可以把Bitmaps想象成一个以位为单位的数组， 数组的每个单元只能存储0和1， 数组的下标在Bitmaps中叫做偏移量。

![image-20210813105348126](https://gitee.com/dong-dameng/images/raw/master/img/20210813105348.png)

### 命令

#### setbit

（1）格式

`setbit <key> <offset> <value>`

设置Bitmaps中某个偏移量的值（0或1）

![image-20210813113702746](https://gitee.com/dong-dameng/images/raw/master/img/20210813113702.png)

offset:偏移量从0开始

（2）实例

每个独立用户是否访问过网站存放在Bitmaps中，将访问的用户记做1，没有访问的用户记做0，用偏移量作为用户的id。

设置键的第offset个位的值（从0算起），假设现在有20个用户，userid=1，6，11，15，19的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图

![image-20210813113217896](https://gitee.com/dong-dameng/images/raw/master/img/20210813113217.png)

`unique:users:20201106`: 代表2020-11-06这天的独立访问用户的Bitmaps

![image-20210813113231591](https://gitee.com/dong-dameng/images/raw/master/img/20210813113231.png)

**PS:**

 很多应用的用户id以一个指定数字（例如10000） 开头， 直接将用户id和Bitmaps的偏移量对应势必会造成一定的浪费， 通常的做法是每次做setbit操作时将用户id减去这个指定数字。

 在第一次初始化Bitmaps时， 假如偏移量非常大， 那么整个初始化过程执行会比较慢， 可能会造成Redis的阻塞。

#### getbit

（1）格式

`getbit <key> <offset>`

获取Bitmaps中某个偏移量的值

​            ![image-20210813113431215](https://gitee.com/dong-dameng/images/raw/master/img/20210813113431.png)

获取键的第offset位的值（从0开始算）

（2）实例

获取id=8的用户是否在2020-11-06这天访问过， 返回0说明没有访问过：

![image-20210813113456976](https://gitee.com/dong-dameng/images/raw/master/img/20210813113457.png)

**PS：因为100根本不存在，所以也是返回0**

#### bitcount

统计**字符串**被设置为1的bit数。一般情况下，给定的整个字符串都会被进行计数，通过指定额外的 start 或 end 参数，可以让计数只在特定的位上进行。start 和 end 参数的设置，都可以使用负数值：比如 -1
表示最后一个位，而 -2 表示倒数第二个位，start、end 是指bit组的字节的下标数，二者皆包含。

（1）格式

`bitcount <key> [start end]`

统计字符串从start字节到end字节比特值为1的数量

![image-20210813114103340](https://gitee.com/dong-dameng/images/raw/master/img/20210813114103.png)

（2）实例

计算2022-11-06这天的独立访问用户数量

![image-20210813114130035](https://gitee.com/dong-dameng/images/raw/master/img/20210813114130.png)

start和end代表起始和结束字节数， 下面操作计算用户id在第1个字节到第3个字节之间的独立访问用户数,对应的用户id是11,15,19。

![image-20210813114224370](https://gitee.com/dong-dameng/images/raw/master/img/20210813114224.png)

#### bitop

（1）格式

`bitop and(or/not/xor) <destkey> [key…]`

​                   ![image-20210813144821222](https://gitee.com/dong-dameng/images/raw/master/img/20210813144821.png)

bitop是一个复合操作， 它可以做多个Bitmaps的and(交集)、or(并集)、not(非) 、xor（异或）操作并将结果保存在destkey中。

（2）实例

2020-11-04 日访问网站的userid=1,2,5,9。

```shell
setbit unique:users:20201104 1 1
setbit unique:users:20201104 2 1
setbit unique:users:20201104 5 1
setbit unique:users:20201104 9 1
```

2020-11-03 日访问网站的userid=0,1,4,9。

```
setbit unique:users:20201103 0 1
setbit unique:users:20201103 1 1
setbit unique:users:20201103 4 1
setbit unique:users:20201103 9 1
```

计算出两天都访问过网站的用户数量

`bitop and unique:users:and:20201104_03 unique:users:20201103 unique:users:20201104`

![image-20210813144927980](https://gitee.com/dong-dameng/images/raw/master/img/20210813144928.png)

翻译翻译：求`unique:users:20201103`和`unique:users:20201104`的交集，结果放入`unique:users:and:20201104_03`中

![image-20210813144726352](https://gitee.com/dong-dameng/images/raw/master/img/20210813144726.png)

计算出任意一天都访问过网站的用户数量（例如月活跃就是类似这种)，可以使用or求并集。

### Bitmaps和Set对比

假设网站有1亿用户， 每天独立访问的用户有5千万， 如果每天用集合类型和Bitmaps分别存储活跃用户可以得到表

**set和bitmaps存储一天活跃用户对比**

| 数据类型 | 每个用户id占用空间 | 需要存储的用户量 |       全部内存量       |
| :------: | :----------------: | :--------------: | :--------------------: |
| 集合类型 |        64位        |     50000000     | 64位*50000000 = 400MB  |
| Bitmaps  |        1位         |    100000000     | 1位*100000000 = 12.5MB |

很明显， 这种情况下使用Bitmaps能节省很多的内存空间， 尤其是随着时间推移节省的内存还是非常可观的

**set和bitmaps存储独立用户对比**

| 数据类型 |  一天  | 一个月 | 一年  |
| :------: | :----: | :----: | :---: |
| 集合类型 | 400MB  |  12GB  | 144GB |
| Bitmaps  | 12.5MB | 375MB  | 4.5GB |

但Bitmaps并不是万金油， 假如该网站每天的独立访问用户很少， 例如只有10万（大量的僵尸用户） ， 那么两者的对比如下表所示， 很显然， 这时候使用Bitmaps就不太合适了， 因为基本上大部分位都是0。

**set和Bitmaps存储一天活跃用户对比（独立用户比较少）**

| 数据类型 | 每个userid占用空间 | 需要存储的用户量 |       全部内存量       |
| :------: | :----------------: | :--------------: | :--------------------: |
| 集合类型 |        64位        |      100000      |  64位*100000 = 800KB   |
| Bitmaps  |        1位         |    100000000     | 1位*100000000 = 12.5MB |

## HyperLogLog

### 简介

在工作当中，我们经常会遇到与统计相关的功能需求，比如统计网站PV（PageView页面访问量）,可以使用Redis的incr、incrby轻松实现。

但像UV（UniqueVisitor，独立访客）、独立IP数、搜索记录数等需要去重和计数的问题如何解决？这种求集合中不重复元素个数的问题称为基数问题。

解决基数问题有很多种方案：

（1）数据存储在MySQL表中，使用distinct count计算不重复个数

（2）使用Redis提供的hash、set、bitmaps等数据结构来处理

以上的方案结果精确，但随着数据不断增加，导致占用空间越来越大，对于非常大的数据集是不切实际的。

能否能够降低一定的精度来平衡存储空间？Redis推出了HyperLogLog

Redis HyperLogLog 是用来做基数统计的算法，**HyperLogLog 的优点是：**

在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

**什么是基数?**

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}。

基数估计就是在误差可接受的范围内，快速计算基数。

### 命令

#### pfadd

（1）格式

`pfadd <key> <element> [element ...]`  添加指定元素到 HyperLogLog 中

​             ![image-20210813151459291](https://gitee.com/dong-dameng/images/raw/master/img/20210813151459.png)

（2）实例

![image-20210813151543354](https://gitee.com/dong-dameng/images/raw/master/img/20210813151543.png)

将所有元素添加到指定HyperLogLog数据结构中。如果执行命令后HLL估计的近似基数发生变化，则返回1，否则返回0。

#### pfcount

（1）格式

`pfcount <key> [key ...]`

计算HLL的近似基数，可以计算多个HLL，比如用HLL存储每天的UV，计算一周的UV可以使用7天的UV合并计算即可

​      ![image-20210813151741952](https://gitee.com/dong-dameng/images/raw/master/img/20210813151741.png)

（2）实例

![image-20210813151810529](https://gitee.com/dong-dameng/images/raw/master/img/20210813151810.png)

#### pfmerge

（1）格式

`pfmerge <destkey> <sourcekey> [sourcekey ...]`

将一个或多个HLL合并后的结果存储在另一个HLL中，比如每月活跃用户可以使用每天的活跃用户来合并计算可得

![](https://gitee.com/dong-dameng/images/raw/master/img/20210813151700.png)

（2）实例

![image-20210813151921080](https://gitee.com/dong-dameng/images/raw/master/img/20210813151921.png)

## Geospatial

### 简介

Redis 3.2 中增加了对GEO类型的支持。GEO，Geographic，地理信息的缩写。该类型，就是元素的2维坐标，在地图上就是经纬度。redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作。

### 命令

#### geoadd

（1）格式

`geoadd <key> <longitude> <latitude> <member> [longitude latitude member...]`

添加地理位置（经度，纬度，名称）

![image-20210813152218993](https://gitee.com/dong-dameng/images/raw/master/img/20210813152219.png)

（2）实例

`geoadd china:city 121.47 31.23 shanghai`

`geoadd china:city 106.50 29.53 chongqing 114.05 22.52 shenzhen 116.38 39.90 beijing`

![image-20210813152304988](https://gitee.com/dong-dameng/images/raw/master/img/20210813152305.png)

两极无法直接添加，一般会下载城市数据，直接通过 Java 程序一次性导入。

有效的经度从 -180 度到 180 度。有效的纬度从 -85.05112878 度到 85.05112878 度。

当坐标位置超出指定范围时，该命令将会返回一个错误。

已经添加的数据，是无法再次往里面添加的。

#### geopos

（1）格式

`geopos <key> <member> [member...]`

获得指定地区的坐标值

![image-20210813152425760](https://gitee.com/dong-dameng/images/raw/master/img/20210813152425.png)

（2）实例

![image-20210813152450479](https://gitee.com/dong-dameng/images/raw/master/img/20210813152450.png)

#### geodist

（1）格式

`geodist <key> <member1> <member2> [m|km|ft|mi ]`

获取两个位置之间的直线距离

![image-20210813152546282](https://gitee.com/dong-dameng/images/raw/master/img/20210813152546.png)

（2）实例

获取两个位置之间的直线距离

![image-20210813152606143](https://gitee.com/dong-dameng/images/raw/master/img/20210813152606.png)

单位：

- m 表示单位为米[默认值]。
- km 表示单位为千米。
- mi 表示单位为英里。
- ft 表示单位为英尺。

如果用户没有显式地指定单位参数， 那么 GEODIST 默认使用米作为单位

#### georadius

（1）格式

`georadius <key> <longitude> <latitude> radius m|km|ft|mi`

以给定的经纬度为中心，找出某一半径内的元素

![image-20210813152657738](https://gitee.com/dong-dameng/images/raw/master/img/20210813152657.png)

key后面的四个参数分别代表：

- 经度
- 纬度
- 距离
- 单位

（2）实例

![image-20210813152747798](https://gitee.com/dong-dameng/images/raw/master/img/20210813152747.png)

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
  
   

 

