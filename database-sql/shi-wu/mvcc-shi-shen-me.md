---
description: MVCC是什么？它的过程是什么？
---

# MVCC是什么？

## MVCC

### 优势

> 写不阻塞读；读不阻塞写。

### 概念

每个事务都有自己的TRX\_ID，并且是唯一的、递增的 。在 MVCC 中事务的修改操作（DELETE、INSERT、UPDATE）会为数据行新增一个版本快照（初始还有一个版本），并赋予给它TRX\_ID。MVCC 规定只能读取已经提交的快照和自身未提交的快照。

### Undo log

![](../../.gitbook/assets/image%20%2833%29.png)

MVCC 的多版本指多个版本的快照，快照存储在 Undo log中，该日志通过回滚指针 ROLL\_PTR 把一个数据行的所有快照连接起来（图中的U）。如图中的V1和V2版本并非真实存在，而是通过当前V3和Undo log计算出来的。

### ReadView

ReadView 结构主要包含了当前系统未提交的事务列表 TRX\_IDs {TRX\_ID\_1, TRX\_ID\_2, ...}，还有该列表的最小值 TRX\_ID\_MIN 和 TRX\_ID\_MAX。

![](../../.gitbook/assets/image%20%2830%29.png)

进行 SELECT 操作时，根据数据行快照的 TRX\_ID 与 TRX\_ID\_MIN 和 TRX\_ID\_MAX 之间的关系，从而判断数据行快照是否可以使用：

* TRX\_ID &lt; TRX\_ID\_MIN，表示该数据行快照时在当前所有未提交事务之前进行更改的，故可用；
* TRX\_ID &gt; TRX\_ID\_MAX，表示该数据行快照是在事务启动之后被更改的，故不可用；
* TRX\_ID\_MIN &lt;= TRX\_ID &lt;= TRX\_ID\_MAX，需要根据隔离级别再进行判断：
  * 读已提交：如果 TRX\_ID 在 TRX\_IDs 列表中，表示该数据行快照对应的事务还未提交，则该快照不可使用。否则表示已经提交，可以使用。
  * 可重复读：都不可以使用。因为如果可以使用的话，那么其它事务也可以读到这个数据行快照并进行修改，那么当前事务再去读这个数据行得到的值就会发生改变，即不可重复读。

在数据行快照不可使用的情况下，需要沿着 Undo Log 的回滚指针 ROLL\_PTR 找到下一个快照，再进行上面的判断。

### 快照读与当前读

**1. 快照读**

MVCC 的 SELECT 操作是快照中的数据，不需要进行加锁操作。

```sql
SELECT * FROM table ...;
```

**\#2. 当前读**

MVCC 其它会对数据库进行修改的操作（INSERT、UPDATE、DELETE）需要进行加锁操作，从而读取最新的数据。可以看到 MVCC 并不是完全不用加锁，而只是避免了 SELECT 的加锁操作。

在进行 SELECT 操作时，可强制指定进行加锁操作。下面第1个语句需要加 S 锁，第2个需要加 X 锁。

```sql
SELECT * FROM table WHERE ? lock in share mode;
SELECT * FROM table WHERE ? for update;
```

## Reference

1. [http://www.cyc2018.xyz/数据库/数据库系统原理.html\#五、多版本并发控制](http://www.cyc2018.xyz/数据库/数据库系统原理.html#五、多版本并发控制)
2. [https://blog.csdn.net/qq\_43255017/article/details/106442887](https://blog.csdn.net/qq_43255017/article/details/106442887)

