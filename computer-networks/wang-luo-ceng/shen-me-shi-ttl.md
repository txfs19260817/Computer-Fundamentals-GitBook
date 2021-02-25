---
description: 什么是TTL?
---

# 什么是TTL?

## TTL\(**Time To Live\)**

TTL 是指报文生存时间，它表示了数据包在网络中的时间。每经过一个路由器后 TTL 就减1，这样 TTL 最终会减为 0 ，当 TTL 为 0 时，则将数据包丢弃。通过设置 TTL 可以避免这两个路由器之间形成环导致数据包在环路上死转的情况，由于有了 TTL ，当 TTL 为 0 时，数据包就会被抛弃。

按照规范TTL是时间，每经过一个中继设备，有IP包入口（Incoming）的时间戳，还有出口（Outgoing\)的时间戳，那么这两个时间戳的差值为 delta = Outgoing timestamp - Incoming timestamp。然后中继设备更新 TTL = TTL - delta。现在跳数的实现方案是为了简化网络层的实现。

## Reference

1. [https://www.zhihu.com/question/61007907](https://www.zhihu.com/question/61007907)

