# 数据库

## 基本概念

**数据库（database）** 是用来组织、存储和管理数据的仓库。用户可以对数据库中的数据进行新增、查询、更新、删除等操作。

::: info 常见的数据库及分类

市面上的数据库有很多种，最常见的数据库有如下几个：

- MySQL 数据库（目前使用最广泛、流行度最高的开源免费数据库；Community + Enterprise）
- Oracle 数据库（收费）
- SQL Server 数据库（收费）
- MongoDB 数据库（Community + Enterprise）

其中，MySQL、Oracle、SQL Server 属于传统型数据库（又叫做：关系型数据库 或 SQL 数据库），这三者的设计理念相同，用法比较类似。  
而 MongoDB 属于新型数据库（又叫做：非关系型数据库 或 NoSQL 数据库），它在一定程度上弥补了传统型数据库的缺陷。

:::

> 在传统型数据库中，数据的组织结构分为数据库(database)、数据表(table)、数据行(row)、字段(field)这 4 大部分组成。类似于Excel的工作簿、工作表、每一行数据、列。

## 安装并配置 MySQL

对于开发人员来说，只需要安装 **MySQL Server** 和 **MySQL Workbench** 这两个软件，就能满足开发的需要了。

- **MySQL Server**: 专门用来提供数据存储和服务的软件。
- **MySQL Workbench**: 可视化的 MySQL 管理工具，通过它，可以方便的操作存储在 MySQL Server 中的数据。

## 使用SQL管理数据库

### 概述

**1. 什么是 SQL**

SQL（Structured Query Language）是结构化查询语言，专门用来访问和处理数据库的编程语言。

三个关键点：

1. SQL 是一门数据库编程语言
2. 使用 SQL 语言编写出来的代码，叫做 SQL 语句
3. SQL 语言只能在关系型数据库中使用（例如 MySQL、Oracle、SQL Server）。非关系型数据库（例如 MongoDB）不支持 SQL 语言

---

**2. SQL 的学习目标**

> 重点掌握如何使用 SQL 从数据表中：
>
> 查询数据（select）、插入数据（insert into）、更新数据（update）、删除数据（delete）

> 额外需要掌握的 4 种 SQL 语法：
>
> where 条件、and 和 or 运算符、order by 排序、count(\*) 函数

### 增删改查

**1. 增**

```
INSERT INTO 语句用于向数据表中插入新的数据行，语法格式如下：

1 -- 语法解读: 向指定的表中, 插入如下几列数据, 列的值通过 values ——指定
2 -- 注意: 列和值要一一对应, 多个列和多个值之间, 使用英文的逗号分隔
3 INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,...)
```

---

**2. 删**

```
DELETE 语句用于删除表中的行。语法格式如下：

1 -- 语法解读:
2 -- 从指定的表中, 根据 WHERE 条件, 删除对应的数据行
3 DELETE FROM 表名称 WHERE 列名称 = 值
```

---

**3. 改**

```
Update 语句用于修改表中的数据。语法格式如下：

1 -- 语法解读:
2 -- 1. 用 UPDATE 指定要更新哪个表中的数据
3 -- 2. 用 SET 指定列对应的新值
4 -- 3. 用 WHERE 指定更新的条件
5 UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值

6

7 -- 更新多列示例：
8 update users set password='admin123', status=1 where id=2

```

---

**4. 查**

```
SELECT 语句用于从表中查询数据。执行的结果被存储在一个结果表中（称为结果集）。语法格式如下：

1 -- 这是注释
2 -- 从 FROM 指定的【表中】，查询出【所有的】数据。 * 表示【所有列】
3 SELECT * FROM 表名称

4

5 -- 从 FROM 指定的【表中】，查询出指定 列名称（字段）的数据。
6 SELECT 列名称 FROM 表名称

```

::: warning ⚠️ 注意

SQL 语句中的关键字对大小写不敏感。SELECT 等效于 select，FROM 等效于 from。  
多个列使用英文逗号进行分隔

:::

### where、and和or

### 排序order by

### count函数和as关键字

## nodejs中使用MySQL

### 引入MySQL模块

### 增删改查
