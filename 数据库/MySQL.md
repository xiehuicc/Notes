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

  ​

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

查询出表1的所有数据和 表1和表2交集的数据

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

