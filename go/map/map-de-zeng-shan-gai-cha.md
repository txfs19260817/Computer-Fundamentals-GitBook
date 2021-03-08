---
description: Map的增删改查如何实现？
---

# Map的增删改查？

## 增删改查

### 查找

1. 根据key计算对应hash值；
2. 取hash值低位与`hmap.B`取模确定bucket位置；
3. 取hash值高8位，遍历`tophash`并比较，若找到则得到该`tophash`的下标`i`；
4. 根据`i`在bucket中找到key，如果key相等则返回value；
5. 如果在该bucket没找到，则去`overflow`指向的bucket查找。

如果在扩容过程中，会先去`oldbuckets`指向的数组查找，没找到再查找`buckets`所指的数组。

### 添加

如同查找过程，如果找到key就更新其value，否则找空余位置插入键值对。

如果在扩容过程中，查找会先看`oldbuckets`指向的数组，如果都没有则直接插入`buckets`所指的新数组。

### 删除

对应 Go 语言中的 `delete` 关键字。如同查找过程，如果找到key就把键和值的指针都赋值为`nil`，并把对应的`tophash`置空。

如果在扩容过程中，则先扩容搬迁再删除。

