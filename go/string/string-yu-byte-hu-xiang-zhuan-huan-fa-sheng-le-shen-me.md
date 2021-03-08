---
description: 'String与[]byte互相转换的过程发生了什么？'
---

# String与\[\]byte互相转换发生了什么？

## String与\[\]byte互转

![](../../.gitbook/assets/image%20%2868%29.png)

以`[]byte`转换成`string`为例：

1. 先在堆或者栈上分配内存，地址为`p`；
2. 这段内存拷贝到`p`的位置；
3. 将变量的类型转换成 `string` ；

反过来的话，在拷贝之前先构造结构体`stringStruct{str: p, len: len}`再拷贝。

内存拷贝存在开销，这可能会成为高频场景中的性能瓶颈。

互转过程中并不总是存在拷贝，在一些场景下有优化。例如switch语句，可以在汇编中直接比较`string`与`[]byte`指向的数据部分。

## Reference

1. [https://medium.com/a-journey-with-go/go-string-conversion-optimization-767b019b75ef](https://medium.com/a-journey-with-go/go-string-conversion-optimization-767b019b75ef)
2. [https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-string/](https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-string/)

