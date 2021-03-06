---
description: 最左匹配原则是什么？为什么会有最左匹配原则？
---

# 最左匹配前缀原则是什么？

## 最左匹配前缀原则

### 联合索引查找

最左前缀原则可以是联合索引的的最左N个字段，也可以是字符串索引的最左的M个字符。举个例子，假如现在有一个表的原始数据如下所示：

![](../../.gitbook/assets/image%20%2837%29.png)

并根据col3 ，col2的顺序建立联合索引，此时联合索引树结构如图下所示：

![](../../.gitbook/assets/image%20%2836%29.png)

叶子结点中首先会根据col3的字符进行排序，若是col3相等，在col3相等的值里面再对col2进行排序，假如我们要查询where col3 like 'Eri%'，就可以快速的定位查询到Eric。

若是查询条件为where col3 like ‘%se’，前面的字符不确定，表示任意字符都可以，这样就可以导致全表扫描进行字符的比较，就会使索引失效。

### 为什么会有最左匹配前缀原则

假设创建了index\_bcd\(b,c,d\)索引，就相当于创建了\(b\), \(b,c\), \(b,c,d\)三个索引，索引在节点的排序是先按b排序，宏观上c和d是无序的，但是在b相等的时候c有序，同理c相等的时候d有序。因此查找的时候也是按照b, c, d的顺序依次查找。

也因此如果你的查找条件不包含b列如\(c,d\), \(c\), \(d\)是无法应用缓存的，以及跨列也是无法完全用到索引，如\(b,d\)，只会用到b列索引。

![](../../.gitbook/assets/image%20%2844%29.png)

## Reference

1. [https://juejin.cn/post/6844904073955639304](https://juejin.cn/post/6844904073955639304)
2. [https://blog.csdn.net/qq\_43255017/article/details/109428962](https://blog.csdn.net/qq_43255017/article/details/109428962)

