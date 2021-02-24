---
description: 有很多 TIME-WAIT 状态如何解决？
---

# 有很多 TIME-WAIT 状态如何解决？

## 解决方案

修改配置或设置SO\_REUSEADDR套接字，使得服务器处于TIME-WAIT状态下的端口能够快速回收和重用。SO\_REUSEADDR允许同一端口上启动同一服务器的多个实例（进程）。但每个实例绑定的IP地址不能相同。

