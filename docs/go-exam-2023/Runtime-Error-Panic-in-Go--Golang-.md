<!--yml

分类：未分类

日期：2024-10-13 06:25:46

-->

# Go（Golang）中的运行时错误恐慌

> 来源：[`golangbyexample.com/runtime-error-panic-golang/`](https://golangbyexample.com/runtime-error-panic-golang/)

目录

**概述**

+   示例*  *# **概述**

程序中的运行时错误可能发生在以下几种情况。所有这些情况也会造成**恐慌**。

+   超出范围的数组访问

+   在空指针上调用函数

+   在关闭的通道上发送

+   错误的类型断言

# **示例**

让我们看一个由于超出范围的数组访问而导致的运行时错误示例。

```go
package main

import "fmt"

func main() {

	a := []string{"a", "b"}
	print(a, 2)
}

func print(a []string, index int) {
	fmt.Println(a[index])
}
```

**输出**

```go
panic: runtime error: index out of range [2] with length 2

goroutine 1 [running]:
main.checkAndPrint(...)
        main.go:12
main.main()
        /main.go:8 +0x1b
exit status 2
```

在上述程序中，我们有一个长度为 2 的切片，而我们在**打印**函数中尝试访问索引为 2 的切片。超出范围的访问是不允许的，这会造成恐慌，正如输出中所示。注意输出中有两个要点。

+   错误消息

+   恐慌发生时的堆栈跟踪

在程序中可能发生运行时错误的情况还有很多。我们不打算提到所有情况，但你能理解这个意思。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
