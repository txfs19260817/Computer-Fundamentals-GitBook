---
description: EXPLAIN关键字打印信息的含义？
---

# EXPLAIN关键字打印信息的含义？

## EXPLAIN

开发人员可以通过分析 EXPLAIN 结果来优化查询语句。比较重要的字段有：

* select\_type : 查询类型，有简单查询SIMPLE、联合查询UNION、子查询SUBQUERY等；
* table：正在访问哪个表；
* type：访问类型，有全表扫描（ALL按行，index按索引），范围扫描（range有限制索引扫描），无索引NULL等；
* key : 使用的索引；
* rows : 扫描的行数；
* Extra：其他重要信息，例如使用索引覆盖Using index。

