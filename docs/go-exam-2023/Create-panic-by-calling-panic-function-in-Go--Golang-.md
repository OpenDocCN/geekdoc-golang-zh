<!--yml

类别：未分类

日期：2024-10-13 06:27:34

-->

# 通过在 Go (Golang) 中调用恐慌函数创建恐慌

> 来源：[https://golangbyexample.com/create-panic-golang/](https://golangbyexample.com/create-panic-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

Go 提供了一个特殊函数来创建恐慌。下面是该函数的语法

```go
func panic(v interface{})
```

此函数可以由程序员显式调用以创建恐慌。它接受一个空接口作为参数。

在程序员可以显式调用恐慌函数的一些情况包括：

+   函数期望一个有效的参数，但却提供了一个 nil 参数。在这种情况下，程序无法继续，并且会因传入 nil 参数而引发恐慌。

+   程序无法继续的任何其他情况。

# 示例

让我们看一个例子

```go
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	checkAndPrint(a, 2)
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		panic("Out of bound access for slice")
	}
	fmt.Println(a[index])
}
```

**输出**

```go
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(0xc00009af58, 0x2, 0x2, 0x2)
        main.go:15 +0x31
main.main()
        main.go:8 +0x7d
exit status 2
```

在上面的程序中，我们再次有一个函数 **checkAndPrint**，它接受一个切片作为参数和一个索引。然后它检查传入的索引是否大于切片长度减去1。如果是，那么它就是超出切片边界的访问，因此会引发恐慌。如果不是，则打印该索引处的值。再注意输出中有两件事

+   错误信息

+   发生恐慌时的堆栈跟踪

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
