---
description: 为什么要四次挥手？
---

# 为什么要四次挥手？

## 被动方ACK和FIN分两次的意义

当主动方在数据传送结束后发出连接释放的通知，由于被动方可能还有必要的数据要处理，所以会**先返回 ACK 确认收到主动方的 FIN 报文**。当被动方也没有数据再发送的时候，**再发出连接释放通知**，对方确认后才完全关闭TCP连接。

## Reference

1. [https://www.zhihu.com/question/63264012](https://www.zhihu.com/question/63264012)

