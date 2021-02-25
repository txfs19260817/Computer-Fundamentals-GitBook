---
description: InnoDB解决幻影读的三种锁是什么？
---

# InnoDB的三种行锁是什么？

## 三种行锁

MVCC 不能解决幻影读问题，MySQL 的 InnoDB 提供 Next-Key Locks 可在可重复读（REPEATABLE READ）隔离级别下解决。

### Record Locks

锁定一个记录上的索引，而非记录本身。如果表没有设置索引，InnoDB 会自动在主键上创建隐藏的聚簇索引，因此 Record Locks 依然可以使用。

### Gap Locks

锁定索引之间的间隙，但是不包含索引本身。例如当一个事务执行以下语句，其它事务就不能在 `t.c` 中插入 15。

```sql
SELECT c FROM t WHERE c >= 10 and c <= 20 FOR UPDATE;
```

### Next-Key Locks

它是 Record Locks 和 Gap Locks 的结合，不仅锁定一个记录上的索引，也锁定索引之间的间隙。它锁定一个前开后闭区间，例如一个索引包含以下值：10, 11, 13, 20，那么就需要锁定以下区间：

```text
(-∞, 10]
(10, 11]
(11, 13]
(13, 20]
(20, +∞)
```

Record Locks防止别的事务修改或删除（UPDATE & DELETE），Gap Locks防止别的事务新增（INSERT），Record Locks和Gap Locks结合形成的的Next-Key Locks共同解决了可重复读级别在写数据时的幻读问题。

### 锁降级

Next-Key Locks在以下情况会降级为Record Locks，即仅锁住索引本身，而不是范围：

* 当查询的索引含有唯一属性时；
* 隔离级别设置为读已提交（READ COMMITTED）。

### Example

![](../../.gitbook/assets/image%20%2827%29.png)

## Reference

1. [https://www.cnblogs.com/huan30/p/12307503.html](https://www.cnblogs.com/huan30/p/12307503.html)
2. [https://tech.meituan.com/2014/08/20/innodb-lock.html](https://tech.meituan.com/2014/08/20/innodb-lock.html)
3. [http://www.cyc2018.xyz/数据库/数据库系统原理.html\#六、next-key-locks](http://www.cyc2018.xyz/数据库/数据库系统原理.html#六、next-key-locks)

