---
description: 丢失修改问题如何解决？
---

# 丢失修改如何解决？

## 丢失修改

### 场景

![](../../.gitbook/assets/image%20%2832%29.png)

只要应用程序存在 read-modify-write 操作，并且可能存在多个这样的事务同时执行，就很容易出现更新丢失的现象：

* 自增变量（如上图）；
* 更新账户收支记录；
* 多个用户同时修改 wiki，每个用户在保存的时候都将完整的文档发送到服务器。

### 解决方案

#### 原子写操作

```sql
UPDATE counters SET value = value + 1 WHERE key = 'foo'
```

原子操作有两种方法实现：

* 每条数据（object/document）加锁
* 单线程执行

#### 显式加锁 \(Explicit Locking\)

```sql
BEGIN TRANSACTION;

SELECT * FROM figures
 WHERE name = 'robot' AND game_id = 222
 FOR UPDATE;

UPDATE figures SET position = 'c4' WHERE id = 1234;
COMMIT;
```

这里的 FOR UPDATE 语句就是告诉数据库，这条查询涵盖的所有数据都要锁定，在事务结束之前不能被其它事务读写。

这种方案的缺点很明显：开发者需要仔细思考应用程序的逻辑，有时候很容易忘记应当对某些数据加锁，导致出现竞争条件。

#### 自动检测更新丢失

前两种解决方案都是应用程序强制要求 read-modify-write 操作顺序执行。另一种思路是允许这些操作并发执行，让事务管理模块 \(trasaction manager\) 负责检测更新丢失的情况，然后中止相关的事务并强迫它重试。

自动检测更新丢失的好处在于，数据库可以利用 snapshot isolation 方便地实现自动检测。如 PostgreSQL 的 repeatable read、Oracle 的 serializable 以及 SQL Server 的 snapshot isolation 都能够自动检测更新丢失，然而 MySQL/InnoDB 的 repeatable read 并不会自动检测更新丢失。

自动检测更新丢失能够帮助应用开发者减少思考负担，更不容易犯错。

#### Compare-and-Set

在不提供事务支持的数据库中，你有时候可以找到一个原子的 compare-and-set 操作，这个操作只在数据与之前读取的数据一致时才写入新的数据。

#### 冲突解决和数据库复制

在多领导复制及无领导复制场景下，通常的方案是允许数据存在多个版本，然后在应用程序级别上解决或合并这些冲突的数据。原子写操作在多节点数据库中依然适用。另外需要注意的是，许多复制数据库都使用 last write win \(LWW\) 策略来解决冲突，这种做法容易产生更新丢失。

## Reference

1. [https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions\#preventing-lost-updates](https://zhenghe.gitbook.io/open-courses/ddia-bi-ji/transactions#preventing-lost-updates)

