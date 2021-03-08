---
description: Channel的哪种读取方式不会产生阻塞？
---

# Channel的哪种读取方式不会产生阻塞？

## 无阻塞读取

select的case语句读取管道时不会阻塞，即使管道中没有数据。这是由于case语句编译后调用读管道时会明确传入不阻塞的参数，读不到数据时不会将当前协程加入等待队列，而是直接返回。

```go
for {
    select {
    case e := <- chan1 :
        fmt.Println("chan1: ", e)
    case e := <- chan2 :
        fmt.Println("chan2: ", e)
    default:
        fmt.Println("No elem")
        time.Sleep(time.Second)
    }
}
```

## 阻塞读取

其他的读取方法都会在管道为空时阻塞当前协程，包括直接读取和for-range循环读取。

for-range读已关闭的管道时的表现如同slice，读完即退出。

