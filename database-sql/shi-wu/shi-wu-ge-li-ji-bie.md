---
description: 事务隔离级别都有哪些？
---

# 事务隔离级别？

## 事务隔离级别

![](../../.gitbook/assets/image%20%2828%29.png)

### 读未提交（READ UNCOMMITTED）

事务中的修改，即使没有提交，对其它事务也是可见的。

导致的问题：脏读。

### 读已提交（READ COMMITTED）

![](../../.gitbook/assets/image%20%2829%29.png)

一个事务只能读取已经提交的事务所做的修改。换句话说，一个事务所做的修改在提交之前对其它事务是不可见的。

是Oracle、PostgreSQL、SQL Server2012的默认隔离级别。

实现方式：

* 写：使用行级别锁 \(row-level locks\)。当事务 A 想要修改 table 中的一行或者 collection 中的一个文档，它需要先获得相应的（互斥）锁，然后持有这个锁直到事务结束。由于只有一个事务能够持有锁，其它想要写入数据的事务就只能等待事务 A 释放锁。
* 读：当写事务 A 正在进行时，数据库会自动保存一份旧的数据供其它事务读取直到 A 结束，在此期间其它事务可以放心地读，无需加锁。当 A 结束时，数据库才将读请求转移到新数据上，清理旧数据。

导致的问题：不可重复读。见下图。

![&#x4E0D;&#x53EF;&#x91CD;&#x590D;&#x8BFB;&#x5E26;&#x6765;&#x7684;&#x95EE;&#x9898;](../../.gitbook/assets/image%20%2832%29.png)

### 可重复读（REPEATABLE READ）

保证在同一个事务中多次读取同一数据的结果是一样的。

是MySQL InnoDB的默认隔离级别。

实现方式：MVCC。

导致的问题：幻影读（实施了MySQL InnoDB的Next-Key Locks除外）。

### 可串行化（SERIALIZABLE）

强制事务串行执行，这样多个事务互不干扰，不会出现并发一致性问题。该隔离级别需要加锁实现，因为要使用加锁机制保证同一时间只有一个事务执行，也就是保证事务串行执行。

导致的问题：效率低。

## Reference

1. [https://zhenghe.gitbook.io/open-courses/cmu-15-445-645-database-systems/timestamp-ordering-concurrency-control\#isolation-level](https://zhenghe.gitbook.io/open-courses/cmu-15-445-645-database-systems/timestamp-ordering-concurrency-control#isolation-level)
2. [https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions\#read-committed](https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions#read-committed)
3. [https://en.wikipedia.org/wiki/Isolation\_\(database\_systems\)](https://en.wikipedia.org/wiki/Isolation_%28database_systems%29)
4. [https://blog.csdn.net/qq\_43255017/article/details/106442887](https://blog.csdn.net/qq_43255017/article/details/106442887)

