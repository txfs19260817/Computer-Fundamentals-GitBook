---
description: slice和array二者有什么关系或异同？
---

# slice和array二者有什么关系或异同？

## Array

Go数组是定长的且不可扩容。数组在Go是值类型，说明：

1. 赋值和函数传参操作都会复制整个数组数据。
2. 数组的长度是类型的一部分，例如\[3\]int和\[5\]int不是同一类型，即使数据类型一致。

## Slice <a id="slice"></a>

slice可以看作是可变长数组。实际上是一个结构体，包含三个字段：长度、容量、指向底层array的指针。因此多个slice的底层是同一数组是可能的。

```text
// runtime/slice.gotype slice struct {    array unsafe.Pointer // 元素指针    len   int // 长度     cap   int // 容量}
```

![&#x5207;&#x7247;&#x6570;&#x636E;&#x7ED3;&#x6784; - by @halfrost](../../.gitbook/assets/image%20%2864%29.png)

### slice的初始化

用make函数初始化slice的过程如下：先判断len与cap是否合法，然后malloc申请内存，最后返回构造好的slice结构体。

```go
func makeslice(et *_type, len, cap int) slice {
	// 根据切片的数据类型，获取切片的最大容量
	maxElements := maxSliceCap(et.size)
    // 比较切片的长度，长度值域应该在[0,maxElements]之间
	if len < 0 || uintptr(len) > maxElements {
		panic(errorString("makeslice: len out of range"))
	}
    // 比较切片的容量，容量值域应该在[len,maxElements]之间
	if cap < len || uintptr(cap) > maxElements {
		panic(errorString("makeslice: cap out of range"))
	}
    // 根据切片的容量申请内存
	p := mallocgc(et.size*uintptr(cap), et, true)
    // 返回申请好内存的切片的首地址
	return slice{p, len, cap}
}
```

### slice扩容策略

1. 如果期望容量大于当前容量的两倍就会使用期望容量；
2. 如果当前切片的长度小于1024 就会将容量翻倍；
3. 如果当前切片的长度大于1024 就会每次增加 25% 的容量，直到新容量大于期望容量。

### 扩容后的slice是一个新的、与扩容前不同的slice吗？

不一定。

新建空间的例子：如果向slice添加新元素时cap已满，会按照扩容策略另起一个新空间并把旧值复制过去，再完成append。

```go
func main() {
	slice := []int{10, 20, 30, 40}
	newSlice := append(slice, 50)
	fmt.Printf("Before slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("Before newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
	newSlice[1] += 10
	fmt.Printf("After slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("After newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
}
```

结果：

```text
Before slice = [10 20 30 40], Pointer = 0xc4200b0140, len = 4, cap = 4
Before newSlice = [10 20 30 40 50], Pointer = 0xc4200b0180, len = 5, cap = 8
After slice = [10 20 30 40], Pointer = 0xc4200b0140, len = 4, cap = 4
After newSlice = [10 30 30 40 50], Pointer = 0xc4200b0180, len = 5, cap = 8
```

图示：

![&#x6269;&#x5BB9;&#x5F97;&#x5230;&#x65B0;slice - by @halfrost](../../.gitbook/assets/image%20%2867%29.png)

反例是如果声明一个截取了旧slice的一段而成的新slice，此时未被截取的后部剩余部分变得可修改，执行append一个元素时，会直接修改后部剩余部分的第一个值。

```go
func main() {
	array := [4]int{10, 20, 30, 40}
	slice := array[0:2]
	newSlice := append(slice, 50)
	fmt.Printf("Before slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("Before newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
	newSlice[1] += 10
	fmt.Printf("After slice = %v, Pointer = %p, len = %d, cap = %d\n", slice, &slice, len(slice), cap(slice))
	fmt.Printf("After newSlice = %v, Pointer = %p, len = %d, cap = %d\n", newSlice, &newSlice, len(newSlice), cap(newSlice))
	fmt.Printf("After array = %v\n", array)
}
```

结果：

```text
Before slice = [10 20], Pointer = 0xc4200c0040, len = 2, cap = 4
Before newSlice = [10 20 50], Pointer = 0xc4200c0060, len = 3, cap = 4
After slice = [10 30], Pointer = 0xc4200c0040, len = 2, cap = 4
After newSlice = [10 30 50], Pointer = 0xc4200c0060, len = 3, cap = 4
After array = [10 30 50 40]
```

图示：

![&#x5728;&#x539F;&#x5148;slice&#x57FA;&#x7840;&#x4E0A;&#x6269;&#x5BB9; - by @halfrost](../../.gitbook/assets/image%20%2861%29.png)

## Trivia

### 与Redis SDS对比

| item\type | Go Slice | Redis SDS |
| :--- | :--- | :--- |
| structure | array pointer, len, cap | char array, len, free |
| 扩容策略 | 指定-&gt;长度小于1024则2倍-&gt;大于则增加25% | 小于1MB翻倍-&gt;大于1MB则增加1MB |

## Reference

1. [深入解析 Go 中 Slice 底层实现](https://halfrost.com/go_slice/#toc-5)

