### 1.  redis数据结构

命令不区分大小写，key区分大小写

#### 1. String  

(simple Dynamic String 简称 sds)

- 最常用： set key value 	get key

  - 同时设置/获取多个键值： mset key value[key value....]	mget key[key....]
- 数值增减：递增数字 incr key； 增加指定整数：incrby key increment; 递减数值：decr key； 减少指定整数：decrby key decrement
- 获取字符串长度：STRLEN key
- 分布式锁：setnx key value;   setnx key value [EX seconds] [Px millsecon] [nx|xx]

**应用场景**

储存key-value 键值对；

点赞；

转发量；

是否喜欢的文章；

#### 2. hash

#### 3. list

（列表）

> 底层是链表结构，list的插入和删除操作非常块，时间复杂度为O(1)，不像数组结构插入、删除操作需要移动数据。

**应用场景**

- 消息队列： lpop 和 rpush (或者反过来，lpush和rpop)能实现队列的功能；
- 朋友圈的点赞列表、评论列表、排行榜：lpush命令和lrange命令能实现最新列表的功能，每次通过lpush命令往列表插入新的元素，然后通过lrange命令读取最新的元素列表。

#### 4. set

无重复数据的容器

- 添加元素 ： sadd key member [member...]

- 删除元素：  srem key member[member...]

- 遍历集合中的所有元素：smembers key

- 判断元素是否在集合中：sismember key member

- 获取集合中的元素总数：scard key

- 从集合中随机弹出一个元素，元素不删除：srandmember key[数字]

- 从集合中随机弹出一个元素，出一个删一个：spop key[数字]

- 集合运算（A、B）A: abc12  B: 123ax

  - 集合的差集运算A-B：属于A但不属于B的元素构成的集合； sdiff key [key...]
  - 集合的交集运算A^B:  属于A但同时属于B的元素构成的集合；sinter key[key...]
  - 集合的并集运算A U B：属于A或者属于B的元素合并后的集合；sunion key[key...]



**应用场景**

1. 微信抽奖小程序
     1. 用户ID，立即参与抽奖；sadd key 用户ID
     2. 显示已经有多少人参与了；scard key
     3. 抽奖（从set中任选取N个人中奖）srandmember key 2 ，随机两人，元素不删除； spop key 3 ，随机3人，元素会删除
2. 微信朋友圈点赞
       1. 新增点赞； sadd pub:msgId 点赞用户id1 点赞用户id2
           2. 取消点赞：srem pub:msgId 点赞用户id
           3. 展现所有点赞用户：smember pub:msgId
           4. 点赞用户数统计，就是常见的点赞红色数字：scard pub:msgId
           5. 判断某个朋友是否对楼主点赞：sismember pub:msgId 用户Id
3. 微博好友关注社交关系
          1. 共同关注的人：交集
              2. 
4. QQ内推可能认识的人： 差集 交集

#### 5. Zset

- 添加元素：zadd key score member [score member...]
- 按照元素分数从小到大排序，返回索引从start到stop之间的所有元素：zrange key start stop [withscores]
- 获取元素分数：zscore key member
- 删除元素：  zrem key member [key member...]
- 获取指定分数范围的元素： zrangebyscore key min max [withscores] [limit offset count ]
- 增加某个元素的分数：zincreby key increment member 
- 获取集合中元素数量：zcard key
- 获取指定分数范围内的元素个数：zcount key mix max
- 按照排名范围删除元素： zremrangebyrank key start stop
- 获取元素排名：从小到大：zrank key member;   从大到小:  zrevrank key member



**应用场景**

排序

1. 根据商品销售对商品进行排序显示
2. 抖音热搜

展示当日排名前10 条：zrevrange hotvcr: 20200919 0 9 withscores

questions：

分页 

list做法

zset做法

区别



#### 6. bitmap

![](E:\Typora\Notes\图片\bitmap.jpg)

> Bit arrays ，我们称之为位图
>
> bitmap 底层是string类型
>
> 位图本质是数组，该数组是由二进制数 0 1组成,每一个二进制位都对应一个偏移量（称之为一个索引或位格）
>
> bitmap支持的最大位数是2^32位，可以极大节省空间，使用512M内存就可以存储42.9亿（2^32）的字节信息



基本命令

- 给指定key值的第offset赋值val： setbit key offset value
- 获取指定key值的offset位： gitbit key offset
- 返回指定key中[start,end]中为1的数量：bitcount key [start end]
- 对不同二进制存储数据进行位运算（AND，OR，NOT，XOR）：bitop operation destkey key 
- strlen：统计字节数占用多少
- ​

**bitmap底层编码说明**

![](E:\Typora\Notes\图片\bitmap底层原理.jpg)

对应的ascii码值

**应用场景**

- 用户用户统计：yes/no， 

- 用户是否登陆过，（Y,N），比如京东每日签到送京豆

- 电影，广告是否被点击播放过

- 钉钉打卡，签到统计

- 真实案列

  - 日活统计
  - 连续签到打卡
  - 统计用户一年之内的登陆天数
  - 一年内，那几天登陆过？那几天没登陆？

  **签到用户体量大时（千万上亿的数据量）** 如何解决这个痛点：

  > 1. 一条签到对应一条记录，会占据越来越大的空间
  > 2. 一个月最多31天，刚好bitmap int型是32位，这样一个int型就可以存储一个月
  > 3. 一条数据直接存储一个月的签到记录

  ```shell
  # 按月统计
  # 202205 1号 签到
  setbit sign:u1 202205 1 1
  setbit sign:u1 202205 30 1

  # 按年统计
  setbit sign:u1 2022 5 1
  setbit sign:u1 2022 364 1
  ```

  一亿位bitmap约占内存12M（10^8/8/1024/1024）

  ​

#### 7. hyperloglog

**专业名词**

- UV：Unique Vistor，独立访客，一般理解为客户端IP；需要去重考虑
- PV：Page View ，页面的浏览量，不用去重
- DAU：Daily Active User ， 日活跃用户量，常用于反应网站，互联网应用或者网络游戏的运营情况
- MAU：Monthly Active User，月活跃用户量

**看需求**

- 统计网站的UV，某个文章的UV
- 用户搜索网站关键词的数量
- 统计用户每天搜索不同词条个数

**是什么**

- 去重复统计功能的基数估计法—就是Hyperloglog；在redis中，每个Hyperloglog只需要花费12kb内存，就可以计算接近2^64个不同元素
- 基数：是一种数据集，去重后的真实个数

**HyperLogLog如何做的？如何演化出来的？**

- 基数统计就是HyperLogLog

- 去重复统计方法：redis set，bitmap（该网站 访问过1，没有访问0，但是对于亿级数据会有问题，需要存储大数据）

- HyperLogLog是一种概率算法的实现，通过牺牲准确率来换取空间，对于不要求绝对准确率的场景可以使用，应为概率算法**不直接存储数据本身**，

  通过一定的概率统计方法预估基数值，同时保证误差在一定范围内，不存储数据所以可以大大节约内存。

**原理说明**

- 不是集合也不保存数据，只是记录数量而不是具体内容
- 有误差 仅为0.81%
- ​

**基本命令**

- pfadd key element：将所有元素添加到key中
- pfcount key：统计key的估算值（不准确，误差率0.81%）
- pfmerge new_key key1 key2：合并key1，key2到新key中

#### 8.GEO

类型是Zset

**基本命令**

- geoadd：多个经度（longitude）、纬度（latitude）、位置名称(member)添加到指定的key
- geopos：从给定key里面返回所有指定名称(member)的位置（经度和纬度）
- geodist：返回两个给定位置之间的距离
- georadius：以给定的经纬度为中心，返回与中心的距离不超过给定最大距离的所有地理位置集合（以半径为中心，查找附近的XX）
- georadiusbymember：跟georadius类似
- geohash：返回一个或多个位置元素的geohash表示

**实例**

```shell
#geoadd key longitude latitude member [longitude latitude member...]
geoadd city 116.403963 39.915119 "天安门"

#geopos key member [member...]
geopos city 天安门 故宫

#geodist key member1 member2 [m|km|ft|mi]
geodist city 天安门 故宫 m
 
georadius city  116.403963 39.915119 10 km withdist withcoord count 10
# withdist: 返回位置的同时，将位置元素与中心之间的距离也一并返回
# widthcoord：将位置元素的经纬度也一并返回
# withhash：以52位有符号整数的形式，返回位置元素经过原始geohash编码的有序集合分值。这个选项主要用于底层应用和调试，实际中作用不大
# count 限定返回的记录数

#geohash key member
geohash city 天安门 故宫

```

### 2. 布隆过滤器(BloomFilter)

**考题**

> 现在有50亿个电话号码，现有10万个电话号码，如何快速准确判断这些电话号码已经存在？
>
> 50亿*8字节 大约40G
>
> 通常我们判断一个元素是否存在于集合中的场景，一般是想到把集合中的所有元素保存起来，然后在比较确定。链表，树，散列表，等等数据结构都是这个思路



**是什么**

> 布隆过滤器是一个很长的二进制数组 + 一系列随机hash算法映射函数，主要用于判断一个元素是否在集合中。（bitmap + 分布均与的多个hash算法构成的集合）
>
> 

### 面试

#### 一：

- App中的每天的用户登录信息： 1天对应1系列用户ID或者移动设备ID；
- 电商网站上商品的用户评论列表：1个商品对应1系列的评论；
- 用户在手机App上的签到打卡信息：1天对应1系列用户的签到记录；
- 应用网站上的网页访问信息：1个网页对应1系列的访问点击；

#### 二：

- 在移动应用中，需要统计每天新增用户数和第2天的留存用户数；
- 电商网站上商品评论中，需要统计评论列表中最新的评论；
- 在签到打卡中，需要统计一个月内连续打卡的用户数；
- 在网页访问记录中，需要统计独立访客量；

痛点：类似今日头条，抖音，淘宝，这样的访问都是亿级别，请问如何处理？



**亿级数据的收集+统计**

存的进 + 取的快 + 多统计

1. 数据收集
2. 数据清洗
3. 数据统计




### redis缓存案列

对于新增和修改：（保证数据一致性）

1. 新增该条数据至mysql
2. 查询mysql（将该条数据捞出来）
3. 2步骤查出来的是写入redis的一致性数据

查询：

1. 先从redis里面查询，没有再去查询mysql
2. redis无，查询mysql
3. mysql有，需要将数据写回redis，保证下一次缓存命中



缓存击穿和缓存穿透

缓存击穿： 击穿，数据之前有击中，redis上的数据过期了（之前有的数据，某一时刻大量过期了，对于热点数据很致命），直接去查mysql

缓存穿透： redis上没有的数据，直接查询mysql，可能导致mysql崩溃



预防redis击穿，打爆mysql做法：（双端检索思想）

1. 对于在redis没有查询到的数据，进来就先加锁，保证一个请求操作，让外面的redis等待一下，避免缓存击穿
2. 加锁后再查redis，二查redis还是null，可以去查询mysql
3. 查询mysql有数据，需要写回redis，完成数据的一致性工作

