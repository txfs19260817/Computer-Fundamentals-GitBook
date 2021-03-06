---
description: 什么情况下索引会失效呢？怎么查看索引是否生效？
---

# 什么情况下索引会失效？

## 索引失效

### 查看是否使用了索引

![](../../.gitbook/assets/image%20%2840%29.png)

使用explain关键字，查询后查看结果的key字段。若是使用了索引，该字段会展示索引的名字，否则该字段为NULL。

### 索引失效情况

1. **where条件查询中使用了or关键字**，有可能使用了索引进行查询也会导致索引失效。若是想使用or关键字，又不想索引失效，只能在or的所有列上都建立索引；
2. **使用like关键字，并且不符合最左前缀原则**，会导致索引失效。如"%word"；
3. **字段是字符串，而错误的使用where column = 123 数字类型**；
4. **联合索引查询不符合最左前缀原则**；
5. **where条件查询的后面对字段进行null值判断**，会导致索引失效，解决的办法就是可以把null改为0或者-1这些特殊的值代替；
6. **where子句中使用!= ,&lt; &gt;这样的符号**；
7. **where条件子句中=的左边使用表达式操作或者函数操作**；
8. **全表扫描更快**时（数据少）。

## Reference

1. [https://blog.csdn.net/qq\_43255017/article/details/109428962](https://blog.csdn.net/qq_43255017/article/details/109428962)
2. [https://www.cnblogs.com/liehen2046/p/11052666.html](https://www.cnblogs.com/liehen2046/p/11052666.html)

