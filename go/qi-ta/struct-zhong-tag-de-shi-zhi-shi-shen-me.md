---
description: Struct中Tag的实质是什么？
---

# Struct中Tag的实质是什么？

## Tag

* Tag是Struct的一部分；
* Tag实质是String。

在`reflect`包中，`StructField`表示结构体的一个字段：

```go
type StructField struct {
    Name      string    // 字段名
    PkgPath   string

    Type      Type      // 字段类型
    Tag       StructTag // Tag 字符串
    Offset    uintptr   // offset within struct, in bytes
    Index     []int     // index sequence for Type.FieldByIndex
    Anonymous bool      // is an embedded field
}
```

`StructTag`实际上是string类型的别名：

```go
type StructTag string
```

Tag字符串必须按照`key:"value"`的约定编写，且键值对之间使用空格隔开。

## Reference

1. [https://golang.org/pkg/reflect/\#StructField](https://golang.org/pkg/reflect/#StructField)

