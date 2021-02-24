---
description: HTTPS 和 HTTP 的区别？
---

# HTTPS VS HTTP？

## HTTPS 和 HTTP对比

| \ | HTTP | HTTPS |
| :--- | :--- | :--- |
| 内容加密 | 无，明文传输 | 对称密钥加密 |
| 证书认证 | 无 | 根据CA的证书验证实体 |
| 防篡改 | 无 | 可根据hash校验内容是否被篡改 |
| 端口 | 80 | 443 |
| 速度 | 更快 | 更慢，因为有SSL/TLS协商过程 |

## Reference

1. [https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd](https://dev.to/techschoolguru/a-complete-overview-of-ssl-tls-and-its-cryptographic-system-36pd)

