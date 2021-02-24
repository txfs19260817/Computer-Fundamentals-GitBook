---
description: 在浏览器地址栏输入一个URL后回车，背后会进行哪些技术步骤？
---

# What happens when you type in a URL and press enter in browser?

## 极简版答案

![](../.gitbook/assets/image%20%281%29.png)

如下是一个超简化版HTTP请求过程。

1. 浏览器检查缓存，如果请求对象存在且新，跳到\#9（解码渲染部分）。
2. 浏览器向操作系统请求服务器IP地址。
3. 操作系统进行DNS查询并将IP地址返回给浏览器（浏览器缓存→host→本地DNS&gt;根&gt;权威）。
4. 浏览器开启与服务器的TCP连接（三次握手）。
5. 浏览器通过TCP连接向服务器发送HTTP请求。
6. 浏览器接收到HTTP响应后，TCP连接可能关闭，也可能留给其他请求。
7. 浏览器检查状态码，如果是常见的200则继续。如果是3XX的重定向，或者是4XX或5XX的客户端/浏览器错误，则有不一样的处理方式。
8. 如果可以，缓存响应。
9. 浏览器解码响应（例如响应体是经过gzip压缩的）
10. 浏览器依据响应类别（HTML文件或是图像等）决定如何处理响应。
11. 浏览器渲染页面。如果是无法识别的类别则弹出下载对话框。
12. 浏览器断开与服务器的TCP连接（四次挥手）。

## Reference

1. [https://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser](https://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser)
2. [https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)
3. [https://www.zhihu.com/question/34873227](https://www.zhihu.com/question/34873227)



