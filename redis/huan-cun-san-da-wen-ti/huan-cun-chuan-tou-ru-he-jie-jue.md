---
description: 缓存穿透是什么？如何解决？
---

# 缓存穿透如何解决？

## 缓存穿透

### 问题

![](../../.gitbook/assets/image%20%2855%29.png)

查询一条数据库和缓存都没有的数据，就会一直查询数据库，增大数据库的访问压力。

### 解决方法

#### 缓存空对象

设置对应键为空值进行缓存，并设置较短过期时间。

缺点：

1. 消耗内存，如果是被攻击的话可能会OOM；
2. 数据不一致：如果在过期时间内数据库有了该数据，而Redis直接返回了空。

#### 布隆过滤器

如果不在布隆过滤器中就直接返回false，否则依次执行查缓存和DB的步骤。

优点：速度快，省空间。

缺点：

1. 因为碰撞，会有一定的误报率，且随数据量的上升而上涨；
2. 不支持删除操作。

## Reference

1. [https://blog.csdn.net/qq\_43255017/article/details/108396782](https://blog.csdn.net/qq_43255017/article/details/108396782)
2. [https://zhuanlan.zhihu.com/p/353644479](https://zhuanlan.zhihu.com/p/353644479)
3. [https://blog.nekolr.com/2019/08/30/布隆过滤器/](https://blog.nekolr.com/2019/08/30/布隆过滤器/)



