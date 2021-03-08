---
description: 关闭一个channel会发生什么？向一个关闭的channel读写数据会怎样？
---

# 关闭一个channel会发生什么？

## 关闭一个channel会发生什么？

关闭一个channel时会把`recvq`中所有协程唤醒，这些协程获得的数据都是对应类型的零值。同时会把`sendq`队列中所有协程唤醒，但这些协程会panic。

## 一个关闭的channel……

### 向一个关闭的channel写入数据会？

Panic。

### 向一个关闭的channel读取数据会？

可以正常读取，直至把channel读空，再读取的话会返回一个零值（取决于channel存储数据类型，例如int是0，bool是false）和`ok = false`，表示channel已没有数据。

```go
// https://play.golang.com/p/uurR18TiNuL
func main() {
    ch := make(chan int, 5)
    ch <- 18
    close(ch)
    x, ok := <-ch
    if ok {
        fmt.Println("received: ", x)
    }
    fmt.Println("------")
    x, ok = <-ch
    if !ok {
        fmt.Println("received (not ok): ", x)
        fmt.Println("channel closed, data invalid.")
    }
}
```

返回结果：

```text
received:  18
------
received (not ok):  0
channel closed, data invalid.
```

### 关闭一个已关闭的Channel会？

Panic。

### 关闭一个nil channel会？

Panic。

