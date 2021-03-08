---
description: iota的实质是什么？
---

# iota的实质是什么？

## iota

* iota代表了const声明块的行索引，从0开始。

举例：

```go
const (
	bit0, mask0 = 1 << iota, 1<<iota - 1 // 1, 0
	bit1, mask1                          // 2, 1
	_, _
	bit3, mask3                          // 8, 7
)
```

每一行iota分别等于0、1、2、3，因此不难得出结果。

