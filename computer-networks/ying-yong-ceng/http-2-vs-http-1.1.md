---
description: HTTP/1.X和HTTP/2.0的区别？
---

# HTTP/2 VS HTTP/1.1?

## HTTP/1.X和HTTP/2.0对比

| \ | HTTP/1.X | HTTP/2.0 |
| :--- | :--- | :--- |
| 数据传输格式 | Plain Text | Binary |
| 多路复用 | 逐个发送响应 | 一次性发送多个响应 |
| 服务器推送 | 无 | 有 |
| 头部压缩 | 无 | 有，例如HPACK |

## 多路复用

**\[TL; DR\] 一次多发、无阻碍、可加权、缺失可单独重发**

HTTP/1.1 依次加载各个资源，若无法加载某一资源，它将阻碍其后的所有资源。相比之下，HTTP/2 可以使用单个 TCP 连接来一次发送多个数据流，使得任何资源都不会阻碍其他资源。为此，HTTP/2 将数据拆分为二进制代码消息并为这些消息编号，以便客户端知道每个二进制消息所属的流。

HTTP/2 提供了一个称为加权优先顺序的功能。开发人员可以为这些数据流分别分配一个不同的加权值，该值告诉客户端首先要呈现哪个数据流。如果客户端缺失某些部分可以单独向服务器请求它们。

## Reference

1. [https://www.cloudflare.com/zh-cn/learning/performance/http2-vs-http1.1/](https://www.cloudflare.com/zh-cn/learning/performance/http2-vs-http1.1/)
2. [https://zh.wikipedia.org/wiki/HTTP/2](https://zh.wikipedia.org/wiki/HTTP/2)

