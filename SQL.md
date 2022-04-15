## SQL

### 1.1 SQL概述

#### 1.1.1 SQL基本信息

> SQL(Structured Query Language):结构化查询语言，用于存取、查询、更新数据以及管理 关系型数据库系统。

> SQL由ANSI组织确定规范。

> 在不同的数据库产品中遵守SQL的通用击落，但是也对SQL有一些不同的改进，形成了一些数据库的专有指令
>
> * MySQL---limit
> * SQLServer--top
> * Oracle--rownum

#### 1.1.2 SQL分类

> 根据SQL指令完成的数据库操作的不同，可以将SQL指令分为四类:
>
> * DDL(Data Definition Language):数据定义语言
>   * 用于完成对数据库对象(数据库、数据表、视图、索引等)的创建、删除、修改。
> * DML(Data Manipulation Language):数据操作/操纵语言
>   * 用于完成对数据表中的数据的添加、删除、修改操作
>   * 添加：将数据存储到数据表中
>   * 删除:  将数据从数据表中移除
>   * 修改:  对数据表中的数据进行修改
> * DQL(Data Query Language):数据查询语言
>   * 用于将数据表中的数据查询出来
> * DCL(Data Control Language):数据控制语言
>   * 用于完成事务管理等控制性操作

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
## <db_name>：表示数据库的名字

## 显示当前MySQL中的数据库列表
show databases;

## 显示指定名称的数据库的创建的SQL命令
show create database <db_name>;
```

**创建数据库**

```sql
## <db_name>：表示数据库的名字

## 创建数据库
create database <db_name>;

## 创建数据库，当前指定名称的数据库不存在时执行创建
create database if not exists <db_name>;

## 在创建数据库的同时指定数据库的字符集
create database <db_name> character set utf8;
```

**修改数据库**

```sql
## <db_name>：表示数据库的名字

## 修改数据库的字符集
alter database <db_name> character set utf8;
```

**删除数据库**

```sql
## <db_name>：表示数据库的名字

## 删除数据库
drop database <db_name>;

## 如果数据库存在则删除数据库
drop database if exists <db_name>;
```

**使用/切换数据库**

```sql
## <db_name>：表示数据库的名字

## 切换当前使用的数据库
use <db_name>;
```

#### 1.3.2 DDL-数据表操作

**创建数据表**

> 数据表实际就是一个二维的表格，一个表格是由多列组成，表格中的每一列称为表格中的字段(**Feild**);

| stu_NO   | stu_name | stu_age |
| -------- | -------- | ------- |
| 20220415 | 张三     | 18      |
| 20220415 | 李四     | 19      |

```sql
## 创建上图所示的表
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

desc <table-name>
```

**删除数据表**

```sql
## <table-name>:表示表的名字

## 删除数据表
drop table <table-name>
## 当前数据表存在时删除数据表
drop table if exists <table-name>
```

**修改数据表**

```sql
## <table-name>、<new-talbe-name>表示表的名字
## <feild-name>、<old-feild-name>、<new-feild-name>表示字段的名字
## <type>、<newType>表示int、char等类型


## 修改表名
alter table <table-name> rename to <new-talbe-name>;

## 数据表也是有字符集的，默认与所属数据库一致
alter table <table-name> character set utf8;

## 添加字段(列)
alter table <table-name> add <feild-name> <type>;

## 修改字段的名字和类型
alter table <table-name> change <old-feild-name> <new-feild-name> <type>;

## 只修改字段类型
alter table <table-name> modify <feild-name> <newType> 

## 删除表中的字段
alter table <table-name> drop <feild-name>;
```

### 6.4 MySQL数据类型

> 数据类型，指的是数据表中的列中支持存放的数据的类型。

#### 6.4.1数值类型

在MySQL中有多种数值类型，不同的类型存放的数值范围或者形式是不同的。

| 类型             | 内存占用大小             | 数值范围        | 说明                                                                              |
| ---------------- | ------------------------ | --------------- | --------------------------------------------------------------------------------- |
| tinyint          | 1B                       | [-2^7, 2^7-1]   | 特定小型整数                                                                      |
| smallint         | 2B                       | [-2^15, 2^15-1] | 小型整数                                                                          |
| mediumint        | 3B                       | [-2^23, 2^23-1] | 中型整数                                                                          |
| **int**    | 4B                       | [-2^31, 2^31-1] | 整数                                                                              |
| bigint           | 8B                       | [-2^63, 2^63-1] | 大整数                                                                            |
| float            | 4B                       |                 | 单精度                                                                            |
| **double** | 8B                       |                 | 双精度                                                                            |
| decimal          | 根据声明不同占用大小不同 |                 | decimal(10,2):表示值为10进制数表示时，数字的有效个数最大为10，其中小数部分占2位。 |
