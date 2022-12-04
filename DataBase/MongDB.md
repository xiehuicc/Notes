### 1.MongDB的相关概念	

#### 1）业务应用场景

>MongoDB可以应对三高的需求：
>
>- 对数据库高并发读写的需求
>- 对海量数据的高效率存储和访问的需求
>- 对数据库的高扩展性和高可用性的需求

**具体的应用场景**

- 社交场景，使用MongoDB存储用户信息，以及用户发表朋友圈信息，通过地理位置索引实现附近的人，地点等功能
- 游戏场景，使用MongoDb存储游戏用户信息，用户装备，积分等直接以内嵌文档的形式存储，方便查询，高效存储和访问。
- 物流场景，使用MongoDB存储订单信息，订单状态在运送过程不断更新，以MongDB内嵌数组的形式来存储，一次就能将订单所有的变更读取出来。
- 物联网场景，使用MongoDB存储所有接入的智能设备信息，以及设备汇报的信息日志，并对这些信息进行多维度分析。
- 视频直播，使用MongoDB存储用户信息，点赞互动信息等



这些应用场景中，数据操作方面的共同特点是：

- 数据量大
- 写入操作频繁
- 价值较低的数据，对事物性要求不高



#### 2）MongoDB简介

> Mongodb是一个开源，高性能，无模式(动态模式)的文档型数据库，是NoSQL数据产品中的一种，是最像关系型数据库(MySQL)的非关系型数据库。
>
> 它支持的数据结构非常松散，是一种类似JSON的格式叫BSON
>
> MongoDB中记录的是一个文档，它是由字段和值对(field:value)组成的数据结构

**MySQL和MongoDB的对比**

![image-20210417154722923](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210417154722923.png)

### 2.MongoDB的安装和连接



### 3.基本常用命令

查看数据库：

```
show dbs
或
show databases
```

> ==注意==：在MongoDB中，集合只有在内容插入后才会创建，就是说，创建集合(数据表)后要插入一个文档(记录),才会真正创建

**删除数据库：**

```
db.dropDatabase()
```

集合的显示创建：

```
db.createCollection("name")
```

**集合的隐式创建**

```
 db.name.drop()
```

##### 3.1 文档的分页查询

**分页列表查询**

> 可以使用limit()方法来读取指定数量的数据，使用skip()方法来跳过指定数量的数据

基本语法

```js
db.Collection_name.find().limit(NUMBER).skip(NUMBER)
```

##### 3.2 排序查询

> sort()方法对数据进行排序，sort()方法可以通过指定排序的字段，并使用1和-1来指定排序的方式，其中1为升序，-1位降序

基本语法

```js
db.Collection_name.find().sort({KEY:1})
或
db.Collection_name.find().sort(排序方式)
```

例如：

对userId降序排序，并对访问量进行升序排序

```js
db.Collection_name.find().sort({userId:-1,likeNum:1})
```

##### 3.3文档的更多查询

###### 3.3.1正则的复杂条件查询

###### 3.3.2比较查询

###### 3.3.3包含查询

> 包含查询使用$in操作符
>
>  不包含查询使用$nin 操作符

示例：查询集合中userId字段包含1003和1004的文档

```js
db.Collection_name.find({userId:{$in:["1003","1004"]}})
```

###### 3.3.4 条件连接查询

> 如果需要查询同时满足两个以上条件，需要使用$and操作符 将条件进行关联。相当于SQL中的and

格式为：

```js
$and:[{},{},{}]
```

示例：查询集合中likenum大于700小于2000的文档

```js
db.Collection_name.find({$and:[likenum: {$gt:NumberInt(700)},{likenum: {$lt:NumberInt(2000)}]})
```

> 如果两个以上条件之间是或者的关系，我们使用 $or操作符进行关联，

格式为：

```js
$or:[{},{}]
```



### 4. 索引

#### 4.1 概述

> 索引支持在Mongodb中高效的查询。如果没有索引，mongodb必须执行全集合扫描。这种扫描全集合的查询效率非常低的，在处理大量数据时，可能花费几十秒甚至几分钟
>
> 如果查询存在适当的索引，mongodb可以使用该索引限制必须检查的文档数。。
>
> 索引是特殊的数据结构，它易于遍历的形式存储集合数据集的一小部分。索引存储特定字段或一组字段的值，按照字段值排序。索引项的排序支持有效的相等匹配和基于范围的查询操作。此外，mongodb还可以用索引中的排序返回排序结果



#### 4.2索引的类型

##### 4.2.1单字段索引

> mongodb支持在文档的单个字段上创建用户定义的升序/降序索引，称为单字段索引
>
> 对于单个字段索引和排序操作，索引键的排序（升序/降序）并不重要，因为mongodb可以在任意方向上遍历索引。

##### 4.2.2 复合索引

> mongodb支持多个字段的用户定义索引，即复合索引。
>
> 复合索引中列出字段顺序有重要意义。例如：如果复合索引由{userId：1,score:-1}组成，则索引首先按userId升序排序，然后在userId的值内，再按score降序排序。

##### 4.2.3 其他索引

地理空间索引、文本索引、哈希索引

#### 4.3.索引的管理操作

##### 4.3.1 索引的查看

说明：

返回一个集合中所有索引的数组

语法：

```js
db.collection.getIndexs()
```

提示：该语法需要MongoDB3.0+

##### 4.3.2 索引的创建

说明：

在集合上创建索引

语法：

```js
db.collection.createIndexs(keys,options)
```



##### 4.3.3 索引的移除

说明：

在集合上指定删除索引

语法：

```js
// 删除单个索引
db.collection.dropIndex(index)

// 删除所有索引
db.collection.dropIndexs()
```
