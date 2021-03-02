---
description: 什么是索引下推？
---

# 什么是索引下推？

## 索引下推（Index Condition Pushdown，ICP）

 **在索引遍历过程中，对索引中包含的字段先做判断，过滤掉不符合条件的记录，减少回表次数**。

假设查询表中“名字第一个字是张，性别男，年龄为10岁的所有记录”。那么，查询语句是这么写的：

```sql
select * from tuser where name like '张%' and age=10 and ismale=1;
```

根据“最左前缀原则”，该语句在搜索索引树的时候，**只能**匹配到名字第一个字是“张”的记录（即记录ID3），后面的索引全部失效。因此接下来就是从ID3开始，逐个回表（查找聚簇索引），到主键索引上找出相应的记录，再比对age和ismale这两个字段的值是否符合。

![&#x65E0;&#x7D22;&#x5F15;&#x4E0B;&#x63A8;](../../.gitbook/assets/image%20%2848%29.png)

而MySQL 5.6引入了索引下推优化，在回表前先用age做了筛选，对于age不等于10的记录直接跳过，这样回表次数就从4次下降至2次。

![&#x6709;&#x7D22;&#x5F15;&#x4E0B;&#x63A8;](../../.gitbook/assets/image%20%2839%29.png)

## Reference

1. [https://www.cnblogs.com/kkbill/p/11354685.html](https://www.cnblogs.com/kkbill/p/11354685.html#6-%E7%B4%A2%E5%BC%95%E4%B8%8B%E6%8E%A8)

