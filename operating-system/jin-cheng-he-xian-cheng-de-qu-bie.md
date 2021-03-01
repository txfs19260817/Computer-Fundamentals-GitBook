---
description: 进程、线程、文件描述符是什么？
---

# 进程和线程的区别？

## 进程

进程是资源分配的基本单位。

### 进程控制块

进程控制块 \(Process Control Block, PCB\) 描述进程的基本信息和运行状态，所谓的创建进程和撤销进程，都是指对 PCB 的操作。

![](../.gitbook/assets/image%20%2849%29.png)

### 进程状态的切换

![](../.gitbook/assets/image%20%2851%29.png)

## 线程

线程是独立调度的基本单位。一个进程中可以有多个线程，它们共享进程资源。

## 异同

### 不同点

| \ | 进程 | 线程 |
| :--- | :--- | :--- |
| 拥有资源 | 拥有资源，分配资源的基本单位 | 不拥有资源，可访问隶属进程的资源 |
| 调度 | 从一个进程中的线程切换到另一个进程中的线程时，会引起进程切换 | 调度的基本单位 |
| 系统开销 | 大，如内存空间、I/O 设备等 | 小，保存和设置少量寄存器内容 |
| 通信方面 | 进程通信需要借助 IPC（管道、共享内存MMAP、本地socket等） | 线程间可以通过直接读写同一进程中的数据进行通信 |

### 相同点

![](../.gitbook/assets/image%20%2850%29.png)

都是同一个结构体。多进程与多线程唯一的区别就是**共享的数据区域不同**。父子进程共享内存时，Linux 采用了 copy-on-write 的策略优化，也就是并不真正复制父进程的内存空间，而是等到需要写操作时才去复制。

```c
struct task_struct {
	// 进程状态
	long			  state;
	// 虚拟内存结构体
	struct mm_struct  *mm;
	// 进程号
	pid_t			  pid;
	// 指向父进程的指针
	struct task_struct __rcu  *parent;
	// 子进程列表
	struct list_head		children;
	// 存放文件系统信息的指针
	struct fs_struct		*fs;
	// 一个数组，包含该进程打开的文件指针
	struct files_struct		*files;
};
```

## 文件描述符

![](../.gitbook/assets/image%20%2852%29.png)

 `files`的前三位被填入默认值，分别指向标准输入流、标准输出流、标准错误流。 如要打开一个文件进行读写，则进行系统调用，让内核把文件打开，这个文件就会被放到`files[3]`即第 4 个位置。

## Reference

1. [https://github.com/labuladong/fucking-algorithm/blob/master/技术/linux进程.md](https://github.com/labuladong/fucking-algorithm/blob/master/技术/linux进程.md)
2. [http://www.cyc2018.xyz/计算机基础/操作系统基础/计算机操作系统](http://www.cyc2018.xyz/计算机基础/操作系统基础/计算机操作系统)

