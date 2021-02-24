---
description: CA怎么签发和验证证书？
---

# CA怎么去验证证书？

## CA签发证书（以服务器为例）

![](../../.gitbook/assets/image%20%287%29.png)

1. 服务器创建CSR，其中包含实体信息和服务器公钥，并用服务器私钥签署后，发送给CA；
2. CA先验证个人信息，再用附带的服务器公钥验证签名，以确保服务器真正拥有配对的私钥；
3. CA用CA私钥签署证书，并发还给服务器。

## CA证书验证

客户端得到服务器的证书后，用CA的公钥验证即可。

## Reference

1. [https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd](https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd)
2. [https://www.zhihu.com/question/37370216](https://www.zhihu.com/question/37370216)

