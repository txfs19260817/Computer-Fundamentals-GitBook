---
description: 空slice和nil slice的区别在哪里？
---

# 空slice和nil slice的区别？

## 空slice与nil slice的声明

如果显式声明一个slice的变量，那该变量就是nil slice。

```go
var a []string
fmt.Println(a == nil) // true
```

如果是用声明与赋值语句创建的slice变量，那该变量就是空slice。

```go
b := []string{}
fmt.Println(b == nil) // false
c := make([]string, 0)
fmt.Println(c == nil) // false
```

## 使用上的相同

不管是使用 nil 切片还是空切片，对其调用内置函数 append，len 和 cap 的效果都是一样的。

顺带一提对nil map赋值会导致panic。

## 使用上的不同

json.Marshal一个空slice会编码成空array`[]`，Marshal一个nil slice会编码成`null`。

```go
var a []string // a == nil
aj, _ := json.Marshal(&A{Data: a})
fmt.Printf("%s\n", string(aj)) // {"Data":null}

b := []string{} // b != nil
bj, _ := json.Marshal(&A{Data: b})
fmt.Printf("%s\n", string(bj)) // {"Data":[]}
```

另外还有一条官方的编码风格建议，返回值判断slice是否为空（指不含元素）时，不建议用slice直接与nil进行比较，而是检查长度是否为0。

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Bad</b>
      </th>
      <th style="text-align:left"><b>Good</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>func isEmpty(s []string) bool {</code>
        </p>
        <p><code>  return s == nil</code>
        </p>
        <p><code>}</code>
        </p>
      </td>
      <td style="text-align:left">
        <p><code>func isEmpty(s []string) bool {</code>
        </p>
        <p><code>  return len(s) == 0</code>
        </p>
        <p><code>}</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

## 底层实现的差异

空slice和nil slice的底层实现差异在于，空切片的Array Pointer指向的地址不是nil，指向的是一个内存地址，但是它没有分配任何内存空间，即底层元素包含0个元素；而nil slice的Array Pointer指向 nil。

![nil slice - by @halfrost](../../.gitbook/assets/image%20%2866%29.png)

![empty slice - by @halfrost](../../.gitbook/assets/image%20%2862%29.png)

## Reference

1. [深入解析 Go 中 Slice 底层实现](https://halfrost.com/go_slice/#toc-5)
2. [Empty slice vs nil slice in GoLang](https://www.pixelstech.net/article/1539870875-Empty-slice-vs-nil-slice-in-GoLang)
3. [Golangのnil sliceとnil map](https://tutuz-tech.hatenablog.com/entry/2019/10/20/145302)

