---
description: Goroutine的调度时机有哪些？
---

# Goroutine的调度时机有哪些？

## Goroutine的调度时机

| 情形 | 说明 |
| :--- | :--- |
| 使用关键字`go` | `go`创建一个协程后，调度器会进行一次调度决策 |
| 垃圾回收 | GC有自己一套协程并需要时间在M上运行，GC运行时会做很多调度决策，例如调度不涉及堆访问的 goroutine 来运行。因为GC不管栈上的内存，只会回收堆上的内存。 |
| 系统调用 | 协程进行系统调用时会阻塞 M，因此它会被调度走，同时一个新的协程会被调度进来 |
| 同步 | atomic，mutex，channel 操作等会使协程阻塞，因此会被调度走，直至条件满足再回来 |

## Reference

1. [https://qcrao91.gitbook.io/go/goroutine-tiao-du-qi/goroutine-tiao-du-shi-ji-you-na-xie](https://qcrao91.gitbook.io/go/goroutine-tiao-du-qi/goroutine-tiao-du-shi-ji-you-na-xie)
2. [https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html)
3. [https://medium.com/random-life-journal/scheduling-in-go-727c9b88c93a](https://medium.com/random-life-journal/scheduling-in-go-727c9b88c93a)

