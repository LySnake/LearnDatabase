# SQL
### 1.1 SQL概述

#### 1.1.1 SQL基本信息

> **SQL(Structured Query Language)**:结构化查询语言，用于存取、查询、更新数据以及管理关系型数据库系统。

> SQL由ANSI组织确定规范。

> 在不同的数据库产品中遵守SQL的通用规范，但是也对SQL有一些不同的改进，形成了一些数据库的专有指令
>
> * MySQL--limit
> * SQLServer--top
> * Oracle--rownum

#### 1.1.2 SQL分类

> 根据SQL指令完成的数据库操作的不同，可以将SQL指令分为四类:
>
> * **DDL(Data Definition Language)**:数据定义语言
>   * 用于完成对数据库对象(数据库、数据表、视图、索引等)的创建、删除、修改。
>   * 相关SQL关键字：create、drop、alter、show、desc、modify、add、change等。
> * **DML(Data Manipulation Language)**:数据操作/操纵语言
>   * 用于完成对数据表中的数据的添加、删除、修改操作
>   * 添加：将数据存储到数据表中，如insert
>   * 删除:  将数据从数据表中移除，如delete
>   * 修改:  对数据表中的数据进行修改，如update
> * **DQL(Data Query Language)**:数据查询语言
>   * 用于将数据表中的数据查询出来，如select
> * **DCL(Data Control Language)**:数据控制语言
>   * 用于完成事务管理等控制性操作，如while、if、repeat、rollback、commit等

### 1.2 SQL基本语法

> 在MySQL命令行或者navicat等工具中都可以编写SQL指令。

* SQL指令不区分大小写
* 每条SQL表达式对 ``;``作为结束符
* SQL关键字之间以 ``空格``进行分隔
* SQL之间可以不限制换行(可以有空格的地方就可以有换行)

### 1.3 DDL

#### 1.3.1 DDL-数据库操作

> 使用DDL语句可以创建数据库、查询数据库(库本身)、修改数据库、删除数据库

**查询数据库**

```sql
## <db-name>：表示数据库的名字

## 显示当前MySQL中的数据库列表
show databases;

## 显示指定名称的数据库的创建的SQL命令
show create database <db-name>;
```

**创建数据库**

```sql
## <db-name>：表示数据库的名字

## 创建数据库
create database <db-name>;

## 创建数据库，当前指定名称的数据库不存在时执行创建
create database if not exists <db-name>;

## 在创建数据库的同时指定数据库的字符集
create database <db-name> character set utf8;
```

**修改数据库**

```sql
## <db-name>：表示数据库的名字

## 修改数据库的字符集
alter database <db-name> character set utf8;
```

**删除数据库**

```sql
## <db-name>：表示数据库的名字

## 删除数据库
drop database <db-name>;

## 如果数据库存在则删除数据库
drop database if exists <db-name>;
```

**使用/切换数据库**

```sql
## <db-name>：表示数据库的名字

## 切换当前使用的数据库
use <db-name>;
```

#### 1.3.2 DDL-数据表操作

**创建数据表**

> 数据表实际就是一个二维的表格，一个表格是由多列组成，表格中的每一列称为表格中的字段(**Field**);

|  stu_NO  | stu_name | stu_age |
| :------: | :------: | :-----: |
| 20220415 |   张三   |   18    |
| 20220416 |   李四   |   19    |

```sql
## 创建上图所示的数据库表
## char、varchar、int:指定的是字段的类型，对于char、varchar后的(8)，则表示有几个字节用于存储这个字段，char是固定长度数据内容，不足则补齐，而varchar则表示最多存储多长字节的数据内容(中文按2个byte计算)
## no null:表示该字段不可为空值
## unique:表示该字段不可重复
create table students(
	stu_NO char(8) no null unique,
	stu_name varchar(32) no null,
	stu_age int no null
);
```

**查询数据表**

```sql
show tables;
```

**查询数据表结构**

```sql
## <table-name>:表示表的名字

desc <table-name>;
```

**删除数据表**

```sql
## <table-name>:表示表的名字

## 删除数据表
drop table <table-name>;
## 当前数据表存在时删除数据表
drop table if exists <table-name>;
```

**修改数据表**

```sql
## <table-name>、<new-talbe-name>表示表的名字
## <field-name>、<old-field-name>、<new-field-name>表示字段的名字
## <type>、<newType>表示int、char等类型


## 修改表名
alter table <table-name> rename to <new-talbe-name>;

## 数据表也是有字符集的，默认与所属数据库一致
alter table <table-name> character set utf8;

## 添加字段(列)
alter table <table-name> add <field-name> <type>;

## 修改字段的名字和类型
alter table <table-name> change <old-field-name> <new-field-name> <type>;

## 只修改字段类型
alter table <table-name> modify <field-name> <newType>;

## 删除表中的字段
alter table <table-name> drop <field-name>;
```

### 6.4 MySQL数据类型

> 数据类型，指的是数据表中的字段支持存放的数据的类型。

#### 6.4.1数值类型

在MySQL中有多种数值类型，不同的类型存放的数值范围或者形式是不同的。

|    类型    |       内存占用大小       |    数值范围     |                             说明                             |
| :--------: | :----------------------: | :-------------: | :----------------------------------------------------------: |
|  tinyint   |            1B            |  [-2^7, 2^7-1]  |                         特定小型整数                         |
|  smallint  |            2B            | [-2^15, 2^15-1] |                           小型整数                           |
| mediumint  |            3B            | [-2^23, 2^23-1] |                           中型整数                           |
|  **int**   |            4B            | [-2^31, 2^31-1] |                             整数                             |
|   bigint   |            8B            | [-2^63, 2^63-1] |                            大整数                            |
|   float    |            4B            |                 |                            单精度                            |
| **double** |            8B            |                 |                            双精度                            |
|  decimal   | 根据声明不同占用大小不同 |                 | decimal(10,2):表示值为10进制数表示时，数字的有效个数最大为10，其中小数部分占2位。 |

#### 6.4.2 字符串类型

> 在MySQL中存储字符序列的类型。

|     类型     | 字符长度范围 |                             说明                             |
| :----------: | :----------: | :----------------------------------------------------------: |
|   **char**   |  [0, 2^8-1]  | 定长字符串，最多可以存储255个字符;当我们指定数据表字段为char(n)时，此列中的数据最长为n个字符，如果添加的数据少于n，则补'\u0000'. |
| **varchar**  |  [0, 2^16]   |           可变长度字符串，此类型的最大长度为2^16。           |
|   tinyblob   |  [0, 2^8-1]  |                        存储二进制数据                        |
|     blob     | [0, 2^16-1]  |                        存储二进制数据                        |
|  mediumblob  | [0, 2^24-1]  |                        存储二进制数据                        |
|   longblob   |  [0,2^32-1]  |                        存储二进制数据                        |
|   tinytext   |  [0, 2^8-1]  |                       文本数据(字符串)                       |
|     text     | [0, 2^16-1]  |                       文本数据(字符串)                       |
|  mediumtext  | [0, 2^24-1]  |                       文本数据(字符串)                       |
| **longtext** |  [0,2^32-1]  |                       文本数据(字符串)                       |

#### 6.4.3 日期类型

> 在MySQL中存储中，我们可以使用字符串来存储时间，但是如果我们需要基于时间字段进行查询操作(查询在某个时间段内的数据)就不便于查询实现。
>
> 在时间类型中，应该还有时间可用范围的问题，比如时间戳从1970年开始计，且2035年左右的范围。

|     类型      |        格式         |               说明               |
| :-----------: | :-----------------: | :------------------------------: |
|   **date**    |     2021-09-01      |     日期，只存储 ``年月日``      |
|     time      |      11:12:13       |     时间，只存储 ``时分秒``      |
|     year      |        2021         |             ``年份``             |
| **datetime**  | 2021-09-01 11:12:13 | 日期+时间，存储 ``年月日时分秒`` |
| **timestamp** |   20210913 111213   |        日期+时间(时间戳)         |

#### 6.5 字段约束

##### 6.5.1 约束介绍

> 在创建数据表的时候，对数据表中的列的数据作限制性的要求(对表的列中的数据进行限制)

为什么要给表中的列添加约束呢？

* 保证数据的有效性
* 保证数据的完整性
* 保证数据的正确性

字段常见的约束有哪些呢？

* **非空约束(not null)**:限制此列的值必须提供，不能为null
* **唯一约束(unique)**:在表中的多条数据，此列的值不能重复
* **主键约束(primary key)**: 非空+唯一，能够唯一标识数据表中的一条数据

##### 6.5.2 非空约束

> 限制数据表中的此列的值必须提供

* 创建表:设置表(``books``)的字段 ``book_name``不可为null
  ```sql
  ## 由此创建的数据表，字段book_name在创建时如果未指定该字段的值，则报错失败
  create table books(
  	book_isbn char(4),
  	book_name varchar(20) not null,
  	book_author varchar(20) 
  );
  ```

##### 6.5.3 唯一约束

> 在表中的多条数据，此列的值不能重复

* 创建表:设置图书表的book_isbn为unique
  ```sql
  ## 由此创建的表(books)中的字段book_isbn字段的值，不可重复，否则创建/修改时失败并报错
  create table books(
  	book_isbn char(4) unique,
  	book_name varchar(20) not null,
  	book_author varchar(20) 
  );
  ```

##### 6.5.4 主键约束

> 主键：就是数据表中记录的唯一标识，在一张表中只能有一个主键(主键可以是一个字段，也可以是多个字段的组合)

**创建表时定义主键**

```sql
## 创建表books时，指定字段book_isbn字段为主键
create table books(
	book_isbn char(4) primary key,
	book_name varchar(20) not null,
	book_author varchar(20)
);
```

或者

```sql
## 创建表books时，指定字段book_isbn字段为主键
create table books(
	book_isbn char(4),
	book_name varchar(20) not null,
	book_author varchar(20),
	primary key(book_isbn)
);
```

**删除数据表主键约束**

```sql
## books是表的名字
alter table books drop primary key;
```

**创建表之后添加主键约束**

```sql
## 创建表时没有添加主键约束
create table books(
	book_isbn char(4),
	book_name varchar(20) not null,
	book_author varchar(20)
);

## 创建表之后添加主键约束,books是表名，book_isbn是字段名。
alter talbe books modify book_isbn char(4) primary key;
```

##### 6.5.5 主键自动增长

> 在我们创建一张数据表时，如果数据表中有字段可以作为主键，我们可以直接使用这个字段作为主键;
>
> 当有些数据表中没有合适的列作为主键时，我们可以额外定义一个与记录本身无关的列(ID)作为主键，此列数据无具体的含义，主要用于标识一条记录，在MySQL中我们要可以将此列定义为int类型，同时设置为**自动增长**，当我们向数据表中新增一条记录时，无需提供ID列的值，它会自动生成。

**定义主键自动增长**

* 定义int类型的字段自动增长:``auto_increment``

```sql
## 创建表types
## type_id 类型为int,且为主键，且为自动增长类型
create table types(
	type_id int primary key auto_increment,
	type_name varchar(20) not null,
	type_remark varchar(100)
);
```

**Note**:自动增长从1开始，每添加一条记录，自动增长的列会自动+1，当我们把某条记录删除之后再添加数据，自动增长的数据也不会重复生成(自动增长只保证唯一性不保证连续性)。

##### 6.5.6 联合主键

> 联合主键：将数据表中的多列组合在一起设置为表的主键

**定义联合主键**

```sql
create table grades(
	stu_num char(8),
	course_id int,
	score int,
	primary key(stu_num, course_id)
);
```

**Note**:在实际企业项目的数据库设计中，联合主键使用频率并不高;当一张数据表中没有明确的字段可以作为主键时，我们可以额外添加一个ID字段作为主键。

##### 6.5.7 外键约束

查看[外键约束-建立表关系](#7.5 外键约束-建立表关系)

#### 6.6 DML 数据操纵语言

> 用于完成对数据表中数据的插入、删除、修改操作

```sql
## 创建学生表
create table stus(
	stu_num char(8) primary key,
	stu_name varchar(28) not null,
	stu_gender char(2) not null,
	stu_age int not null,
	stu_tell char(11) not null unique,
	stu_qq varchar(11) unique
);
```

##### 6.6.1 插入数据

**语法**

```sql
insert into <table-name>(<field-name1>[,<field-name2>,...]) values(<field-value1>[,<field-value2>,...]);
```

**示例**

```sql
## 向数据表中指定的列添加数据(不允许为空的列必须提供数据)
insert into stus(stu_num, stu_name, stu_gender, stu_age, stu_tel) values('20201111', '李四', '男', 18, '13172725412');

## 数据表名后的字段名列表顺序可以不与表中一致，但是values中值的顺序必须与表名后字段名顺序对应
insert into stus(stu_num, stu_name, stu_age, stu_tell, stu_gender) values('20210103', '五儿',21,'13132325413', '女');

## 当要向表中的所有列添加数据时，数据表名后面的字段列表可以省略，但是values中的值的顺序要与数据表定义字段保持一致
insert into stus values('20201111', '李四', '男', 18, '13172725412');

## 不过在项目开发中，即使要向所有列添加数据，也建议将列名的列表显式写出来(增强SQL的稳定性)
insert into stus(stu_num, stu_name, stu_gender, stu_age, stu_tel,stu_qq) values('20201111', '李四', '男', 18, '13172725412', '131154156');
```

##### 6.6.2 删除数据

> 从数据表中删除满足特定条件(所有)的记录

**语法**

```sql
delete from <table-name> [where conditions];
```

**示例**

```sql
## 删除学号为20210102的学生信息
delete from stus where stu_num='20210102';

## 删除年龄大于20岁的学生信息(如果满足where子句的记录有多条，则删除多条记录)
delete from stus where stu_age>20;

## 如果删除语句没有where子句，则表示删除当前数据表中的所有记录(敏感操作)
delete from stus;
```

##### 6.6.3 修改数据

> 对数据表中已经添加的记录进行修改

**语法**

```sql
update <table-name> set <field-name>=<field-value> [where conditions];
```

**示例**

```sql
## 将学号为202021112的学生姓名修改为"孙七"(只修改一例)
update stus set stu_name='孙七' where stu_num='202021112';

## 将学号为202021112的学生，性别修改为"男"，同时将QQ修改为12345678(修改多列)
update stus set stu_gender='男',stu_qq='12345678' where stu_num='202021112';

## 根据主键修改其它所有列
update stus set stu_name='张三',stu_gender='女',stu_age=18,stu_tell='13112135415',stu_qq='12345443' where stu_name='20210104'

## 如果update语句没有where子句，则表示修改当前表中所有行(记录)
update stus set stu_name='Tom';
```

#### 6.7 DQL 数据查询语言

> 从数据表中提取满足特定条件的记录
>
> * 单表查询
> * 多表联合查询

##### 6.7.1 查询基础

**语法**

```sql
## select 关键字后指定要显示查询到的记录的哪些列
select <field-name1>[,<field-name2>,...] from <talbe-name>;

## 如果要显示查询到的记录的所有列，则可以使用*替代字段名列表(在项目开发中不建议使用)
select * from <table-name>;
```

##### 6.7.2 where子句

> 在删除、修改及查询的语句后都可以添加where子句(条件)，用于筛选满足特定的数据进行删除、修改和查询操作。

```sql
delete from <table-name> [where conditions];
update <table-name> set <field-name1>[,<field-name2>,...] values(<field-value1>[,<field-value2>,...]) [where conditions];
select * from <table-name> [where conditions]; 
```

**条件**

```sql
## = 等于
select * from stus where stu_num = '12345';

## != 或 <>   不等于
select * from stus where stu_num != '12345';
select * from stus where stu_num <> '12345';

## > 大于
select * from stus where stu_age > 18;

## < 小于
select * from stus where stu_age < 21;


## <= 小于等于
select * from stus where stu_age <= 21;


## >= 大于等于
select * from stus where stu_age >= 18;

```

**多条件查询**

> 在where子句中，可以将多个条件通过逻辑运算符(``and、or、not``)进行连接，通过多个条件来筛选要操作的数据。

```sql
## and 并且:筛选多个条件同时满足的记录
select * from stus where stu_age>18 and stu_age<20;

## or 或者:筛选多个条件中至少满足一个条件的记录
select * from stus where stu_age<19 or stu_age>20;

## not 取反
## between value1 and value2,表示值在[value1, value2]中的取值范围 
select * from stus where not between 18 and 20;
```

##### 6.7.3 like查询:模糊查询

> 在where子句的条件中，可以使用like关键字来实现模糊查询

**语法**

```sql
select * from suts where stu_name link 'reg';
```

* 在link关键字后的reg表达式中
  * **%** 表示零到多个任意字符[**%o%** 包含字母o]
  * **_** 表示任意一个字符[ **_o%**  第二个字母为o]

**示例**

```sql
# 查询学生姓名包含字母o的学生信息
select * from stus where stu_name like '%o%';

# 查询学生姓名第一个字为'张'的学生信息
select * from stus where stu_name like '张%';

# 查询学生姓名最后一个字母为o的学生信息
select * from stus where stu_name like '%o';

# 查询学生姓名中第二个字母为o的学生信息
select * from stus where stu_name like '_o%';
```

##### 6.7.4 对查询结果的处理

**设置查询的列**

> 声明显示查询结果的指定列

```sql
select <field-name1>[,<field-name2>,...] from stus where stu_age > 20;
```

**计算列**

> 对从数据表中查询到的记录的列进行一定的运算之后显示出来

```sql
## 出生年份 = 当前年份 - 年龄
select stu_name, 2022 - stu_age from stus;

+--------------------+-----------------+
|      stu_name      |  2022 - stu_age |
+--------------------+-----------------+
|         李四        |      2000       |
+--------------------+-----------------+
|         张三        |      1993       |
+--------------------+-----------------+
```

**字段别名**

> 我们可以为查询结果的列名取一个语义性更强的别名

```sql
select stu_name, 2022 - stu_age as stu_birth_year from stus;
+--------------------+-----------------+
|      stu_name      |  stu_birth_year |
+--------------------+-----------------+
|         李四        |      2000       |
+--------------------+-----------------+
|         张三        |      1993       |
+--------------------+-----------------+

select stu_name as 姓名, 2022 - stu_age as stu_birth_year from stus;
+--------------------+-----------------+
|        姓名         |  stu_birth_year |
+--------------------+-----------------+
|        李四         |       2000      |
+--------------------+-----------------+
|        张三         |       1993      |
+--------------------+-----------------+
```

**消除重复行**

> 从查询的结果中将重复的记录消除**distinct**

```sql
select stu_age from stus;
+--------------------+
|       stu_age      |  
+--------------------+
|         17         | 
+--------------------+
|         17         | 
+--------------------+
|         18         | 
+--------------------+

select distinct stu_age from stus;
+--------------------+
|      stu_age       |  
+--------------------+
|        17          |
+--------------------+
|        18          | 
+--------------------+
```

##### 6.7.5 order by:排序

> 将查询到的满足条件的记录按照指定的列的值升序/降序排列

**语法**

```sql
select * from <table-name> where conditions order by <field-name> asc|desc;
```

* ``order by <field-name>``表示将查询结果按照指定的列排序
  * asc表示按指定的列的升序排列(**默认**)
  * desc表示按指定的列的降序排列

**示例**

```sql
# 单字段排序
select stu_gender from stus where stu_age>18 order by stu_gender desc;
+--------------------+
|      stu_gender    |  
+--------------------+
|         男         |
+--------------------+
|         男         |
+--------------------+
|         男         |
+--------------------+
|         女         | 
+--------------------+

# 多字段排序: 先满足字段stu_gender排序，再满足字段stu_age排序
select stu_gender,stu_age from stus where stu_age>18 order by stu_gender asc,stu_age order by stu_age desc;
+--------------------+--------------------+
|      stu_gender    |       stu_age      |
+--------------------+--------------------+
|         女         |         21         |
+--------------------+--------------------+
|         女         |         20         |
+--------------------+--------------------+
|         女         |         19         |
+--------------------+--------------------+
|         男         |         23         |
+--------------------+--------------------+
|         男         |         19         |
+--------------------+--------------------+
```

##### 6.7.6 聚合函数

> 聚合函数：SQL中提供了一些可以对查询的记录的列进行计算的函数

* `count()`统计函数，统计满足条件的指定字段值的个数(记录数)

  ```sql
  # 统计学生中学生总数
  select count(stu_num) from stus;
  +--------------------+
  |  count(stu_num)    | 
  +--------------------+
  |         7          | 
  +--------------------+
  
  # 统计学生中性别为'男'的学生总数
  select count(stu_num) from stus where stu_gender='男';
  +--------------------+
  |  count(stu_num)    | 
  +--------------------+
  |         3          | 
  +--------------------+
  ```
* `max()`计算最大值，查询满足条件的记录中指定列的最大值

  ```sql
  # 统计学生年龄的最大值
  select max(stu_age) from stus;
  +--------------------+
  |   max(stu_age)     | 
  +--------------------+
  |         21         | 
  +--------------------+
  
  # 统计学生中女生年龄的最大值
  select max(stu_age) from stus where stu_gender='女';
  +--------------------+
  |   max(stu_age)     | 
  +--------------------+
  |         18         | 
  +--------------------+
  ```
* `min()`计算最小值，查询满足条件的记录中的指定列的最小值

  ```sql
  # 统计学生年龄的最小值
  select min(stu_age) from stus;
  +--------------------+
  |    min(stu_age)    | 
  +--------------------+
  |         15         | 
  +--------------------+
  
  # 统计女学生中年龄的最小值
  select min(stu_age) from stus where stu_gender='女';
  +--------------------+
  |    min(stu_age)    | 
  +--------------------+
  |         17         | 
  +--------------------+
  ```
* `sum()`计算和，查询满足条件的记录中，指定的列的值的总和

  ```sql
  # 统计所有学生的年龄总合
  select sum(stu_age) from stus;
  +--------------------+
  |    sum(stu_age)    | 
  +--------------------+
  |         133        | 
  +--------------------+
  
  # 统计所有女学生的年龄总合
  select sum(stu_age) from stus where stu_gender='女';
  +--------------------+
  |    sum(stu_age)    | 
  +--------------------+
  |         63         | 
  +--------------------+
  ```
* `avg()`求平均值，查询满足条件的记录中计算指定列的平均值

```sql
# 计算所有学生的平均年龄
select avg(stu_age) from stus;
+--------------------+
|    avg(stu_age)    | 
+--------------------+
|      19.80000      | 
+--------------------+

# 计算所有女学生的平均年龄
select avg(stu_age) from stus where stu_gender='女';
+--------------------+
|    avg(stu_age)    | 
+--------------------+
|      18.00000      | 
+--------------------+
```

##### 6.7.7 日期函数和字符串函数

**日期函数**

> 当我们向日期类型的列添加数据时，可以通过字符串类型赋值(字符串的格式必须为 yyyy-MM-dd hh:mm:ss )
>
> 如果我们想要获取当前系统时间添加到日期类型的列中，可以使用 `now()`或者 `sysdate()`

**示例**

```sql
# 通过字符串类型给日期类型的列赋值
insert into stus(stu_num, stu_name, stu_gender, stu_age, stu_tell, stu_qq, stu_enterence) values('20200108','张小花','女',18,'13172725431', '35335422','2021-09-01 09:00:00');

# 通过now()给日期类型的列赋值
insert into stus(stu_num, stu_name, stu_gender, stu_age, stu_tell, stu_qq, stu_enterence) values('20200108','张小花','女',18,'13172725431', '35335422', now());

# 通过sysdate()给日期类型的列赋值
insert into stus(stu_num, stu_name, stu_gender, stu_age, stu_tell, stu_qq, stu_enterence) values('20200108','张小花','女',18,'13172725431', '35335422', sysdate());


# 通过sysdate()或now()显示当前时间
select now();
select sysdate();

```

**字符串函数**

> 就是通过SQL指令对字符串进行处理

**示例**

```sql
# concat(<field-name1>[, <field-name2>,...]) 拼接多列
select concat(stu_name, '-', stu_gender) from stus;
+--------------------------------------+
|  concat(stu_name, '-', stu_gender)   | 
+--------------------------------------+
|              Tom-男                  | 
+--------------------------------------+
|              Lucy-女                 | 
+--------------------------------------+
|              林涛-男                  | 
+--------------------------------------+

# upper(<field-name>)将字段的值转换成大写
select upper(stu_name) from stus;
+------------------------+
|     upper(stu_name)    | 
+------------------------+
|          TOM           | 
+------------------------+
|          LUCY          | 
+------------------------+

# lower(<field-name>)将字段的值转换成小写
select lower(stu_name) from stus;
+------------------------+
|     lower(stu_name)    | 
+------------------------+
|          tom           | 
+------------------------+
|          lucy          | 
+------------------------+

# substring(<field-name>, start-index, subStrLength);从指定的列中截取部分显示,start-index从1开始
select stu_name, substring(stu_tell, 8, 4) from stus;

+--------------------+---------------------------------+
|      stu_name      |       substring(stu_tell, 8, 4) |
+--------------------+---------------------------------+
|         Tom        |               3311              |
+--------------------+---------------------------------+

```

##### 6.7.8 分组查询

> 分组:就是将数据记录按照指定的字段进行分组

**语法**

> select <field-name>/聚合函数 from <table-name> 
>
> [where conditions] 
>
> [group by <field-name> [having conditions]] [order by <field-name>];

* `select ` 后使用 `*`显示对查询结果进行分组之后，显示每组的第一条记录(这种显示通常是无意义的)
* `select`后通常显示分组字段和聚合函数(对分组后的数据进行统计 、求和、平均值等)
* 语句执行属性:
  1. 先根据where条件从数据库获取查询记录。
  2. group by对查询记录进行分组
  3. 执行having对分组后的数据进行筛选(一定要存在group by分组名)

**示例**

```sql
# 先对查询的学生信息按性别进行分组(分成了男、女两组)，然后再分别统计每组学生的个数
select stu_gender, count(stu_num) from stus group by stu_gender;
+--------------------+---------------------------------+
|      stu_gender    |       count(stu_num)            |
+--------------------+---------------------------------+
|         女         |               3                 |
+--------------------+---------------------------------+
|         男         |               2                 |
+--------------------+---------------------------------+

# 先对查询的学生信息按性别进行分组(分成了男、女两组)，然后再计算每组中平均年龄
select stu_gender, avg(stu_age) from stus group by stu_gender;
+--------------------+---------------------------------+
|      stu_gender    |          avg(stu_age)           |
+--------------------+---------------------------------+
|         女         |               19.7500           |
+--------------------+---------------------------------+
|         男         |               18.2000           |
+--------------------+---------------------------------+

# 先对学生按年龄进行分组(16、17、18、20、21、22六组)，然后统计各组的学生数量，还可以对最终结果排序
select stu_age, count(stu_num) from stus group by stu_age order by stu_age;
+--------------------+---------------------------------+
|      stu_age       |          count(stu_num)         |
+--------------------+---------------------------------+
|         16         |               2                 |
+--------------------+---------------------------------+
|         17         |               1                 |
+--------------------+---------------------------------+
|         18         |               3                 |
+--------------------+---------------------------------+
|         20         |               1                 |
+--------------------+---------------------------------+
|         21         |               4                 |
+--------------------+---------------------------------+
|         22         |               2                 |
+--------------------+---------------------------------+

# 查询所有学生，按年龄进行分组，然后分别统计每组的人数，再筛选当前组人数>1的组，再按年龄升序显示出来
select stu_age,count(stu_num) from stus where stu_gender='男' group by stu_age having count(stu_num)>1 order by stu_age;
```

##### 6.7.9 分页查询

> 当数据表中的记录比较多的时候，如果一次性全部查询出来显示给用户，用户的可读性/体验性就不太好，因此我们可以将这些数据分页进行展示。

**语法**

```sql
# start-index: int类型，表示获取查询语句的结果中的第一条数据的索引(索引从0开始)
# page-size: int类型，表示获取的查询记录的条数。如果最后条数不够，可以实际查询到的条数小于page-size

select <field-name1>[,<field-name2>,...]
	from <table-name>
	[where conditions]
	limit start-index, page-size;
```

**示例**

```sql
# 查询第一页
select * from stus limit 0,3;

# 查询第二页
select * from stus limit 3,3;

# 查询第三页
select * from stus limit 6,3;

# 查询第四页:因共10条记录，所以最后一页的获取到的总条数小可以小于或等于3.
select * from stus limit 9,3;
```

### 7. 数据表的关联关系

#### 7.1关联关系介绍

> MySQL是一个关系型数据库，不仅可以存储数据，还可以维护数据与数据之间的关系--通过在数据表中添加字段建立 `外键约束`

数据与数据之间的关联关系，分为四种：

* 一对一关联
* 一对多关联
* 多对一关联
* 多对多关联

#### 7.2 一对一关联

> 人-----身份证号码：一个人只有一个身份证号码，一个身份证号码对应一个人
>
> 学生---学籍:一个学生只有一份学籍信息，一份学籍信息对应唯一的一个学生
>
> 用户---用户详情:一个用户只有一份用户详情，一份用户详情对应一个用户

**方案1**:主键关联--两张数据表中主键相同的数据为相互对应的数据

> 即表A的主键值与表B的主键值一一对应。包含类型、值。

**方案2**:唯一外键约束--在任意一表中添加一个字段，添加外键约束与另一张表主键关联，并且将外键列设置为唯一约束(**unique**)

> 即表A增加一个字段field,其中field字段设置为外键约束且为唯一约束，其值与表B的主键一一对应。

#### 7.3 一对多与多对一

> 班级--学生   (一对多)
>
> 学和--班级   (多对一)

**方案**:在表A(多的一端)添加外键，与表B(少的一端)的主键进行关联。

> 比如:学生表，是 `多`的一端，班级一有是 `一`的一端，所以学生表中增加一个外键与班级表的主键进行关联。

#### 7.4 多对多关联

> 学生--课程  一个学生可以选择多门课、一门课程也可以由多个学生选择
>
> 会员--社团  一个会员可以参加多个社团，一个社团也可以招纳多个会员

**方案**:额外创建一张关系表来维护多对多关联。

> 比如：
>
> 学生表A:每个学生可以选择多个课程。
>
> 课程表B:每个课程可以被多个学生选择。
>
> 新建关系表(选课表):可以提供一列作为 `自增主键`ID或使用 `联合主键`,另外二列为 `外键`，一个外键关联表A的主键，一个外键关联表B的主键。

#### 7.5 外键约束-建立表关系

> 外键约束：将一个列添加外键约束与另一张表的主键(或唯一约束的列)进行关联之后，这个外键约束的列添加的数据必须要在关联的主键字段中存在。

**案例** ：学生表与班级表

1. 先创建班级表

    ```sql
    create table classes(
    class_id int primary key auto_increment,
    class_name varchar(20) not null unique,
    class_remark varchar(200)
    );
    ```
2. 创建学生表(在学生表中添加外键与班级表的主键进行关联)

    ```sql
    # [方式1]
    # 在创建表的时候，定义cid字段，并添加外键约束
    # 由于cid列要与classes表的class_id进行关联，因此cid字段类型和长度要与class_id一致
    create table students(
        stu_num char(8) primary key,
        stu_name varchar(20) not null,
        stu_gender varchar(2) not null,
        stu_age int not null,
        cid int,
        constraint FK_STUDENTS_CLASSES foreign key(cid) references classes (class_id)
    );

    # [方式2]
    # 先正常创建表，再添加外键约束
    create table students(
        stu_num char(8) primary key,
        stu_name varchar(20) not null,
        stu_gender varchar(2) not null,
        stu_age int not null,
        cid int
    );

    alter talbe students add constraint FK_STUDENTS_CLASSES foreign key(cid) references classes(class_id);

    # 删除外键约束
    alter table students drop foreign key FK_STUDENTS_CLASSES;
    ```

3. 向班级表添加班级信息

   ```sql
   insert into classes(class_name, class_remark) values('数学', '...');
   insert into classes(class_name, class_remark) values('语文', '...');
   insert into classes(class_name, class_remark) values('化学', '...');
   insert into classes(class_name, class_remark) values('物理', '...');
   
   select * from classes;
   +--------------------+----------------+-----------------+
   |      class_id      |    class_name  |  class_remark   |
   +--------------------+----------------+-----------------+
   |         1          |      数学       |      ...        |
   +--------------------+----------------+-----------------+
   |         2          |      语文       |      ...        |
   +--------------------+----------------+-----------------+
   |         3          |      化学       |      ...        |
   +--------------------+----------------+-----------------+
   |         4          |      物理       |      ...        |
   +--------------------+----------------+-----------------+
   ```
4. 向学生表中添加学生信息

   ```sql
   # 添加成功，因为cid为4的班级存在
   insert into students(stu_num, stu_name, stu_gender, stu_age, cid) 
   values('20220102', 'Tom', '女', 18, 4);
   
   # 添加失败，设置能cid的外键列的值必须在其关联的主表classes的class_id的列存在
   insert into students(stu_num, stu_name, stu_gender, stu_age, cid) 
   values('20220103', 'lucy', '女', 18, 5);
   ```

#### 7.6 外键约束-级联

> **级联操作**:在添加外键时，设置为级联修改和级联删除,比如表B的主键被表A的外键所关联。
>
> * **级联修改**:当表B的主键的某条记录由a改为b，那么表A中的外键上所有原值为a的记录全改为b(仅改对应列上的字段值)。
> * **级联删除**:当表B删除了一条记录行，则该记录行的主键值对应的表A的外键上所有相同值的记录，均会在表A中删除所有匹配行。

**语法**：

```sql
# 删除原来的外键
alter table students drop foreign key FK_STUDENTS_CLASSES;

# 重新添加外键，并设置级联修改和级联删除
alter table students add constraint FK_STUDENTS_CLASSES foreign key(cid) references classes(class_id) ON UPDATE CASCADE ON DELETE CASCADE;
```

**不使用级联操作时**

> 当学生表中存在学生信息关联班级表中的某条记录时，就不能对班级表的这条记录进行值修改和删除操作

> 如果要修改或删除存在外键关联的列的值，常规操作为:
>
> * 将引用该值的外键所在的记录，修改为NULL，即解除外键值与关联表主键值之间的关联关系。
> * 再修改/删除需要修改的被外键关联的主键的记录。
> * 然后再修改外键为NULL的值为新的值或删除。

**使用级联操作时**

> * 级联关系由表本身的属性，可以设置。
> * 当有依赖的表A的外键和表B的主键关联了
>   * 表B的主键的某条记录由a改为b，那么表A的的外键列，所有原值为a的记录全都会被修改为b.
>   * 表B的主键的某条记录行为a的值的行被删除时，那么表A的的外键列值为a的记录行，也会同步删除该记录行。

### 8. 连接查询

> 通过对DQL的学习，我们可以很轻松的从一张数据表中查询出需要的数据；在企业的应用开发中，我们经常需要从多张表中查询数据(如:查询学生信息的时候需要同时查询学生的班级信息),可以通过连接查询从多张数据表提取数据;
>
> 在MySQL中可以使用join实现多表的联合查询--连接查询,join按照其功能不同，分为三个操作：
>
> * inner join 内连接
> * left join 左连接
> * right join 右连接

#### 8.1 数据准备

##### 8.1.1 创建数据表

创建班级信息表和学生信息表

```sql
create table classes(
	class_id int primary key auto_increment,
	class_name varchar(20) not null unique,
	class_remark varchar(200)
);

create table students(
	stu_num char(8) primary key,
	stu_name varchar(20) not null,
	stu_gender char(2) not null,
	stu_age int not null,
	cid int,
	constraint FK_STUDENTS_CLASSES foreign key(cid) references classes(class_id) ON UPDATE CASCADE ON DELETE CASCADE
);
```

##### 8.1.2 添加数据

添加班级数据

```sql
inert into classes(class_name, class_remark) values('Java2104', '...');
inert into classes(class_name, class_remark) values('Java2105', '...');
inert into classes(class_name, class_remark) values('Java2106', '...');
inert into classes(class_name, class_remark) values('Python2107', '...');
```

添加学生信息

```sql
insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210101','张三'，'男', 20, 1);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210102','李四'，'女', 20, 1);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210103','王五'，'男', 20, 1);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210104','赵六'，'女', 20, 2);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210105','孙七'，'男', 20, 2);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210106','小红'，'女', 20);

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) values('20210107','小明'，'男', 20);
```

#### 8.2 内连接 INNER JOIN

**语法**

> select * from `<table-name1> inner join <table-name2>;`

##### 8.2.1 笛卡尔积

* 笛卡尔积(A集合&B集合):使用A中的每个记录一次关联B中每个记录，笛卡尔积的总数=A总*B总数。
* 如果直接执行 ``select * from <table-name1> inner join <table-name2>;``，会获取到两种表中的数据集合的笛卡尔积（依次使用 `<table-name1> `表中的每条记录去匹配 `<table-name2>` `的每条数据。 `

##### 8.2.2 内连接条件

> 查询二张表时用inner join连接查询之后 产生的笛卡尔积数据中很多数据都是无意义的，我们如何消除无意义的数据呢？--添加二种进行连接查询时的条件。

* 使用 `ON`设置二张表连接查询的匹配条件

  ```sql
  # 使用where设置过滤条件：先生成笛卡尔积再从笛卡尔积中过滤数据(效率低)
  select * from students inner join classes where students.cid = classes.class_id;
  
  # 使用on设置连接查询条件:先判断连接条件是否成立，如果成立那么二张表的数据进行组合和生成一条结果记录
  select * from students inner join classes on students.cid = classes.class_id;
  ```
* 结果：只获取两种表中匹配条件成立的数据，任何一张表在另一张表如果没有找到匹配则不会出现在查询结果中(例如:小红和小明没有对应的班级信息，Java2106和Python2106没有对应的学生)。

#### 8.3 左连接 LEFT JOIN

> 需求：请查询出所有的学生信息，如果学生有对应的班级信息，则将对应的班级信息也查询出来。

**左连接**:显示左表中的所有数据，如果在右表中存在与左表记录满足匹配条件的数据，则进行匹配;如果右表中不存在匹配数据，则显示为NULL。

**语法**

```sql
# on conditions:是left join的连接条件，避免生成笛卡积，效率低下
# where conditions:是针对最后的结果，做过虑操作。

select * from <left-table-name> 
left join <right-talbe-name> 
[on conditions] 
[where conditions];
```

#### 8.4 右连接 RIGHT JOIN

**右连接** :显示右表中的所有记录

**语法**

```sql
# on conditions:是left join的连接条件，避免生成笛卡积，效率低下
# where conditions:是针对最后的结果，做过虑操作。

select * from <left-table-name> 
right join <right-table-name>
[on conditions]
[where conditions];
```

#### 8.5 数据表别名

> 如果在连接查询的多张表中存在相同名字的字段，我们可以使用 `表名.字段名`进行区分，如果表名太长则不便于SQL语句的书写，我们可以使用数据表别名。

**示例**

```sql
# s是表students的别名
# c是表classes的别名
select s.*, c.class_name from students s inner join classes c on s.cid  = t.class_id;
```

#### 8.6 子查询/嵌套查询

> 子查询-先进行一次查询，第一次查询的结果作为第二次查询的源/条件(第二次查询是基于第一次的查询结果来进行的)

##### 8.6.1 子查询返回单个值

**示例：**`查询班级名称为 Java2104 班级中的学生信息`(只知道班级名称，而不知道班级ID)

* 传统方式:

  ```sql
  # 先查询出 Java2104 班的班级ID
  select class_id from classes where class_name ='Java2104';
  
  # 再根据查询到的class_id，在表students中查询到相关学生
  select * from students where cid = 1;
  ```
* 子查询

  ```sql
  # 括号中的句子即为子句。
  select * from students where cid = (select class_id from classes where class_name = 'Java2104');
  ```

##### **8.6.2 子查询返回多个值**

**示例**: `查询所有Java班级中的学生信息`

* 传统方式:

  ```sql
  # 先查询出所有班级名称为Java开头的班级ID
  select class_id from classes where class_name like 'Java%';
  
  +----------+
  | class_id |
  +----------+
  |     1    |
  +----------+
  |     2    |
  +----------+
  |     3    |
  +----------+
  
  # union:将多个查询语句的结果整合在一起输出(必需是输出样式一)
  select * from students where cid = 1
  union
  select * from students where cid = 2
  union
  select * from students where cid = 3;
  ```
* 子查询

  ```sql
  # 括号中的是子句，且查询结果为单字段多列结果，即为一个class_id的结果集合
  # IN/NOT IN:表示该cid 属于/不属于该集合
  select * from students where cid in (select class_id from classes where class_name like 'Java%');
  ```

##### 8.6.3 子查询返回多个值--多行多列

**示例**:`查询cid=1的班级中性别为男的学生信息`

* 传统方式

    ```sql
    # 多条件查询
    select * from students where cid = 1 and stu_gender = '男';
    ```

* 子查询
  ```sql
  # 括号中的子句，查询结果为所有cid为1的班级中的学生，将这些信息作为一个整体(students的子表或虚拟表，多行多列)，并为该虚拟表起别名为t(虚拟表需要起别名)，再对该虚拟表进行查询
  select * from (select * from students where cid = 1) t where t.stu_gender = '男';
  ```
