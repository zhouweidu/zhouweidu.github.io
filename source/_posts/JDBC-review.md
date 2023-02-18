---
title: JDBC review
date: 2022-09-06 19:10:39
tags:
- JDBC
- 后端
categories:
- 后端
---

# JDBC核心技术

## 1.数据库事务

### 1.1什么是数据库事务

事务：一组逻辑操作单元（一个或多个DML操作），使数据从一种状态变换到另一种状态

### 1.2哪些操作会导致数据自动提交

- DDL操作，`set autocommit=false`的方式对DDL无效
- DML操作m默认情况下，一旦执行也会自动提交，可以通过`set autocommit=false`的方式取消DML操作的自动提交
- 默认在关闭连接时，会自动提交数据

## 2.DAO:data(base) access object(数据访问对象)

## 一些其他知识

1. url:uniform resource locator (统一资源定位符)
