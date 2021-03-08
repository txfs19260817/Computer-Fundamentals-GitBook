---
description: String数据结构是什么？
---

# String数据结构？

## 数据结构

```go
type StringHeader struct {
	  Data uintptr // 字符串首地址
	  Len  int     // 长度
}
```

 与切片的结构体相比，字符串只少了一个表示容量的 `Cap` 字段。

String的构造过程：

```go
func gostringnocopy(str *byte) string {
    ss := stringStruct{str: unsafe.Pointer(str), len: findnull(str)}
    s := *(*string)(unsafe.Pointer(&ss))
    return s
}
```

## Reference

1. [https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-string](https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-string)

