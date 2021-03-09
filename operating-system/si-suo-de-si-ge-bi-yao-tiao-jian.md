---
description: 死锁的四个必要条件是什么？
---

# 死锁的四个必要条件？

## 死锁的四个必要条件

![](../.gitbook/assets/image%20%2877%29.png)

* 互斥：某资源仅为一个线程所占有，每个资源要么已经分配给了一个进程，要么就是可用的；
* 占有并等待：线程持有了一个资源，同时又在等待其他资源；
* 不可抢占：已分配给一个线程的资源不能强制性地被剥夺，它只能被占有它的线程显式地释放。
* 循环等待：有两个或者两个以上的进程组成一条环路，该环路中的每个进程都在等待下一个进程所占有的资源。

## Reference

1. [https://blog.csdn.net/wljliujuan/article/details/79614019](https://blog.csdn.net/wljliujuan/article/details/79614019)
2. [http://www.cyc2018.xyz/计算机基础/操作系统基础/计算机操作系统-死锁.html](http://www.cyc2018.xyz/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%9F%BA%E7%A1%80/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%9F%BA%E7%A1%80/%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%20-%20%E6%AD%BB%E9%94%81.html)

