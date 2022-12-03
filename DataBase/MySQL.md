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



**索引结构**

- B+ Tree索引：大部分引擎支持B+树索引
- Hash索引：底层数据结构是用hash表实现的，只有精确匹配索引的查询才有效，不支持范围查询
- R-Tree(空间索引)：是MyISAM引擎的一种特殊索引类型，主要用于地理空间数据类型，通常使用较少
- Full-text(全文索引)：是通过建立倒排索引，快速匹配文档的方式。