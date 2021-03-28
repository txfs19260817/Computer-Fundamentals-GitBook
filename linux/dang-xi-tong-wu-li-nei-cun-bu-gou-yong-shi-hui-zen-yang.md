---
description: 如何理解Linux中的OOM(Out Of Memory Killer)机制？
---

# 当系统物理内存不够用时会怎样？

## Out Of Memory Killer

### 简介

这是一个避免Out Of Memory \(OOM\) 情况出现的管理器。内存不足（即使算上swap）时，内核会检查是否真正陷入Out Of Memory \(OOM\)，如果是则选择杀掉一个进程。

### Memory Overcommit <a id="Memory-Overcommit"></a>

![](../.gitbook/assets/image%20%2881%29.png)

一个进程会申请一大块虚拟内存，但仅用到其中一小部分，对应一块虚拟页面。当程序首次访问这个页面时，会触发一个缺页异常 \(page fault\)。这时内核会分配一个物理页面，让虚拟页面映射到这个物理页面，同时更新进程的页表 \(page table\)。

Memory Overcommit指的是分配的虚拟内存超过物理内存与交换空间的总和。Linux 默认允许 Memory Overcommit，因为虽然申请了很多内存但实际使用往往并不多。但在很多进程大量占用内存时，为避免OOM，OOM Killer会出来杀进程。

## Reference

1. [https://www.kernel.org/doc/gorman/html/understand/understand016.html](https://www.kernel.org/doc/gorman/html/understand/understand016.html)
2. [http://senlinzhan.github.io/2017/07/03/oom-killer/](http://senlinzhan.github.io/2017/07/03/oom-killer/)

