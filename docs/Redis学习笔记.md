# 1、redis简介

> **Redis**是一个使用[ANSI C](https://zh.wikipedia.org/wiki/ANSI_C)编写的[开源](https://zh.wikipedia.org/wiki/开源)、支持[网络](https://zh.wikipedia.org/wiki/电脑网络)、基于[内存](https://zh.wikipedia.org/wiki/内存)、可选[持久性](https://zh.wikipedia.org/w/index.php?title=持久性_(数据库)&action=edit&redlink=1)的[键值对存储数据库]
>
> 它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

# 2、linux安装

## 1、下载

- 在线下载

	```c
	$ wget http://download.redis.io/releases/redis-5.0.7.tar.gz
	$ tar xzf redis-5.0.7.tar.gz -C /root/tool //解压到指定目录
	$ cd redis-5.0.7.tar.gz
	$ make
	```

- 利用ftp等软件上传至Linux桌面

	```c
	$ tar xzf redis-5.0.7.tar.gz
	$ cd redis-5.0.7.tar.gz
	$ make
	```

	**注：**我们需要将源码编译后再安装，因此需要安装 c 语言的编译环境！不能直接 make

	```c
	yum install gcc-c++ -y
	```

	==make时遇到的错误：==

	```c
	In file included from adlist.c:34:0:
	zmalloc.h:50:31: fatal error: jemalloc/jemalloc.h: No such file or directory
	 #include <jemalloc/jemalloc.h>
	                               ^
	compilation terminated.
	```

	**解决办法：==make MALLOC=libc==**

## 2、安装

```c
make PREFIX=/usr/local/redis instal
```

安装到指定目录，若没有，则创建该目录；PREFIX必须大写

## 3、启动服务端跟客户端

![](https://raw.githubusercontent.com/yhx1001/PicGo/img/20200401214417.png)

```c
./redis-server
./redis-cli
```

Redis服务器默认会使用6379端口1) , 通过－－port参数可以自定义端口号：

```c
redis-server --port 6380
```

## 4、关闭

```c
shutdown
```

## 5、docker安装redis

```c
docker search redis        //命令来查看可用版本
    
docker pull redis:latest   //拉取官方的最新版本的镜像
    
docker images              //查看镜像
 
docker run -d --name redis-test -p 6379:6379 redis --requirepass "100104"//创建并运行容器
  //  --requirepass 密码 -d: 后台运行容器，并返回容器ID；
docker exec -it redis redis-cli -a 100104            //运行客户端 --it 交互
 
/*docker常用命令
1、docker ps 查看进程
2、docker container rm 删除容器
https://www.runoob.com/docker/docker-command-manual.html
*/
```

# 3、redis配置

## 1、redis.conf

## 2、内存维护策略（缓存清理策略）

1. | 属性                            |          含义          | 备注                                                         |
	| :------------------------------ | :--------------------: | :----------------------------------------------------------- |
	| bind                            |    限定访问的主机地    | 如果没有bind，就是任意ip 地址都可以访问。生产环境下，需要写自己应用服务器的ip 地址。 |
	| protected-mode                  |      安全防护模式      | 如果没有指定bind 指令，也没有配置密码，那么保护模式就开启，只允许本机访问。 |
	| port                            |         端口号         | 默认是6379                                                   |
	| timeout                         |        超时时间        | 默认永不超时                                                 |
	| daemonize                       | 是否为守护进程模式运行 | 守护进程模式可以在后台运行，默认是no                         |
	| pidfile                         | 进程id 文件保存的路径  | 配置PID 文件路径，当redis 作为守护进程运行的时候，它会把pid默认写到/var/redis/run/redis_6379.pid 文件里面 |
	| logfile                         |     日志文件的位置     | 当指定为空字符串时，为标准输出，如果redis 以守护进程模式运行，那么日志将会输出到/dev/null |
	| databases                       |     设置数据库数量     | 默认是0                                                      |
	| requirepass                     |        设置密码        | 默认没有，但远程连接可能会连接不上                           |
	| maxclients                      |       最大连接数       |                                                              |
	| maxmemory                       |    最大占用多少内存    | 一旦占用内存超限，就开始根据缓存清理策略移除数据如果Redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么Redis 则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH 等。 |
	| **maxmemory-policy noeviction** |    **缓存清理策略**    | **（1）volatile-lru：使用LRU 算法移除key，只对设置了过期时间的键<br/>（2）allkeys-lru：使用LRU 算法移除key<br/>（3）volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键<br/>（4）allkeys-random：移除随机的key<br/>（5）volatile-ttl：移除那些TTL 值最小的key，即那些最近要过期的key<br/>（6）noeviction：不进行移除。针对写操作，只是返回错误信息** |

# 4、Redis基本操作

## 1、数据库

```c
select <dbid>  //切换数据库 select 1 切换到1号数据库，默认为0
flushdb        //清空当前库
dbsize         //查看数据库个数
flushall       //通杀全部库
```

## 2、Key

Redis 中的数据以**键值对（key-value）**为基本存储方式，其中**key 都是字符串**。

```c
KEYS pattern   //查询符合指定表达式的所有key，支持*，？等
TYPE key       //查看key 对应值的类型
EXISTS key     //指定的key 是否存在，0 代表不存在，1 代表存在
DEL key       //删除指定key
RANDOMKEY     //在现有的KEY 中随机返回一个
EXPIRE key seconds   //为键值设置过期时间，单位是秒，过期后key 会被redis 移除
TTL key       //查看key 还有多少秒过期，-1 表示永不过期，-2 表示已过期
RENAME key newkey
//重命名一个key，NEWKEY 不管是否是已经存在的都会执行，如果NEWKEY 已经存在则会被覆盖
RENAMENX key newkey  //只有在NEWKEY 不存在时能够执行成功，否则失败
```

![image-20200404225335437](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200404225335437.png)

## 3、String

String 类型是Redis 中最基本的类型，它是key 对应的一个单一值。

二进制安全，不必担心由于编码等问题导致二进制数据变化。所以redis 的string 可以包含任何数据，比如jpg

图片或者序列化的对象。Redis 中一个字符串值的最大容量是512M。

```c
SET key value              //添加键值对
GET key                    //查询指定key 的值
APPEND key value           //将给定的value 追加到原值的末尾
STRLEN key                //获取值的长度
SETNX key value           //只有在key 不存在时设置key 的值
INCR key                  //指定key 的值自增1，只对数字有效
DECR key                  //指定key 的值自减1，只对数字有效
INCRBY key num            //自增num
DECRBY key num            //自减num
MSET key1 value1 key2 value2…    //同时设置多个key-value 对
MGET key1 key2            //同时获取一个或多个value
MSETNX key1 value1 key2 value2   //当key 不存在时，设置多个key-value 对
GETRANGE key 起始索引 结束索       //获取指定范围的值，都是闭区间
SETRANGE key 起始索引value        //从起始位置开始覆写指定的值
GETSET key value                 //以新换旧，同时获取旧值
SETEX key 过期时间value           //设置键值的同时，设置过期时间，单位秒
```

![image-20200404230347984](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200404230347984.png)

## 4、List

Redis列表是简单的字符串列表，按照插入**顺序排序**。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)

常见操作：

**遍历：遍历的时候，是从左往右取值；**

**删除：弹栈，POP；**

**添加：压栈，PUSH ；**

它的底层实际是个**双向链表**，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差。

```c
LPUSH/RPUSH key value1 value2…  //从左边/右边压入一个或多个值,头尾效率高，中间效率低
LPOP/RPOP key                   //从左边/右边弹出一个值,值在键在，值光键亡,弹出=返回+删除
LRANGE key start stop          //查看指定区间的元素,正着数：0,1,2,3,...倒着数：-1,-2,-3,...
LINDEX key index              //按照索引下标获取元素（从左到右）
LLEN key                      //获取列表长度
LINSERT key BEFORE|AFTER value newvalue //在指定value 的前后插入newvalue
LREM key n value             //从左边删除n 个value
LSET key index value         //把指定索引位置的元素替换为另一个值
LTRIM key start stop         //仅保留指定区间的数据
RPOPLPUSH key1 key2         //从key1 右边弹出一个值，左侧压入到key2
```

![image-20200405152307205](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200405152307205.png)

## 5、Set

##### ==set 是无序的，且是不可重复的。==

```c
SADD key member [member ...] 
//将一个或多个member 元素加入到集合key 当中，已经存在于集合的member 元素将被忽略。
SMEMBERS key                     //取出该集合的所有值
SISMEMBER key value              //判断集合<key>是否为含有该<value>值，有返回1，没有返回0
SCARD key                        //返回集合中元素的数量
SREM key member [member ...]    //从集合中删除元素
SPOP key [count]                //从集合中随机弹出count 个数量的元素，count 不指定就弹出1 个
SRANDMEMBER key [count]         //从集合中随机返回count 个数量的元素，count 不指定就返回1 个
SINTER key [key ...]            //将指定的集合进行“交集”操作
SINTERSTORE dest key [key ...]  //取交集，另存为一个set
SUNION key [key ...]             //将指定的集合执行“并集”操作
SUNIONSTORE dest key [key ...]  //取并集，另存为set
SDIFF key [key ...]             //将指定的集合执行“差集”操作
SDIFFSTORE dest key [key ...]  //取差集，另存为set
```

![image-20200405153111672](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200405153111672.png)

## 6、Hash

Hash 数据类型的键值对中的值是“单列”的，不支持进一步的层次结构。hash 特别适合用于**存储对象**。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

```java
//从前到后的数据对应关系
@Data
public class User implements Serializable {
    private String id;
    private String name;
    private Integer age;
}

User user = new User();
user.setId("1");
user.setName("yhx");
user.setAge(18);
```

```c
HSET key field value                          //为key 中的field 赋值value
HMSET key field value [field value ...]       //为指定key 批量设置field-value
HSETNX key field value                        //当指定key 的field 不存在时，设置其value
HGETALL key                                   //获取指定key 的所有信息（field 和value）
HKEYS key                                     //获取指定key 的所有field
HVALS key                                     //获取指定key 的所有value
HLEN key                                     //指定key 的field 个数
HGET key field                               //从key 中根据field 取出value
HMGET key field [field ...]                  //为指定key 获取多个filed 的值
HEXISTS key field                            //指定key 是否有field
HINCRBY key field increment                  //为指定key 的field 加上增量increment
```

![image-20200405154227541](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200405154227541.png)

## 7、Zset

zset 是一种特殊的set（sorted set），在保存value 的时候，为每个value 多保存了一个score 信息。根据score
信息，可以进行排序。

这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可
以是重复了

```c
ZADD key [score member ...]               //添加
ZSCORE key member                        //返回指定值的分数
ZRANGE key start stop [WITHSCORES]      //返回指定区间的值，可选择是否一起返回scores
ZRANGEBYSCORE key min max [WITHSCORES][LIMIT offset count]
//在分数的指定区间内返回数据，从小到大排列
ZREVRANGEBYSCORE key max min[WITHSCORES] [LIMIT offset count]
//在分数的指定区间内返回数据，从大到小排列
ZCARD key                               //返回集合中所有的元素的数量
ZCOUNT key min max                      //统计分数区间内的元素个数
ZREM key member                        //删除该集合下，指定值的元素
ZRANK key member                       //返回该值在集合中的排名，从0 开始
ZINCRBY key increment value            //为元素的score 加上增量
```

![image-20200405154822259](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200405154822259.png)

## 8、HyperLogLog

Redis 在 **2.8.9 版本添加了 HyperLogLog 结构**。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，==在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。==

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

#### 什么是基数?

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是

在误差可接受的范围内，快速计算基数。

```c
PFADD key element [element ...]               //添加指定元素到 HyperLogLog 中。
PFCOUNT key [key ...]                         //返回给定 HyperLogLog 的基数估算值。
PFMERGE destkey sourcekey [sourcekey ...]     //将多个 HyperLogLog 合并为一个 HyperLogLog
```

![image-20200405155752941](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200405155752941.png)

#### 应用场景

- 统计注册ip数
- 统计每日访问IP数
- 统计页面实时uv数
- 统计在线用户数
- 统计用户每天搜索不同词条的个数
- 统计真实文章阅读数

## 9、redis发布订阅

Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

Redis 客户端可以订阅任意数量的频道。

![](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406201522100.png)

```c
SUBSCRIBE channel [channel ...]            //订阅给定的一个或多个频道的信息。
PUBLISH channel message                    //将信息发送到指定的频道。
PUBSUB subcommand [argument [argument ...]]//查看订阅与发布系统状态。
PSUBSCRIBE pattern [pattern ...]           //订阅一个或多个符合给定模式的频道。
UNSUBSCRIBE [channel [channel ...]]        //指退订给定的频道。
```

# 5、持久化

Redis 主要是**工作在内存中**。内存本身就不是一个持久化设备，断电后数据会清空。所以Redis 在工作过程中，如果发生了意外停电事故，如何尽可能减少数据丢失。

## 1、RDB

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是Snapshot 快照，它恢复时是将**快照文件直接读到内存里。**
==工作机制：每隔一段时间，就把内存中的数据保存到硬盘上的指定文件中。==RDB 是默认开启的！

> Redis 会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO 操作的，这就确保了极高的性能如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB 方式要比AOF 方式更加的高效。
> RDB 的缺点是**最后一次持久化后的数据可能丢失。**

### 1、RDB保存策略

```c
save 900 1      //900 秒内如果至少有1 个key 的值变化，则保存
save 300 10     //300 秒内如果至少有10 个key 的值变化，则保存
save 60 10000   //60 秒内如果至少有10000 个key 的值变化，则保存
save “”         //就是禁用RDB 模式；
```

### 2、RDB 常用属性配置

```c
save                        //保存策略
dbfilename RDB              //快照文件名
dir RDB                     //快照保存的目录必须是一个目录，不能是文件名。最好改为固定目录。默认为./代表执行redis-server 命令时的当前目录！
    
stop-writes-on-bgsave-error 
    //是否在备份出错时，继续接受写操作如果用户开启了RDB 快照功能，那么在redis 持久化数据到磁盘时如果出现失败，默认情况下，redis 会停止接受所有的写请求
    
rdbcompression             
    //对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis 会采用LZF 算法进行压缩。如果你不想消耗CPU 来进行压缩的话，可以设置为关闭此功能，但是存储在磁盘上的快照会比较大。
    
rdbchecksum                 
    //是否进行数据校验在存储快照后，我们还可以让redis 使用CRC64 算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能。
```

### 3、RDB 数据丢失的情况

**两次保存的时间间隔内，服务器宕机，或者发生断电问题。**

### 4、RDB 的触发

​	① 基于自动保存的策略

​	② 执行save，或者bgsave 命令！执行时，是阻塞状态。

​	③ 执行flushdb 命令，也会产生dump.rdb，但里面是空的，没有意义。

​	④ 当执行shutdown 命令时，也会主动地备份数据。

### 5、RDB的优缺点

**RDB优点：**

（1）RDB会生成多个数据文件，每个数据文件都代表了某一个时刻中redis的数据，这种多个数据文件的方式，非常适合做冷备。
（2）RDB对redis对外提供读写服务的时候，影像非常小，因为redis 主进程只需要fork一个子进程出来，让子进程对磁盘io来进行rdb持久化
（3）RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快

（4）RDB 适合用于灾难恢复，因为它只有一个文件，而且体积小，方便拷贝。

**RDB缺点：**

（1）如果redis要故障时要尽可能少的丢失数据，RDB没有AOF好，例如1:00进行的快照，在1:10又要进行快照的时候宕机了，这个时候就会丢失10分钟的数据。
（2）RDB每次fork出子进程来执行RDB快照生成文件时，如果文件特别大，可能会导致客户端提供服务暂停数毫秒或者几秒

## 2、AOF

- AOF 是 以日志的形式来记录每个写操作，将每一次对数据进行修改，都把新建、修改数据的命令保存到指
	定文件中。Redis 重新启动时读取这个文件，重新执行新建、修改数据的命令恢复数据。
-  默认不开启，需要手动开启
- AOF 文件的保存路径，同RDB 的路径一致。
- AOF 在保存命令的时候，只会保存对数据有修改的命令，也就是写操作！
-  当RDB 和AOF 存的不一致的情况下，按照AOF 来恢复。因为AOF 是对RDB 的补充。备份周期更短，也就更
	可靠。

### 1、AOF 保存策略

```c
appendfsync always：每次产生一条新的修改数据的命令都执行保存操作；效率低，但是安全！
appendfsync everysec：每秒执行一次保存操作。如果在未保存当前秒内操作时发生了断电，仍然会导致一部分数据丢失（即1 秒钟的数据）。
appendfsync no：从不保存，将数据交给操作系统来处理。更快，也更不安全的选择。
推荐（并且也是默认）的措施为每秒fsync 一次， 这种fsync 策略可以兼顾速度和安全性。
```

### 2、AOF 常用属性

```c
appendonly                   //是否开启AOF 功能默认是关闭的
appendfilename               //AOF文件名称
appendfsync AOF              //保存策略官方建议everysec
no-appendfsync-on-rewrite     
    //在重写时，是否执行保存策略执行重写，可以节省AOF 文件的体积；而且在恢复的时候效率也更高。
auto-aof-rewrite-percentage   
    //重写的触发条件当目前aof文件大小超过上一次重写的aof文件大小的百分之多少进行重写
auto-aof-rewrite-min-size     
    //设置允许重写的最小aof文件大小,避免了达到约定百分比但尺寸仍然很小的情况还要重写
aof-load-truncated            
    //截断设置如果选择的是yes，当截断的aof 文件被导入的时候，会自动发布一个log 给客户端然后load
```

### 3、AOF 文件的修复

如果AOF 文件中出现了残余命令，会导致服务器无法重启。此时需要借助redis-check-aof 工具来修复！

```c
redis-check-aof –fix 文件
```

### 4、AOF 的优缺点

**AOF的优点：**

（1）AOF可以更好的保护数据不丢失，一般AOF会以每隔1秒，通过后台的一个线程去执行一次fsync操作，如果

redis进程挂掉，最多丢失1秒的数据。

（2）AOF以appen-only的模式写入，所以没有任何磁盘寻址的开销，写入性能非常高。9

（3）AOF日志文件的命令通过非常可读的方式进行记录，这个非常适合做灾难性的误删除紧急恢复，如果某人不

小心用flushall命令清空了所有数据，只要这个时候还没有执行rewrite，那么就可以将日志文件中的flushall删除，

进行恢复。

```
- 备份机制更稳健，丢失数据概率更低
- 可读的日志文本，通过操作AOF 稳健，可以处理误操作
```

**AOF的缺点：**

（1）对于同一份文件AOF文件比RDB数据快照要大。

（2）AOF开启后支持写的QPS会比RDB支持的写的QPS低，因为AOF一般会配置成每秒fsync操作，每秒的fsync

操作还是很高的

（3）数据恢复比较慢，不适合做冷备。

```
- 比起RDB 占用更多的磁盘空间
- 恢复备份速度要慢
- 每次读写都同步的话，有一定的性能压力
- 存在个别Bug，造成恢复不能
```

## 3、备份建议

**如何看待数据“绝对”安全？**

> Redis 作为内存数据库从本质上来说，如果不想牺牲性能，就不可能做到数据的“绝对”安全。
> RDB 和AOF 都只是尽可能在兼顾性能的前提下降低数据丢失的风险，如果真的发生数据丢失问题，尽可能
> 减少损失。
> 在整个项目的架构体系中，Redis 大部分情况是扮演“二级缓存”角色。
>
> 二级缓存适合保存的数据
>
> - 经常要查询，很少被修改的数据。
>
> - 不是非常重要，允许出现偶尔的并发问题。
>
> - 不会被其他应用程序修改。
>
> 	如果Redis 是作为缓存服务器，那么说明数据在MySQL 这样的传统关系型数据库中是有正式版本的。数据最终以MySQL 中的为准。

**RDB和AOF到底如何选择?**

（1）不要仅仅使用RDB这样会丢失很多数据。

（2）也不要仅仅使用AOF，因为这会有两个问题，第一通过AOF做冷备没有RDB做冷备恢复的速度快；第二

RDB每次简单粗暴生成数据快照，更加健壮。

（3）综合AOF和RDB两种持久化方式，用AOF来保证数据不丢失，作为恢复数据的第一选择；用RDB来做不同程

度的冷备，在AOF文件都丢失或损坏不可用的时候，可以使用RDB进行快速的数据恢复。

**官方推荐两个都用：**==如果对数据不敏感，可以选单独用RDB；不建议单独用AOF，因为可能出现Bug;如果只是
做纯内存缓存，可以都不用==

# 6、事务

**Redis 事务可以一次执行多个命令**， 并且带有以下三个重要的保证：

- 批量操作在发送 EXEC 命令前被放入队列缓存。
- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。
- 命令入队。
- 执行事务。

## 1、事务简介

- Redis 中事务，不同于传统的关系型数据库中的事务。
- Redis 中的事务指的是一个单独的隔离操作。
- Redis 的事务中的所有命令都会序列化、按顺序地执行且不会被其他客户端发送来的命令请求所打
- Redis 事务的主要作用是串联多个命令防止别的命令插队

## 2、常用命令

```c
MULTI          //标记一个事务块的开始

EXEC           
                //执行所有事务块内的命令。执行事务中所有在排队等待的指令并将链接状态恢复到正常当使用WATCH时，只有当被监视的键没有被修改，且允许检查设定机制时，EXEC会被执行
                
WATCH           //标记所有指定的key 被监视起来，在事务中有条件的执行（乐观锁）

UNWATCH         //取消 WATCH 命令对所有 key 的监视。

DISCARD         
//刷新一个事务中所有在排队等待的指令，并且将连接状态恢复到正常。如果已使用WATCH，DISCARD将释放所有被WATCH 的key。
```

![image-20200406215622035](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406215622035.png)

![image-20200406221329363](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406221329363.png)

**MULTI 开启组队，EXEC 依次执行队列中的命令。**

**DISCARD 中途取消组队**

![image-20200406215934571](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406215934571.png)

## 3、错误命令处理

1、此种情况，语法符合规范，Redis 只有在执行中，才可以发现错误。而在Redis 中，并没有回滚机制，因此错误的命令，无法执行，正确的命令会全部执行！

![image-20200406220325713](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406220325713.png)

2、在编译的过程中，Redis 检测出来了错误的语法命令，因此它认为这条组队，一定会发生错误，因此全体取消；

![image-20200406220528937](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200406220528937.png)

## 4、锁

### 	1、悲观锁

执行操作前假设当前的操作肯定（或有很大几率）会被打断（悲观）。基于这个假设，我们在做操作前就会把相关

资源锁定，不允许自己执行期间有其他操作干扰。

Redis 不支持悲观锁。Redis 作为缓存服务器使用时，以读操作为主，很少写操作，相应的操作被打断的几率较

少。不采用悲观锁是为了防止降低性能。

### 	2、乐观锁

执行操作前假设当前操作不会被打断（乐观）。基于这个假设，我们在做操作前不会锁定资源，万一发生了其他操

作的干扰，那么本次操作将被放弃。

## 5、Redis 中的锁策略

- Redis 采用了乐观锁策略**（通过watch 操作）**。乐观锁支持读操作，适用于多读少写的情况！

- 在事务中，可以通过**watch 命令来加锁**；使用**UNWATCH 可以取消加锁**；

- 如果在事务之前，执行了WATCH（加锁），那么执行EXEC 命令或DISCARD 命令后，锁对自动释放，即不需

	要再执行UNWATCH 了

# 7、Redis Cluste集群

> 什么是集群：
> Redis 集群实现了对Redis 的水平扩容，即启动N 个redis 节点，将整个数据库分布存储在这N 个节点中，每个节点存储总数据的1/N。
> Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

## 1、搭建集群环境

### 1、创建目录并修改集群配置文件（redis.conf）

```c
mkdir cluster-test
cd cluster-test
mkdir 7000 7001 7002 7003 7004 7005
```

```c
port 7000
//开启集群模式
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes

//依次修改6个配置文件
cp ./7000/redis.conf ./7000/
:%s/7000/7001/g      替换
```

### 2、启动节点

把安装Redis目录下的src复制到redis_cluster下，方便启动服务

```c
//进入安装目录下 cd /root/tool/redis-5.0.8/（我的为例）
cp -r ./src/ /usr/local/redis_cluster
    
./src/redis-server ./7000/redis.conf
./src/redis-server ./7001/redis.conf
./src/redis-server ./7002/redis.conf
./src/redis-server ./7003/redis.conf
./src/redis-server ./7004/redis.conf
./src/redis-server ./7005/redis.conf
```

![image-20200407231906916](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200407231906916.png)

```c
ps -ef |grep -i redis          //查看进程
```

### 3、创建集群

```c
//redis5之后
./src/redis-cli --cluster create -a 100104 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 --cluster-replicas 1
```

![image-20200408194731829](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200408194731829.png)

![image-20200408194602993](D:%5C360MoveData%5CUsers%5CAdministrator%5CDesktop%5Cupload%5Cimage-20200408194602993.png)

## 2、集群验证

```
./src/redis-cli -p 7000 -a 100104
```

```
127.0.0.1:7000> CLUSTER nodes
b7959193eb80fa0abc8cf9d296d08de76cf47f4d 127.0.0.1:7002@17002 master - 0 1586346968000 3 connected 10923-16383
74dc8b2155478d22d147d86dd052f84824bcc021 127.0.0.1:7001@17001 master - 0 1586346969041 2 connected 5461-10922
26764a2d2dc63b0ba3216de62ac0a171329e6f43 127.0.0.1:7005@17005 slave 1af6831ee1e7f9448fed04fab40b68f30b001b59 0 1586346968033 6 connected
1af6831ee1e7f9448fed04fab40b68f30b001b59 127.0.0.1:7000@17000 myself,master - 0 1586346968000 1 connected 0-5460
6373481e4202f55a6d3a0f46a0da00a395573861 127.0.0.1:7003@17003 slave 74dc8b2155478d22d147d86dd052f84824bcc021 0 1586346968537 4 connected
20c5257ce6ea6a2aa666f283e068e865152c52a6 127.0.0.1:7004@17004 slave b7959193eb80fa0abc8cf9d296d08de76cf47f4d 0 1586346969544 5 connected
```

## 3、集群开启与关闭

### 1、开启

```sh
#redisstart.sh     –c 参数实现自动重定向
/usr/local/redis_cluster/src/redis-server ./7000/redis.conf
/usr/local/redis_cluster/src/redis-server ./7001/redis.conf
/usr/local/redis_cluster/src/redis-server ./7002/redis.conf
/usr/local/redis_cluster/src/redis-server ./7003/redis.conf
/usr/local/redis_cluster/src/redis-server ./7004/redis.conf
/usr/local/redis_cluster/src/redis-server ./7005/redis.conf
```

```c
chmod u+x redisall.sh  //变成可执行文件
./redisall.sh          //在当前目录下启动
```

```shell
#startall.sh
/usr/local/redis_cluster/src/redis-cli --cluster create -a 100104 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 --cluster-replicas 1
```

```c
chmod u+x startall.sh
./startall.sh
```

### 2、关闭

```shell
#shutdown.sh
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7000 -a 100104 shutdown
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7001 -a 100104 shutdown
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7002 -a 100104 shutdown
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7003 -a 100104 shutdown
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7004 -a 100104 shutdown
/usr/local/redis_cluster/src/redis-cli -c -h 127.0.0.1 -p 7005 -a 100104 shutdown
```

```
chmod u+x shutdown.sh
./shutdown.sh
```

## 4、集群中故障恢复

问题1：如果主节点下线？从节点能否自动升为主节点？

> 答：**主节点下线，从节点自动升为主节点**。

问题2：主节点恢复后，主从关系会如何？

> **主节点恢复后，主节点变为从节点！**

问题3：如果所有某一段插槽的主从节点都宕掉，redis 服务是否还能继续?

> 答：**服务是否继续，可以通过redis.conf 中的cluster-require-full-coverage 参数进行控制。**



> 主从都宕掉，意味着有一片数据，会变成真空，没法再访问了！
> 如果无法访问的数据，是连续的业务数据，我们需要停止集群，避免缺少此部分数据，造成整个业务的异常。此时可以通过配置cluster-require-full-coverage 为yes.
> 如果无法访问的数据，是相对独立的，对于其他业务的访问，并不影响，那么可以继续开启集群体提供服务。此时，可以配置cluster-require-full-coverage 为no。

## 5、集群的优缺点

**优点：**

- 实现扩容
- 分摊压力
-  无中心配置相对简单

**缺点：**

- 多键操作是不被支持的
- 多键的Redis 事务是不被支持的。lua 脚本不被支持。
- 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁
	移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。