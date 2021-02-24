---
description: TCP半连接队列和全连接队列是什么？
---

# TCP半连接队列和全连接队列是什么？

## TCP半连接队列与全连接队列

![](../../.gitbook/assets/image%20%283%29.png)

在 TCP 三次握手的时候，Linux 内核会维护两个队列，分别是：

* 半连接队列，也称 SYN 队列；
* 全连接队列，也称 accept 队列。

服务端收到客户端发起的 SYN 请求后，内核会把该连接存储到半连接队列，并向客户端响应 SYN+ACK，接着客户端会返回 ACK，服务端收到第三次握手的 ACK 后，内核会把连接从半连接队列移除，然后创建新的完全的连接，并将其添加到 accept 队列，等待进程调用 accept\(\) 函数时把连接取出来。

二者都有最大长度限制。超过限制时，内核会直接丢弃（默认），或返回 RST 包。

## Reference

1. [https://www.cnblogs.com/xiaolincoding/p/12995358.html](https://www.cnblogs.com/xiaolincoding/p/12995358.html)

