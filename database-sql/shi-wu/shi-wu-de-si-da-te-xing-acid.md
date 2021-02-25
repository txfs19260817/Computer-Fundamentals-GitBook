---
description: 事务的四大特性(ACID)？
---

# 事务的四大特性\(ACID\)？

## ACID简介

![](../../.gitbook/assets/image%20%2824%29.png)

 **原子性（Atomicity）、一致性（Consistent）、隔离性（Isalotion）、持久性（Durable）**

1. 原子性：事务的原子性操作，对数据的修改要么**全部执行成功，要么全部失败**。实现事务的原子性，是基于日志的Redo/Undo机制。
2. 一致性：事务将数据库从一种状态转换成下一种一致的状态，事务开始之前与结束以后数据库的完整性约束没有被破坏，如唯一性、外键等，需要由应用本身来保证。
3. 隔离性：事务之间相互隔离，每个事务都认为只有自己在执行。与事务隔离级别有密切关系。
4. 持久性：一旦事务提交，则其所做的修改将会永远保存到数据库中。即使系统发生崩溃，事务执行的结果也不能丢失。

## Reference

1. [http://www.cyc2018.xyz/数据库/数据库系统原理.html](http://www.cyc2018.xyz/数据库/数据库系统原理.html)
2. [https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions\#acid](https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions#acid)
3. [https://blog.csdn.net/qq\_43255017/article/details/106442887](https://blog.csdn.net/qq_43255017/article/details/106442887)

