---
description: Goroutine的GPM调度模型是怎么样的？
---

# GPM调度模型？

## GPM

* M \(Machine\): 工作线程，由操作系统调度；
* P \(Processor\): 处理器（Go的概念），包含运行Go代码的必要资源，也有调度Goroutine的能力；
* G \(Goroutine\): 协程。

每个`go`关键字会创建一个协程。P的个数在启动时决定，默认为逻辑处理器数目。M必须持有P才能执行代码，且跟线程一样会被系统调用阻塞，数目稍大于P，因为还有其他内置任务处理（如GC）。

![](../../.gitbook/assets/image%20%2868%29.png)

每个M持有一个P，且每个M中有一个G在运行。每个P有一个Local Run Queue \(LRQ\)存放等待被调度的G。此外还有Global Run Queue \(GRQ\)，P可以从此取G去运行。如果LRQ满了，新来的G也会被放入GRQ。

## Reference

1. [https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html)
2. [https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine)

