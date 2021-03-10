---
description: 为什么区分内核态和用户态？二者如何互相切换？
---

# 为什么区分内核态和用户态？

## 内核态和用户态

### 定义

![](../.gitbook/assets/image%20%2880%29.png)

用户态和内核态，虽然是操作系统概念，但本质是对CPU提供的功能的一层封装抽象。CPU 将指令分为特权指令和非特权指令，内核态就对应到CPU的特权模式，用户态对应非特权模式。

例如X86架构的特权级别有四级，从高到低是Ring0到Ring3，用户态对应Ring3而内核态对应Ring0。Ring3 状态不能访问Ring0的地址空间，包括代码和数据。

处于内核态的操作系统内核对于CPU、内存等硬件有不受限制的使用权限，并且可以执行任何 CPU 指令以及访问任意的内存地址。用户态的应用程序相对来说就是受限的，若用户的应用程序要访问内核管理的资源（CPU、内存、I/O），必须通过内核提供的系统调用**。**

因此区分内核态和用户态的目的是**要限制应用程序的行为，因此在应用程序和操作系统执行时有不同的状态**，核心问题在于保护关键寄存器和重要的物理内存。

### **用户态切换到内核态的三种方式**

1. 系统调用：用户态进程通过系统调用申请使用操作系统提供的服务程序完成工作；
2. 异常：如果当前进程运行在用户态，如果这个时候发生了异常事件就会触发切换，例如缺页异常；
3. 外设中断：当外设完成用户的请求时，会向CPU发送中断信号。

## Reference

1. [https://leetcode-cn.com/circle/discuss/zIxrWn/](https://leetcode-cn.com/circle/discuss/zIxrWn/)
2. [https://www.zhihu.com/question/306127044](https://www.zhihu.com/question/306127044)
3. [https://www.zhihu.com/question/397142622](https://www.zhihu.com/question/397142622)
4. [https://zhuanlan.zhihu.com/p/69554144](https://zhuanlan.zhihu.com/p/69554144)
5. [https://www.v2ex.com/t/504408](https://www.v2ex.com/t/504408)
6. [https://zh.wikipedia.org/wiki/%E5%88%86%E7%BA%A7%E4%BF%9D%E6%8A%A4%E5%9F%9F](https://zh.wikipedia.org/wiki/%E5%88%86%E7%BA%A7%E4%BF%9D%E6%8A%A4%E5%9F%9F)

