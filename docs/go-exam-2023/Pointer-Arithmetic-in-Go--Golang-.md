<!--yml

分类：未分类

日期：2024-10-13 06:29:00

-->

# Go语言中的指针算术运算

> 来源：[https://golangbyexample.com/pointer-arithmetic-golang/](https://golangbyexample.com/pointer-arithmetic-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

与C语言不同，Golang中不支持指针算术运算，这会引发编译错误。

# **程序**

```
package main
func main() {
    a := 1
    b := &a
    b = b + 1
}
```

**输出**

上述程序引发编译错误

```
invalid operation: b + 1 (mismatched types *int and int)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
