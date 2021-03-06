---
description: Map是如何扩容的？
---

# Map如何扩容？

## 扩容

### 条件

下列条件满足其一便会发生扩容：

* 装载因子（元素个数/bucket数目）大于6.5，可以理解成平均每个bucket键值对超过6.5个；
* 溢出桶overflow数量大于2^15。

### 策略

考虑到map有可能存储大量键值对，则一次性搬迁的开销会很大。因此Go采用逐步搬迁策略，即每次访问map时会触发一次搬迁，每次搬运2个键值对。

### 增量扩容步骤

1. 当hmap的`buckets`指针指向的bucket数组的装载因子大于6.5时，`oldbuckets`指针接管该数组；
2. `buckets`指针指向新创建的bucket数组，其长度是原先的2倍；
3. 把`oldbuckets`指向的旧数组的值逐步搬运到新数组空间，如果有新值要插入也插在新数组；
4. 搬迁完成后释放`oldbuckets`指向的旧数组。

### 等量扩容

经过大量增删后，数据有可能只集中在一小部分bucket中，即其他大部分bucket是空的。因此等量扩容操作会重新组织这些数据的位置摆放。操作完成后buckets数目不变，`overflow`指向的溢出桶数目减少。





