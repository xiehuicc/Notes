**基础篇**

### 1. mysql概念

#### 1.1 数据库相关概念

- 数据库：DataBase（DB）存储数据的仓库，数据是有组织的进行存储的
- 数据库管理系统：DataBase Management System （DBMS）操纵和管理数据库的大型软件
- SQL：操作关系型数据库的编程语言，定义了一套关系型数据库的统一标准



#### 1.2 数据模型

- 关系型数据库：RDBMS

> 概念：建立在关系模型基础上，由多张相互连接的二维表组成的数据库
>
> 特点：
>
> 1. 使用表存储数据，格式统一，便于维护
> 2. 使用sql语言操作，标准统一，使用方便



### 2. SQL语法及分类

- 语法



- 分类
  - DDL：(data defined language)数据定义语言，用来定义数据库对象（数据库，表，字段）
  - DML：(data maniputlation language)数据操作语言，用来对数据库表中的数据进行增删改
  - DQL：(Data query language)数据查询语言，用来查询数据库中表的记录
  - DCL：(data controller language)数据控制语言，用来创建数据库用户，控制数据库的访问权限

#### 2.1 DQL

语法

```sql
select 字段列表 from 表名 where 条件列表 group by 分组字段列表 having 分组后条件列表 order by 排序字段列表 limit 分页参数
```

- 基本查询

  **去除重复数据**

  ```sql
  selest distinct  字段名 form 表名
  ```


- 条件查询(where)

> 不等于：<> 或!= 
>
> between ... and ... ：在某个范围之内
>
> in(...)：在in之后的列表中的值，多选一
>
> like：占位符，模糊匹配，（'_'匹配单个字符，''%''匹配任意个字符）
>
> is null ：是null
>
> and 或 &&： 并且
>
> or 或 ||： 或者
>
> not 或 !：非



- 聚合函数(count,max,min,avg,sum)

> 将一列数据作为一个整体，进行纵向计算

语法：

```sql
selsect 聚合函数(字段) from 表名
```

null值不参与聚合函数的计算



- 分组查询(group by)

语法：

```sql
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤]
```

**where 和having的区别**

1. 执行时机不同：where 是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤
2. 判断条件不同：where 不能对聚合函数进行判断，而having可以

**注意：**

执行顺序：where > 聚合函数 > having

分组之后：查询的字段一般为聚合函数和分组字段，查询其他字段无意义



- 排序查询(order by)

语法：

```sql
select 字段列表 form 表明 order by 字段1 排序方式1， 字段2 排序方式2
```



- 分页查询(limit)

语法：

```sql
select 字段列表 from 表名 limit 起始索引，查询记录数;
```

注意：

1. 起始索引从0开始，起始索引 = （查询页码 - 1）* 每页记录数
2. 分页查询是数据库方言（其他数据库不是limit关键词）mysql是limit
3. 如果查询的是第一页数据，起始索引可以沈略，直接limit 10



**DQL执行顺序**

```sql
select 
	字段列表 	4
from 
	表名		  1
where 
	条件列表	2
group by
	分组字段列表	3
having
	分组后条件列表	5
order by
	排序字段列表	6
limit 
	分页参数	7

可以通过别名进行验证
```



#### 2.2 DCL

**DCL-管理用户**

查询用户

```sql
use mysql;
select *from user;
```

创建用户

```sql
create user '用户名' @'主机名' identified by '密码'
```

修改用户密码

```sql
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
```



**DCL-权限控制**

1、 查询权限

```sql
show grants for '用户名'@'主机名';
```

2、授予权限

```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';

# 所有权限
grant all on ...
```

3、撤销权限

```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
```

注意：

1. 多个权限之间，逗号隔开
2. 授权时，数据库名和表名可以使用*进行统配



### 3. 函数

#### 3.1 字符串函数

#### 3.2 数组函数

#### 3.3 日期函数

#### 3.4 流程控制函数

- if(value, t,f) 如果value为true，返回t 否则f
- ifnull(value1,value2)  如果val1不为空，返回val1，否则val2
- case when [val1] then [res1]... else [default] end 如果val1的为true，返回res1，否则返回default默认值
- case [expr] when [val1] then [res1]... else [default] end 如果expr的值等于val1，返回res1，否则返回default默认值



### 4.约束

#### 4.1 概述

1. 概念

> 约束是作用于表中字段上的规则， 用于限制存储在表中的数据

2. 目的

> 保证数据库中数据的完整性，有效性和正确性

3. 分类

- 非空约束：不能为null ；NOT NULL
- 唯一约束：保证字段唯一，不重复；UNIQUE
- 主键约束：主键是一行数据的唯一标识，要求唯一且非空；Primary key
- 默认约束：保存数据时，未指定时，采用默认值； default
- 检查约束（8.0.16版本后）：保证字段满足某一条件； check
- 外键约束：用来让两张表的数据之间建立连接，保证数据的一致性和完整性；foreign key



#### 4.2 主键约束

#### 4.3 外键约束

> 拥有外键的数据的表称为子表（从表），外键所关联的这张表称为父表（主表）
>
> 用来让两张表的数据之间建立连接，保证数据的**一致性**和**完整性**；有相关联的数据不能删除；

语法：

添加外键：

```sql
create table 表名(
	字段名 数据类型,
  	...
  [constraint] [外键名称] foreign key (外键字段名) references 主表(主表列名)
)；

或
alter table 表名 add constraint 外键名称 foreign key（外键字段名）references 主表（主表列名）;

### example constraint(约束)
alter table emp add constraint fk_emp_dept foreign key(dept_id) refrences dept(id);

```

删除外键

```sql
alter table 表名 drop foreign key 外键名
```

**外键约束中的 删除和更新行为**

- no action：当在父表中删除/更新记录时，首先检查记录是否有外键，如果有则不允许删除/更新（与restrict一致）
- restrict： 与no action 一致
- cascade：当在父表中删除/更新记录时，首先检查记录是否有外键，如果有，则也删除/更新外键在子表中的记录
- set null ：当在父表中删除/更新记录时，首先检查记录是否有外键，如果有，则设置子表中该外键值为null
- set default：父表有更新时，子表将外键设置成一个默认值(innodb不支持)

```sql
alter table 表名 add constraint 外键名称 foreign key(外键字段) references 主表(主表列名) on update cascade on delete cascade;
```



### 5. 多表查询

#### 5.1 多表关系

- 一对一（多对一）

  案例；部门与员工的关系

- 多对多

  案例： 学生与课程的关系

  实现：建立第三张**中间表**，中间表至少包含两个外键，分别关联两个主键，以此来维护两张表的关系

- 一对一

  案例；用户与用户详情

  关系：一对一关系，多用于单表拆分，将一张标的基础数据放在一张表中，其他详情数据放在另一张表中，以提升操作效率

  实现：在任意一方加入外键，关联另一方的主键，并设置外键为唯一的unique

#### 5.2 多表查询概述

笛卡尔积；两个集合A集合和B集合的所有组合情况（A*B）在多表查询中，需要消除无效的笛卡尔积，可以通过外键关联的字段

**多表查询分类**

- 连接查询
  - 内连接：相当于查询A,B交集的部分数据
  - 外连接；
    - 左连接查询：查询**左表**所有数据，以及两张表交集部分数据
    - 右连接查询：查询**右表**所有数据，以及两张表交集部分数据
  - 自连接：当前表与自身的连接查询，自连接必须使用表别名
- 子查询



#### 5.3内连接

语法：

隐式内连接：

```sql
select 字段列表 from 表1,表2 where 条件
```

显示内连接：

```sql
select 字段列表 from 表1 [inner] join 表2 on 连接条件
```



#### 5.4 外连接

语法

左外连接：

```sql
select 字段列表 from 表1 left  outer join 表2 on 条件
```

查询出表1的所有数据 和 表1表2交集的数据

左表的数据将会全部展示出来，而右表只会显示符合条件的记录

右外连接：

```sql
select 字段列表 from 表2 right outer join 表1 on 条件
```

#### 5.5 自连接

可以想象成两张表，

比如：查询员工表的，员工，及其领导名

语法：自连接必须使用表别名

```sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件
```



#### 5.6 联合查询（union union all）

> 对于联合查询，就是把多次的查询结果合并起来，形成一个新的查询结果集。

语法：

```sql
select 字段列表 from 表A ...
union [all]
select 字段列表 from 表B ...
```

union all  直接把查询的结果进行合并

union 查询的结果会去重

**注意**

对于联合查询的多张表的列数必须保持一致，字段类型也要保持一致



#### 5.7 子查询

> SQL语句中嵌套select语句，称为**嵌套查询**，又称子查询

语法：

```sql
select * from table1 where column1 = (select column1 from table2)
```

子查询外部的语句可以是insert/update/delete/select的任何一个 



p48 -p49多表查询练习





### 6. 事务

> 事务是 一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或者撤销操作请求，即这些操作**要么同时成功，要么同时失败**。
>
> 默认MySQL的事务是自动提交的，也就是说，当执行一条DML语句时，MySQL会立即隐式的提交事务。



#### 6.1  事务操作

查看/设置事务提交方式

```sql
select @@autocommit
# 设置为手动提交
set @@autocommit=0
```

提交事务

```sql
commit;
```

回滚事务

```sql
rollback
```

开启事务

```sql
start transaction 或 begin;
```



#### 6.2 事务四大特性（ACID）

- 原子性（Automicity）:事务是不可分割的最小操作单元，要么全部成功，要么全部失败
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性（Durability）：事务一旦提交或者回滚，它对数据库中的数据的改变就是永久的



#### 6.3 并发事务问题

1. 脏读

   一个事务读到另一个事务还**没有提交**的数据

2. 不可重复读

   一个事务先后读取同一条记录，但**两次读取的数据不同**，称之为不可重复读

3. 幻读

   一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在了，好像出现了幻影

   例如：在同一事务中就可能出现第一次查询没有，第二次查询就有，出现insert 出现重复数据



#### 6.4 事务的隔离级别

| 隔离级别                | 脏读   | 不可重复读 | 幻读   |
| ------------------- | ---- | ----- | ---- |
| Read uncommited     | √    | √     | √    |
| Read commited       | ×    | √     | √    |
| Repeatable Read（默认） | ×    | ×     | √    |
| Serializable        | ×    | ×     | ×    |

MySQL 默认隔离级别是Repeatable Read(可重复读) ，有可能会出现幻读的情况；

从上往下性能越差；

事务隔离级别越高，数据越安全，但性能越低。



查看事务隔离级别

```sql
select @@transaction_isolation
```

设置事务隔离级别

```sql
set [session|global] transaction isloation level {Read uncommited | Read commited | Repeatable Read | serializable}
```



### 7. 存储引擎

#### 7.1 MySQL体系结构 

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9d1701cbbf9409bb671a72ae7a61466~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

连接层；

服务层

引擎层

存储层



#### 7.2 存储引擎简介

> 存储引擎就是存储数据，建立索引，更新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也被称为表类型。

在创建表时，指定存储引擎

```sql
create table 表名(
	字段1 字段类型 [comment 字段1注释]
  	...
 
)ENGIN = INNODB [comment 表注释];
```

查看数据库支持的引擎

```sql
show engins; 
```



#### 7.3 存储引擎特点



##### 1. innodb

介绍

> innodb 是一种兼顾可靠性和高性能的通用存储引擎，在MySQL5,5之后，Innodb是默认的MySQL存储引擎

特点

- DML（数据的增删改语句）操作遵循ACID模型，支持**事务**
- **行级锁**，提高并发访问性能
- 支持**外键** foreign key 约束，保证数据的完整性和正确性

文件

xxx.ibd：xxx代表表名，innodb引擎的每一张表都会有对应一个这样的表空间文件，存储标的结构，数据和索引

参数：innodb_file_per_table



逻辑存储结构：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2f334a95e22b494d9accac7db6a65cee~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.image)

##### 2. MyISAM

介绍：

> MyISAM是早期Mysql的默认存储引擎

特点：

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快



##### 3. Memory

介绍：

> Memory 引擎的数据是存储再内存中的，由于受到硬件问题，或者断电问题的影响，只能将这些表作为临时表或者缓存使用

特点

- 内存中存放
- hash索引（默认）



#### 7.4 存储引擎选择



- Innodb：是Mysql的默认存储引擎，支持事务，外键。如果应用对事物的完整性有比较高的要求，在并发条件下要求数据的一致性，数据除插入和查询之外，还包含很多更新和删除操作，那么Innodb存储引擎比较合适
- MyISAM：如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性，和并发性要求不高时，MyISAM引擎比较适合
- Memory：将所有的数据保存在内存中，访问速度快，通常用于临时表及缓存。Memory的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保证数据的安全性。



### 8. 索引（Index）



#### 8.1 索引概述

**介绍：**

> 索引（index）是帮助Mysql**高效**获取数据的**数据结构**（有序）。在数据之外，数据库还维护着满足快速特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就可以在这些数据上实现高级查找算法，这种数据结构就是索引。



**优点**：

- 提高数据检索效率，降低数据库IO成本
- 通过索引列对数据库进行排序，降低数据排序成本，降低CPU的消耗

**缺点：**

- 索引也是需要占用空间的
- 提高查询效率，但却降低了更新表的速度，如对表进行Insert，Update，Delete时，效率降低。



#### 8.2 索引结构

- B+ Tree索引：大部分引擎支持B+树索引
- Hash索引：底层数据结构是用hash表实现的，只有精确匹配索引的查询才有效，不支持范围查询
- R-Tree(空间索引)：是MyISAM引擎的一种特殊索引类型，主要用于地理空间数据类型，通常使用较少
- Full-text(全文索引)：是通过建立倒排索引，快速匹配文档的方式。



#### 8.3 B-Tree（多路平衡查找树）

以一棵最大度数为5（5阶）的b-tree为例，每个节点最多存储4个key，5个指针



#### 8.4 B+ Tree

以一颗最大度数为（max-degree）为4（4阶）的b+Tree

![image-20221204145902594](https://github.com/xiehuicc/Notes/blob/main/Images/image-20221204145902594.png)

![image-20221204145911175](C:/Users/xiehui/AppData/Roaming/Typora/typora-user-images/image-20221204145911175.png)相对于B-Tree区别：

- 所有的数据都会出现在叶子节点
- 叶子节点形成一个单向链表



#### 8.5 Mysql中的B+Tree

MySQL索引数据结构对经典B+Tree进行了优化，在原有B+Tree的基础上，增加了一个指向相邻节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问性能。

![image-20221204150051804](https://github.com/xiehuicc/Notes/blob/main/Images/image-20221204150051804.png)

![image-20221204150114909](C:/Users/xiehui/AppData/Roaming/Typora/typora-user-images/image-20221204150114909.png)



#### 8.6 Hash索引

> hash索引就是采用一定的hash算法，将键值换成新的hash值，映射到对应槽位上，然后存储在hash表中。
>
> 如果两个（或多个）键值，映射到同一槽位上，就产生了hash冲突（也称为hash碰撞），可以通过链表来解决，



**hash索引特点**

1. hash索引只能用于对等比较（= ，in），不支持范围查询（between，<，>，...）
2. 无法利用索引完成排序操作
3. 查询效率高，通常只需要一次检索就可以了，效率通常要高于B+Tree索引



**存储引擎支持**

在MySQL中，支持hash索引的是Memory引擎，而Innodb中，具有自适应hash功能，hash索引是存储引擎根据B+Tree索引在指定条件下构建的。

​	

#### 8.7 思考选择B+Tree原因



**为什么InnoDB引擎选择使用B+Tree索引结构？**

- 相对于二叉树，层级更少，搜索效率高；
- 对于B-tree，无论是叶子节点还是非叶子节点，都会保留数据，这样导致一页中存储的键值减少，指针跟着减少。要存储相同的数据量，B-Tree的层级会更高，导致性能降低。（关键是在树的高度上）



#### 8.8 索引的分类

| 分类   | 含义                       | 特点           | 关键字      |
| ---- | ------------------------ | ------------ | -------- |
| 主键索引 | 针对表中主键创建的索引              | 默认自动创建，只能有一个 | PRIMARY  |
| 唯一索引 | 避免一个表中某数据列中的值重复          | 可以有多个        | UNIQUE   |
| 常规索引 | 快速定位特定数据                 | 可以有多个        |          |
| 全文索引 | 全文索引查找的是文本种的关键词，而不是索引中的值 | 可以有多个        | FULLTEXT |



在InnoDB存储引擎中,根据索引的储存形式，又可以分为两种：

| 分类                    | 含义                           | 特点        |
| --------------------- | ---------------------------- | --------- |
| 聚集索引（Clustered Index） | 将数据存储于索引放在一起，索引结构的叶子节点保存了行数据 | 必须有，且只有一个 |
| 二级索引（Secondary Index） | 将数据与索引分开存储，索引结构的叶子节点关联的是对应主键 | 可以存多个     |



聚集索引的选取规则：

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引
- 如果没有主键，也没有唯一索引，则InnoDB会自动生成一个rowId作为隐藏的聚集索引



![image-20221204153731656](https://github.com/xiehuicc/Notes/blob/main/Images/image-20221204153731656.png)

![image-20221204153743245](C:/Users/xiehui/AppData/Roaming/Typora/typora-user-images/image-20221204153743245.png)



**聚集索引叶子节点挂的是数据；**

**二级索引叶子节点挂的是id；**



例子：

查询的是name=‘Arm‘，先到二级索引中找到Arm，通过id再到聚集索引中查找该id下存储的数据

![image-20221204153952165](https://github.com/xiehuicc/Notes/blob/main/Images/image-20221204153952165.png)

![image-20221204153956927](C:/Users/xiehui/AppData/Roaming/Typora/typora-user-images/image-20221204153956927.png)



#### 8.9 思考题



**InnoDB主键索引的B+Tree高度为多高呢？**

![image-20221204154534275](https://github.com/xiehuicc/Notes/blob/main/Images/image-20221204154534275.png)

![image-20221204154539812](C:/Users/xiehui/AppData/Roaming/Typora/typora-user-images/image-20221204154539812.png)

高度为3的B+Tree就可以存储两千多万的记录



#### 8.10 索引语法



**创建索引**

```sql
create [UNIQUE][FULLTEXT] index index_name on table_name (index_col_name,...);
```



**查看索引**

```sql
show index from table_name;
```



**删除索引**

```sql
drop index index_name on table_name;
```



例子：（user表）

1. name字段为姓名字段，该字段的值可能重复，为该字段创建索引

   ```sql
   create index idx_user_name on user(name)
   ```


2. phone手机号字段的值，是非空，且唯一的，为该字段创建唯一索引

   ```sql
   create UNIQUE index idx_user_phone on user(phone)
   ```


3. 为profession，age，status，创建联合索引

   ```sql
   create index idx_user_pro_age_sta on user(profession,age,status)
   ```


4. 为email建立合适的索引来提高查询效率

   ```sql
   create index idx_email on user(email)
   ```


#### 8.11  SQL性能分析



**SQL执行频率**

MySQL查看当前数据库的Insert ，Update，delete，select的访问频率

session: 查看 当前会话信息

global：全局

```shosql
show [session|global] status

# 模糊查询，Com后面七个字符
show global status like 'Com_______'
```



**慢查询日志**

> 慢查询日志记录了执行时间超过指定参数（long_query_time ,单位：秒，默认10s）的所有SQL语句日志

MySQL的慢查询日志默认未开启，需要在MySQL配置文件（/etc/my.cnf）中配置如下信息：

```
# 开启mysql慢查询日志
slow_query_log = 1
# 设置慢日志的时间为2s，SQL语句执行时间超过2s，就会视为慢查询，记录慢查询日志
long_query_time = 2
```

慢日志文件中的记录信息在：

/var/lib/mysql/localhost-slow.log



**profile详情**

show profile是能够在做SQL优化时，帮助我们了解时间都耗费在哪里。通过have_profiling参数，能够看到当前MySql是否支持profile操作：

```sql
# 查看是否支持profile
select @@have_profiling;

# 默认profiling是关闭的
# 开启
set [session|global] profiling = 1
```



**explain执行计划**

explain或者desc命令获取mysql如何执行select语句信息，包括在select语句执行过程中，如何连接表和连接表顺序

语法：

```sql
# 
explain select 字段列表 from table where 条件
```
explain 执行计划个字段的含义：

- id：	select 查询的序列号，表示查询中执行select子句或者是操作表的顺序（id相同，执行顺序从上到下；id值不同，值越大，越先执行）（应为有可能多表查询，id会有多个）
- select_type：表示select的类型，常见的取值有simple（简单表，即不使用表连接或者子查询）、primary（主查询，即外层查询）、union、subQuery等
- type：表示连接类型，性能由好到差的连接类型为null，system，const，eq_ref，ref，rang，index，all
- prossible_key：显示可能应用在这张表上的索引，一个或多个。
- key：实际使用的索引，如果为null，则没有使用索引
- key_len：表示索引中使用的字节数，改值为索引字段最大可能长度，并非实际使用长度，在不损失经度的前提下，长度越短越好
- rows：mysql认为必须要执行的查询的行数，在Innodb引擎表中，可能是一个估计值，可能不总准确
- filtered：表示返回结果的行数占需读取行数的百分比，filtered值越大越好。 



#### 8.12 索引使用

 **验证索引效率**

 为建立索引时查看 执行SQL耗时-->针对字段创建索引 -->执行相同SQL语句，查看耗时



**索引使用**



**索引失效的情况：**

- 最左前缀法则：

  如果索引了多列（联合索引），要遵循最左前缀法则。指的是查询从索引的最左列开始，并且不能跳过索引中的列。如果跳过某一列，索引将部分失效（后面的字段索引失效）

  ```sql
  explain select *from tb_user where professoin = '软件工程' and age = 31 and status = '0'

  # 索引失效，因为不符合最左前缀法则
  explain select *from tb_user where  age = 31 and status = '0'

  # 索引部分（status）失效
  explain select *from tb_user where  age = 31

  # 索引部分（age,status）失效
  explain select *from tb_user where  professoin = '软件工程' and status = '0'

  # 索引成功的，因为只要最左的字段存在
  explain select *from tb_user where status = ’0 and age =31 and professoin = '软件工程'
  ```

  ​

- 范围查询

  联合索引中，出现范围查询(>,<)，范围查询右侧的列索引失效。

  ```sql
  # 索引部分失效（status）
  explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0'

  # 走索引（所以尽量使用>=或<=）
  explain select * from tb_user where profession = '软件工程' and age >= 30 and status = '0'
  ```



- 索引列运算

  不要在索引列上进行运算操作，否则索引将失效。

- 字符串不加引号

  字符串字段类型使用时，不加引号，索引将失效

- 模糊查询

  如果仅仅是尾部模糊匹配，索引不会失效。如果头部模糊匹配，索引将失效

- or连接的条件

  用or分割开的条件，如果or前的条件中的列有索引，而后面的列没有索引，那么涉及的索引都不会被用到。

  ```sql
  # 如：phone有索引，age没有索引
  # 索引将失效
  select * from tb_user phone='1331162808' or age = 31
  ```

  解决办法：只需要将age也建立索引

- 数据分布影响

  如果MySql评估使用索引比全表更慢，则不使用索引。

  ```sql
  # 如果 数据库中所有的phone都大于这个，走完索引，还需要回表查询，这比直接全表查询慢，mysql会直接不走索引。
  select * from tb_user where phone > '1779999005'

  select * from tb_user where phone >= '1779999015'
  ```

  ​

**SQL提示**

SQL提示，是优化数据库的一个重要手段，简单来说就是在SQL语句中加入一些人为的提示来达到优化操作的目的

use index(如果一个字段有单列索引 又有联合索引，不指定的话mysql会自动选择)

```sql
select *from tb_user use index(idx_user_pro) where profession = '软件工程'
```

ignore index

```sql
select *from tb_user ignore index(idx_user_pro) where profession = '软件工程'
```

force index

```sql
select *from tb_user force index(idx_user_pro) where profession = '软件工程'
```



第一个use 是建议mysql，第三个force是强制使用；



**覆盖索引**

> 尽管使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到）减少使用select *
>
> 覆盖索引可以避免回表查询（只需要返回联合索引的字段时）



**前缀索引**

> 当字段类型为字符串（varchar, text 等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费磁盘io，影响查询效率。此时，可以只将字符串的一部分前缀，建立索引，这样可以大大节省空间。

语法：

```sql
# 前n个字符建立索引
create index idx_xxx on table_name（column(n));
```

前缀长度：

可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和表记录数比值，选择性越高，查询效率越高

 

#### 8.13 索引设计原则

1. 针对数据量较大，且查询较为频繁。（一百万以上的数据量）
2. 针对于常作为查询条件（where），排序（order by），分组（group by） 操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
4. 如果是字符串类型的字段，字段的长度较长，可以针对于字段特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时联合索引有时候可以覆盖索引，避免回表查询，提高查询效率
6. 要控制索引的数量，索引不是多多益善，索引越多，维护索引的代价越大，会影响增删改的效率
7. 如果索引列不能存储null值，请在创表时使用 not null约束它。当机器知道每列是否包含null值时，他可以更好的确定那个索引最有效的用于查询。



### 9. SQL优化



#### 9.1 插入数据优化

- insert插入

  批量插入：

  ```sql
  insert into tb_user values(1,'Tom'),(2,'cat'),(3,'Jerry')
  ```

  手动提交事务：

  ```sql
  start  transaction;
  insert into tb_user values(1,'Tom'),(2,'cat'),(3,'Jerry');
  insert into tb_user values(4,'Tom'),(5,'cat'),(6,'Jerry');
  insert into tb_user values(7,'Tom'),(8,'cat'),(9,'Jerry');
  commit;

  ```

  主键顺序插入：

- 大批量插入数据

  如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。

  可以直接将符合一定规则的文件（字段之间通过"," “：”或者其他字符分割），可以直接一次性通过load插入

  ```sql
  # 客户端连接服务器时，加上参数 --local-infile
  mysql--local-infile -u root -p 
  # 设置全局参数local_infile 为1 ，开启从本地加载文件导入数据的开关
  set global local_infile = 1;
  # 执行load指令，将准备好的数据，加载到表结构中;例： 每个字段分隔符为,每一行分隔符为换行
  load data local infile '/root/sql.log' into table 'tb_user' fields terminated by ',' lines terminated by '\n';
  ```

  ​

#### 9.2 主键优化

- 数据的组织方式

  在Innodb引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为**索引组织表**（IOT）。

- 页分裂

  页可以为空，也可以填充一半，或100%。（如果一行数据过大，会行溢出），根据主键排列。

  主键乱序插入的情况，可能发生页分裂。

- 页合并

  当删除一行记录时，实际上记录并没有物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许其他记录声明使用。

  当页中删除的记录达到merge_threshold（默认为页的50%）,Innodb会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。

- 主键的设计原则

  - 满足业务需求的情况下，尽量降低主键的长度
  - 插入数据时，尽量选择顺序插入，选择使用auto_inscrement自增主键
  - 尽量不要使用UUID做主键或者其他自然主键，如身份证号
  - 业务操作时，避免对主键的修改。



#### 9.3 order by 优化

1. using filesort ：通过表的索引或者全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回的排序结果的排序都叫filesort排序

2. using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为using index，不需要额外排序，操作效率高。其中需要覆盖索引，没有覆盖索引的话，需要回表查询 还是会使用using filesort

   ```sql
   # 根据age，phone 进行一个降序，一个升序排列
   # 可以给age创建一个降序索引，phone创建一个升序索引
   create index idx_user_age_phone_da on (age desc, phone asc)
   ```


**总结：**

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
- 尽量使用覆盖索引
- 多字段排序，此时要注意联合索引在创建时的规则（ASC/DESC）
- 如不可避免出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size（默认256k）



#### 9.4 group by 优化

 ```sql
# 执行分组操作，根据profession 字段分组（此时profession 有联合索引）
select profession, count(*) from tb_user group by profession;
 ```

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是遵循最左前缀法则



#### 9.5 limit 优化

一个头疼的问题就是limit 20000000,10 ，此时需要MySQL排序前2000010记录，仅仅返回20000000 - 20000010的记录，其他记录丢弃，查询代价非常大。

优化思路：一般分页查询时， 通过创建覆盖索引 比较好的提高性能，可以通过覆盖索引+子查询的方式进行优化。

```sql
select * from tb_user t, (select id form tb_suer order by id limit 20000000,10) a where t.id = a.id;
```



#### 9.6 count 优化

- MyISAM引擎 把一个表的总行数存在了磁盘上，因此count(*)的时候直接返回这个数，效率很高
- innodb引擎就麻烦了，它执行count(*)时，需要把数据一行一行读出来，再累计。

优化思路：自己计数 （通过redis ...）

**count 的几种用法：**

>  count()是一个聚合函数，对于返回的结果，一行行判断，如果count函数的参数不是null，累计值+1。否则不加，最后返回累计值。统计的是不为null值的记录数

- count(*)：专门做了优化，不会把全部字段取出来，直接按行累加。
- count(主键)：取id值，直接按行累加
- count(字段)：需要判断字段值是否为null，null则不计数。如果有not null约束，则不用判断
- count(1)：innodb引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字 1（随意数字） 进去，直接按行累加

效率：count(字段) < count(主键 )< count(1) < count(*)

#### 9.7 update 优化

Innodb的行锁是针对索引加的锁，不是针对记录，并且该索引不能失效，否则，会从行锁升级为表锁。

并发下，有索引，MySQL会行锁，没有索引 MySQL会表锁。



