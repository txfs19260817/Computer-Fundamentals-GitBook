---
description: Goroutine的调度策略是怎样的？
---

# Goroutine的调度策略？

## 调度策略

Goroutine调度器不断演进，下面介绍一些基础的调度策略。

### 队列轮转

处理器P维护一个协程G的队列，P依次将G调度到M中运行。此外，P还会周期性检查全局队列中的G，这些G主要来自从系统调用恢复的G，如果全局队列里有G可调用的话P就会取来调度到M执行，避免“饿死”。

### 系统调用

![](../../.gitbook/assets/image%20%2872%29.png)

如图，G0即将进入系统调用，则此时M0会释放P，该P被M1接管，M1可能是新建的也可能是从线程缓存（池）取出的空闲M。

M0以阻塞状态继续“运行”G0。当系统调用返回：

* 如果有空闲P，则获取一个来继续执行G0；
* 否则将G0放入全局队列等待被取走，M0存入线程缓存（池）。

### 工作量窃取

![](../../.gitbook/assets/image%20%2873%29.png)

当某个P没有可调度的协程时，会从其他P偷取协程，一般是偷取另一个P本地队列的一半。

### 抢占式调度

Go 1.14之前是基于协作的抢占式调度，在该设计中，如果协程没有函数调用，会无限期占用执行权。

```go
func main() {
	runtime.GOMAXPROCS(1)
	go func() {
		for {
			// 无函数调用的无限循环
		}
	}()
	time.Sleep(1 * time.Second) // 系统调用，出让执行权给上面的协程
	fmt.Println("Hello, playground")
}
```

Go 1.14之后采用基于信号的抢占式调度解决该问题。

## Reference

1. [https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/)
2. [https://morsmachine.dk/go-scheduler](https://morsmachine.dk/go-scheduler)

