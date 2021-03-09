---
description: Go的内存布局如何设计？
---

# 内存布局如何设计？

## 虚拟内存布局

![](../../.gitbook/assets/image%20%2874%29.png)

在1.10之前是线性内存。

* `spans` 区域存储了指向内存管理单元 `mspan` 的指针，每个内存单元会管理几页的内存空间，每页8KB；
* `bitmap` 用于标识 `arena` 区域中的那些地址保存了对象，位图中的每个字节都会表示堆区中的 32 字节是否包含空闲，主要用于GC；
* `arena` 区域是真正的堆区，划分为每页8KB，这些内存页中存储了所有在堆上初始化的对象；

![](../../.gitbook/assets/image%20%2875%29.png)

从1.11起改用稀疏内存方案。这解决了申请大块的内存空间而不使用、移除堆大小的上限和C 和 Go 混合使用时的地址空间冲突等问题。

```go
type heapArena struct {
	bitmap       [heapArenaBitmapBytes]byte // 内存中每个32字节是否空闲
	spans        [pagesPerArena]*mspan      // 指向内存管理单元mspan的指针
	pageInUse    [pagesPerArena / 8]uint8
	pageMarks    [pagesPerArena / 8]uint8
	pageSpecials [pagesPerArena / 8]uint8
	checkmarks   *checkmarksMap
	zeroedBase   uintptr                    // 该结构体管理的内存的基地址
}
```

每个`heapArena`单元都会管理 64MB 的内存空间。

![&#x603B;&#x89C8;](../../.gitbook/assets/image%20%2877%29.png)

## Reference

1. [https://draveness.me/golang/docs/part3-runtime/ch07-memory/golang-memory-allocator/](https://draveness.me/golang/docs/part3-runtime/ch07-memory/golang-memory-allocator/#%E8%99%9A%E6%8B%9F%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80)
2. [https://golang.design/under-the-hood/zh-cn/part2runtime/ch07alloc/basic/](https://golang.design/under-the-hood/zh-cn/part2runtime/ch07alloc/basic/)

