---
title: MySQL数据库教程
date: 2022-09-06 19:25:33
tags:
- MySQL
- 后端
categories:
- 数据库
---

# MySQL初级篇

## 1. 基本的select语句

**SQL的分类**

- DDL**（Data Definition Languages、数据定义语言）** ，这些语句定义了不同的数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。主要的语句关键字包括`CREATE`、`DROP`、`ALTER`、`RENAME`、`TRUNCATE`等。

- DML:**（Data Manipulation Language、数据操作语言）** ，用于添加、删除、更新和查询数据库记录(增删改查)，并检查数据完整性。主要的语句关键字包括`INSERT`、`DELETE`、`UPDATE`、`SELECT`等。SELECT是SQL语言的基础，最为重要。

- DCL:**（Data Control Language、数据控制语言）**，用于定义数据库、表、字段、用户的访问权限和安全级别。主要的语句关键字包括`GRANT`、`REVOKE`、`COMMIT`、`ROLLBACK`、`SAVEPOINT`等。

> 因为查询语句使用的非常的频繁，所以很多人把查询语句单拎出来一类：DQL（数据查询语言）。还有单独将`COMMIT`、`ROLLBACK` 取出来称为TCL （Transaction Control Language，事务控制语言）。

<!--more-->

## 2. 多表查询

1. 自然连接(NATURAL JOIN)

   它会帮你自动查询两张连接表中所有相同字段,然后进行等值连接.

2. **时间戳**:是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。

3. `SELECT`语句的完整结构:

```mysql
#SQL92语法
SELECT (DISTINCT)..., ...,...(存在聚合函数)
FROM ...,...
WHERE 多表的连接条件 AND 不包含聚合函数的过滤条件
GROUP BY ...,...
HAVING 包含聚合函数的过滤条件
ORDER BY ...,...(ASC / DESC)
LIMIT ...,...
```

```mysql
#SQL99语法
SELECT (DISTINCT)..., ...,...(存在聚合函数)
FROM ... 
(LEFT / RIGHT)JOIN ... ON 多表的连接条件
JOIN ... ON ...
WHERE 不包含聚合函数的过滤条件
GROUP BY ...,...
HAVING 包含聚合函数的过滤条件
ORDER BY ...,...(ASC / DESC)
LIMIT ...,...
```

4. SQL语句的执行过程:

``` mysql
FROM ...,... -> ON -> (LEFT / RIGHT JOIN) -> WHERE -> GROUP BY -> HAVING -> 
SELECT -> DISTINCT -> ORDER BY -> LIMIT
```

5. 内连接和外连接
   - 内连接: 合并具有同一列的两个以上的表的行, 结果集中不包含一个表与另一个表不匹配的行 
   - 外连接: 两个表在连接过程中除了返回满足连接条件的行以外还返回左（或右）表中不满足条件的 行 ，这种连接称为左（或右） 外连接。没有匹配的行时, 结果表中相应的列为空(NULL)。 
   - 如果是左外连接，则连接条件中左边的表也称为 主表 ，右边的表称为 从表 。 如果是右外连接，则连接条件中右边的表也称为 主表 ，左边的表称为 从表 。

## 3. 聚合函数

1. `SELECT`中出现的非组函数(非聚合函数)的字段必须声明在`GROUP BY`中,反之,`GROUP BY`中声明的字段可以不出现在`SELECT`中
1. 如果过滤条件中使用了聚合函数,则必须使用`HAVING`来替换`WHERE`,且`HAVING`必须声明在`GROUP BY`后面,开发中使用`HAVING`的前提是SQL中使用了`GROUP BY`
1. 当过滤条件中有聚合函数时,此过滤条件必须声明在`HAVING`中,当过滤条件中没有聚合函数时,则此过滤条件声明在`WHERE`中或`HAVING`中都可以,但建议声明在`WHERE`中

## 4. 子查询

1. 子查询的分类

   角度1:从内查询返回的结果的条目数

   - 单行子查询:子查询只计算出一个结果
   - 多行子查询:子查询计算出多个结果

   角度2:内查询是否被执行多次

   - 相关子查询(内查询执行多次,里外有相关性)
   - 不相关子查询

2. 子查询的编写技巧（或步骤）：

   - 从里往外写
   - 从外往里写
   - 选择经验:
     1. 如果子查询相对简单,建议从外往里写.一旦子查询结构较为复杂,建议从里往外写
     2. 如果是相关子查询,通常从外往里写

3. 结论：在SELECT中除GROUP BY和LIMIT之外，其他位置都可以声明子查询

4. 可以先排序（升序）再取第一条记录就是最小的数据

## 5. 创建和管理表

1. `TRUNCATE TABLE` 一旦执行此操作,表数据全部清除,数据不可以回滚.`DELETE FROM` 一旦执行此操作,表数据可以全部清除(可以用WHERE过滤出需要清除的数据,不带WHERE全部清除),数据可以回滚(默认情况下不能回滚,执行了`SET autocommit=FALSE`,则之后执行的DML操作可以回滚 ),执行完DDL之后会自动执行一次COMMIT,此COMMIT操作不受`SET autocommit=FALSE`的影响
1. 字符和日期类型数据应该包含在单引号中，给字段或表取别名包含在双引号中

## 6. 数据类型

1. 简短和固定长度的字符串用CHAR，其他情况都用VARCHAR
2. 在定义数据类型时，如果确定是整数，就用`INT`；如果是小数，就用`DECIMAL(M,D)`，M表示一共多少位，D表示小数部分有多少位；如如果是日期与时间，就用`DATETIME`

## 7. 约束

1. 唯一性约束允许出现多个NULL，可以向声明为unique的字段上添加NULL值
1. 主键约束的特征：非空且唯一，用于唯一的标识表中的一条记录。MySQL的主键名总是PRIMARY，自己取名也没用
1. 对于外键约束，最好采用：`ON UPDATE CASCADE ON DELETE RESTRICT` (CASCADE表示主表与从表同步操作，RESTRICT是严格模式，从表中还有主表关联的数据，主表就不能删除该数据)

# MySQL高级篇

## 1. 
