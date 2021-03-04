---
description: fork()是什么？
---

# 什么是fork函数？

## Fork <a id="firstHeading"></a>

### 定义

系统调用`fork()`用于创建进程。这个函数没有参数，返回进程ID。创建的进程是调用者的子进程，当这个子进程被创建之后，两个进程都会执行`fork()`的下一条指令。通过`fork()`的返回值，可以分辨出父进程与子进程。

* 返回值为负数表示子进程创建失败；
* 返回值为0表示当前是被创建出来的子进程；
* 返回值为正数的话，这个数是子进程的PID。

子进程会拷贝父进程的地址空间，即两个进程拥有着独立的地址空间。

### 举例

#### 例一

```cpp
#include  <stdio.h>
#include  <string.h>
#include  <sys/types.h>

#define   MAX_COUNT  200
#define   BUF_SIZE   100

void  main(void)
{
     pid_t  pid;
     int    i;
     char   buf[BUF_SIZE];

     fork();
     pid = getpid();
     for (i = 1; i <= MAX_COUNT; i++) {
          sprintf(buf, "This line is from pid %d, value = %d\n", pid, i);
          write(1, buf, strlen(buf));
     } 
}
```

![](../.gitbook/assets/image%20%2860%29.png)

如果父进程执行成功，Unix会：

* 为子进程拷贝一份一样的地址空间；
* 两个进程各自从`fork()`的下一条语句开始执行。

![](../.gitbook/assets/image%20%2859%29.png)

因为两进程独立，所以一个进程内部变量的改变不影响另一个，即使变量名相同。

```text
     ................
This line is from pid 3456, value 13
This line is from pid 3456, value 14
     ................
This line is from pid 3456, value 20
This line is from pid 4617, value 100
This line is from pid 4617, value 101
     ................
This line is from pid 3456, value 21
This line is from pid 3456, value 22
     ................
```

#### 例二

```cpp
#include  <stdio.h>
#include  <sys/types.h>

#define   MAX_COUNT  200

void  ChildProcess(void);                /* child process prototype  */
void  ParentProcess(void);               /* parent process prototype */

void  main(void)
{
     pid_t  pid;
     pid = fork();
     if (pid == 0) 
          ChildProcess();
     else 
          ParentProcess();
}

void  ChildProcess(void)
{
     for (int i = 1; i <= MAX_COUNT; i++)
          printf("   This line is from child, value = %d\n", i);
     printf("   *** Child process is done ***\n");
}

void  ParentProcess(void)
{
     for (int i = 1; i <= MAX_COUNT; i++)
          printf("This line is from parent, value = %d\n", i);
     printf("*** Parent is done ***\n");
}
```

![](../.gitbook/assets/image%20%2856%29.png)

调用`fork()`之后，其返回值`pid`在父进程是子进程的PID，在子进程是0。因此按之后的逻辑，父进程将执行`ParentProcess()`而子进程去执行`ChildProcess()`。

![](../.gitbook/assets/image%20%2858%29.png)

## Reference

1. [http://www.csl.mtu.edu/cs4411.ck/www/NOTES/process/fork/create.html](http://www.csl.mtu.edu/cs4411.ck/www/NOTES/process/fork/create.html)
2. [https://en.wikipedia.org/wiki/Fork\_\(system\_call\)](https://en.wikipedia.org/wiki/Fork_%28system_call%29)

