### 1.  redis 5 中数据结构

命令不区分大小写，key区分大小写

1. **String**  (simple Dynamic String 简称 sds)

   - 最常用： set key value 	get key

   - 同时设置/获取多个键值： mset key value[key value....]		mget key[key....]
   - 数值增减：递增数字 incr key； 增加指定整数：incrby key increment; 递减数值：decr key； 减少指定整数：decrby key decrement
   - 获取字符串长度：STRLEN key
   - 分布式锁：setnx key value;   setnx key value [EX seconds] [Px millsecon] [nx|xx]

   **应用场景**

   储存key-value 键值对

   点赞；

   转发量；

   是否喜欢的文章；

2. **list** （列表）

   > 底层是链表结构，list的插入和删除操作非常块，时间复杂度为O(1)，不像数组结构插入、删除操作需要移动数据。

   **应用场景**

   - 消息队列： lpop 和 rpush (或者反过来，lpush和rpop)能实现队列的功能；
   - 朋友圈的点赞列表、评论列表、排行榜：lpush命令和lrange命令能实现最新列表的功能，每次通过lpush命令往列表插入新的元素，然后通过lrange命令读取最新的元素列表。



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