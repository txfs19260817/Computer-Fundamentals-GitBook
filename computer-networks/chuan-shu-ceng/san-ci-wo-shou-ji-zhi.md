---
description: TCP建立连接的三次握手流程是什么?
---

# 三次握手机制?

## TCP三次握手建立连接

![](../../.gitbook/assets/image%20%285%29.png)

1. 客户端发起连接，向服务器的指定端口发送一个SYN包，其中：
   * 标志位的SYN置1；
   * 初始化序列号Seq = x；
   * 随后客户端进入SYN-SEND状态。
2. 服务器对该包进行确认后，结束LISTEN阶段，发送一个SYN包作为应答，其中：
   * 服务器分配该连接的缓存和变量；
   * 标志位的SYN和ACK置1，表示确认客户端的报文Seq序号有效；
   * 初始化序列号Seq = y；
   * 设置确认号Ack = x + 1，表示收到客户端的序号Seq并将其值加 1 作为自己确认号 Ack 的值。一个SYN占用一个字节（序号）；
   * 随后服务器端进入SYN-RCVD阶段。
3. 客户端收到后，明确了二者间数据传输正常，结束SYN-SEND阶段，并返回确认包：
   * 客户端分配该连接的缓存和变量；
   * 标志位的ACK置1，表示确认收到服务器端同意连接的信号；
   * 设置序列号Seq = x + 1，表示上一个包成功的发出去了；
   * 设置确认号Ack = y + 1，表示收到服务器端序号Seq；
   * 随后客户端进入 ESTABLISHED。

当服务器端收到来自客户端确认收到服务器数据的报文后，得知从服务器到客户端的数据传输是正常的，从而结束SYN-RCVD阶段，进入ESTABLISHED阶段，从而完成三次握手。

