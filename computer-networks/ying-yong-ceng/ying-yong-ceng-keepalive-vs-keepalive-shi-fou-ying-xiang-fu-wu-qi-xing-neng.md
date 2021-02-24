---
description: 对比Keep-Alive和非Keep-Alive，是否影响服务器性能？
---

# Keep-Alive VS 非Keep-Alive，是否影响服务器性能？

## Keep-Alive和非Keep-Alive对比

![](../../.gitbook/assets/image%20%2817%29.png)

非Keep-Alive：浏览器每次发起HTTP请求都要与服务器创建一个新的TCP连接，服务器完成请求处理后立即断开TCP连接，服务器不跟踪每个客户也不记录过去的请求。

Keep-Alive：客户端对服务器有继请求时，Keep-Alive指示还用这个请求继续交流，避免关闭与重新建立连接。

HTTP/1.0默认是非Keep-Alive，需要在HTTP头部设置”Connection: Keep-Alive”，才能启用Keep-Alive。HTTP/1.1 版本中默认使用Keep-Alive。

Keep-Alive也指长连接。

## 服务器性能影响

对非Keep-Alive来说，每次建立连接都需要客户端和服务器分配TCP的缓冲区和变量，如果服务器同时与大量客户端建立连接的话开销很大。

对Keep-Alive来说，长时间保持TCP连接容易导致系统资源被无效占用。以下措施可以合理关闭TCP连接以节省资源：

1. Content-Length：表示实体内容的长度。浏览器通过这个字段来判断当前请求的数据是否已经全部接收。例如，当浏览器请求的是一个静态资源时，即服务器能明确知道返回内容的长度时，可以设置Content-Length来控制请求的结束。
2. Transfer-Encoding：当服务端无法知道实体内容的长度时，就可以通过指定"Transfer-Encoding: chunked"来告知浏览器当前的编码是分块传递的。当浏览器接收到一个长度为0的chunked时， 知道当前请求内容已全部接收。
3. Keep-Alive timeout：当 TCP 连接在传送完最后一个HTTP响应，该连接会保持timeout秒，之间若没有请求就开始关闭这个链接。

## Reference

1. [https://blog.csdn.net/weixin\_37672169/article/details/80283935](https://blog.csdn.net/weixin_37672169/article/details/80283935)



