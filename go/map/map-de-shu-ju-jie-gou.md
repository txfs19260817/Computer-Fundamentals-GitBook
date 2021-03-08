---
description: Map的数据结构是什么样的？
---

# Map的数据结构？

## 数据结构

![](../../.gitbook/assets/image%20%2863%29.png)

### Map

表示 map 的结构体是 hmap，它是 hashmap 的“缩写”：

```go
type hmap struct {
    count      int            // 保存元素个数，调用len(map)时直接返回此值
    flags      uint8
    B          uint8          // buckets 数目的对数 log_2
	  noverflow  uint16
	  hash0      uint32

	  buckets    unsafe.Pointer // 指向 buckets 数组（[]bmap），大小为 2^B
	  oldbuckets unsafe.Pointer // 指向旧bucket，扩容时buckets长度是oldbuckets的2倍
	  nevacuate  uintptr

	  extra      *mapextra
}

type mapextra struct {
	  overflow     *[]*bmap
	  oldoverflow  *[]*bmap
	  nextOverflow *bmap
}
```

### Bucket

buckets 是一个指针，最终它指向的是一个结构体的**数组（\[\]bmap）**：

```go
type bmap struct {
    tophash [bucketCnt]uint8
}
```

但这只是表面\(src/runtime/hashmap.go\)的结构，编译期间会根据数据类型推导键值对占据的内存空间大小，动态地创建一个新的结构：

```go
type bmap struct {
    topbits  [8]uint8
    keys     [8]keytype
    values   [8]valuetype
    pad      uintptr
    overflow uintptr // 溢出bucket指针，指向地址*bmap
}
```

哈希碰撞发生后，`overflow`指针会指向一个新桶，并在新桶上发生碰撞的key对应的value处存值。

溢出桶只是临时解决哈希碰撞的方案。

## Reference

1. [https://qcrao91.gitbook.io/go/map/map-de-di-ceng-shi-xian-yuan-li-shi-shi-mo](https://qcrao91.gitbook.io/go/map/map-de-di-ceng-shi-xian-yuan-li-shi-shi-mo)
2. [https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-hashmap](https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-hashmap/#%E6%89%A9%E5%AE%B9)

