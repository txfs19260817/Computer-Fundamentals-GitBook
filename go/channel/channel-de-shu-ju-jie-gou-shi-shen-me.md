---
description: Channel的数据结构是什么？
---

# Channel的数据结构是什么？

## 数据结构 <a id="shu-ju-jie-gou"></a>

保存数据的结构是一个环形队列，有两个读写指针分别指向下一个被读取的元素位置和下一个可放置数据的空位置。

![](../../.gitbook/assets/image%20%2865%29.png)

```go
type hchan struct {
    qcount   uint           // Channel 中的元素个数
    dataqsiz uint           // Channel 中的环形/循环队列的长度
    buf      unsafe.Pointer // 指向底层循环数组的指针（只针对有缓冲的 channel）
    elemsize uint16         // 元素大小
    closed   uint32         // 是否被关闭的标志
    elemtype *_type         // 元素类型
    sendx    uint           // 队列下标，指示元素写入时存放至队列中的位置
    recvx    uint           // 队列下标，指示下一个被读取的元素在队列中的位置
    recvq    waitq          // 等待读的 goroutine 队列
    sendq    waitq          // 等待写的 goroutine 队列
    lock     mutex          // 互斥锁，保护 hchan 中所有字段，不允许并发读写
}
```

goroutine 队列的`waitq` 类型是 `sudog` 的一个双向链表，而 `sudog` 实际上是对 goroutine 的一个封装：

```go
type waitq struct {
    first *sudog
    last  *sudog
}
```

## Reference

1. [https://qcrao91.gitbook.io/go/channel/channel-di-ceng-de-shu-ju-jie-gou-shi-shi-mo](https://qcrao91.gitbook.io/go/channel/channel-di-ceng-de-shu-ju-jie-gou-shi-shi-mo)

