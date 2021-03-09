---
description: 什么是协程？
---

# 什么是协程？

## 协程

### 定义

协程可理解为一种轻量级线程，与线程相比，协程工作在用户态。而且不受操作系统调度，协程调度器由用户应用程序提供。

Go的协程Goroutine同样有挂起、就绪和运行三种状态。

### 优势

![](../../.gitbook/assets/image%20%2870%29.png)

多线程往往采用线程池策略，其中预先有一部分线程，它们不断地从任务队列领取任务。但线程中执行的任务发生系统调用，操作系统会将该线程置为阻塞状态，致使线程池消费任务能力下降。如果增加线程以提高线程池处理能力的话，过多的线程会导致上下文切换的开销变大。

![](../../.gitbook/assets/image%20%2871%29.png)

工作在用户态的协程则能大大减少上下文切换的开销。协程调度器把可运行的协程逐个调度到线程中执行，同时及时把阻塞的协程调度出线程，从而有效避免线程频繁切换，达到少线程高并发的效果。
