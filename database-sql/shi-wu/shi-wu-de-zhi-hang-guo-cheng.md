---
description: 事务的执行过程？
---

# 事务的执行过程？

## 事务执行

![](../../.gitbook/assets/image%20%2857%29.png)

1. SQL语句经过语法分析和优化，生成执行计划并提交；
2. 从磁盘的数据页获取数据到Buffer Pool，可能依靠索引；
3. 将修改前的记录写入undoLog；
4. 执行修改后先写入redoLog；
5. 再将修改结果写在Buffer Pool的页上；
6. 通知执行器事务可以提交；
7. 事务发起提交请求；
8. 将事务提交涉及的redoLog刷盘；
9. 刷盘成功后将SQL级别数据录入binLog；

## Reference

1.  ****[一个线上SQL死锁异常分析：深入了解事务和锁](https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247502033&idx=1&sn=897bcc7790a004c48a61c24053059d39&chksm=e92af5dede5d7cc81bdf61641f8aa89f80d6f21351343a0e3d38ff249421f79eadf946e7f84c&scene=27#wechat_redirect)

