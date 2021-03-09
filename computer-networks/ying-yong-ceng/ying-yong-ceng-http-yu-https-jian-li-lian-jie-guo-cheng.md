---
description: HTTP 与 HTTPs 的工作方式/建立连接过程分别是什么？
---

# HTTP与HTTPs建立连接过程？

## HTTP的建立连接过程

![](../../.gitbook/assets/image%20%282%29.png)

客户端向服务器发起一个TCP连接（80端口）。连接建立后，浏览器和服务器进程就可以通过套接字接口访问TCP。客户端从套接字接口发送HTTP请求报文和接收HTTP响应报文。类似地，服务器也是从套接字接口收发HTTP报文。其通信内容以明文的方式发送，不通过任何方式的数据加密。当通信结束时，客户端与服务器关闭连接。

## HTTPs的建立连接过程

![](../../.gitbook/assets/image%20%284%29.png)

1. \[Client Hello\] 客户端向服务器发起一个TCP连接（443端口），发送的信息主要包括自身所支持的TLS版本和加密算法列表（cipher suite），以及称为“客户端随机数”的一串随机字节等；
2. \[Server Hello\] 服务器选择一种支持的SSL版本和加密算法，并将“服务器随机数”发送给客户端；
3. \[Certificate\] 服务器向客户端发送一个包含数字证书的报文，该数字证书中包含证书的颁发机构、过期时间等信息，证书还附上了服务端的公钥；
4. \[Server Hello Done\] 最后服务端发送一个完成报文通知客户端SSL的第一阶段已经协商完成。
5. \[Client Key Exchange\] SSL第一次协商完成后，客户端发送一个回应报文，其中包含一个客户端生成的并经服务器公钥加密过的随机密码串，称为pre-master secret；
6. \[Change Cipher Spec\] 客户端发送一个报文提示服务器此后的报文采用pre-master secret加密；
7. \[Finished\] 客户端发送一个finish报文，包含自第一次握手至今所有报文的整体校验值，最终协商是否完成取决于服务端能否成功解密；
8. \[Change Cipher Spec\] 服务器同样发送Change Cipher Spec报文，向客户端确认；
9. \[Finished\] 服务器同样发送finish报文，告诉客户端自己能正确解密报文。至此SSL连接建立完成，此时双方已经拥有根据pre-master secret和客户端与服务器的随机数计算得到的master secret；
10. \[Request\] 客户端发送master secret加密的HTTP请求；
11. \[Response\] 服务器返回master secret加密的HTTP响应；
12. \[Close Notify\] 客户端断开连接时，发送 close\_notify 报文。这步之后再发送TCP FIN报文来关闭TCP连接。

![](../../.gitbook/assets/image%20%2823%29.png)

## Reference

1. 《图解HTTP》
2. 《HTTP权威指南》
3. [https://www.cloudflare.com/zh-cn/learning/ssl/what-happens-in-a-tls-handshake/](https://www.cloudflare.com/zh-cn/learning/ssl/what-happens-in-a-tls-handshake/)
4. [https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd](https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd)

