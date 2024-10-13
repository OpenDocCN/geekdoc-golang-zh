<!--yml

类别：未分类

日期：2024-10-13 06:26:32

-->

# 如何在Go (Golang)中创建恐慌

> 来源：[https://golangbyexample.com/how-create-panic-golang/](https://golangbyexample.com/how-create-panic-golang/)

目录

**   [概述](#Overview "Overview")

+   [运行时错误恐慌](#Runtime_Error_Panic "Runtime Error Panic")

+   [显式调用panic函数](#Calling_the_panic_function_explicitly "Calling the panic function explicitly")*  *# **概述**

恐慌类似于golang中的异常。恐慌是为了在异常条件下退出程序。程序中可能以两种方式发生恐慌。

+   程序中的运行时错误

+   通过显式调用panic函数。这可以由程序员在程序无法继续并必须退出时调用。

Go提供了一个特殊函数来创建恐慌。下面是该函数的语法。

```go
func panic(v interface{})
```

该函数可以由程序员显式调用以创建恐慌。它接受一个空接口作为参数。当程序中发生恐慌时，它输出两件事。

+   作为参数传递给panic函数的错误信息

+   恐慌发生时的堆栈跟踪

# **运行时错误恐慌**

程序中的运行时错误可能发生在以下情况

+   超出边界的数组访问

+   在nil指针上调用函数

+   在封闭通道上发送

+   错误的类型断言

让我们看一个因超出边界的数组访问而导致的运行时错误的例子。

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

在上述程序中，我们有一个长度为2的切片，而我们在**print**函数中尝试访问索引3。超出边界的访问是不允许的，这将导致恐慌，如输出所示。请注意，在输出中有两件事。

+   错误信息

+   恐慌发生时的堆栈跟踪

在程序中发生运行时错误的情况还有很多。我们不会列出所有情况，但你应该明白。

# **显式调用panic函数**

一些情况下程序员可以显式调用panic函数：

+   该函数期望一个有效的参数，但却提供了一个nil参数。在这种情况下，程序无法继续并会因为传递了nil参数而引发恐慌。

+   任何其他程序无法继续的场景。

让我们看一个例子。

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

在上述程序中，我们再次有一个函数**checkAndPrint**，它接受一个切片作为参数和一个索引。然后它检查传递的索引是否大于切片长度减1。如果是，那么它就是切片的超出边界访问，所以它会引发恐慌。如果不是，它将打印该索引处的值。再次请注意，在输出中有两件事。

+   错误信息

+   恐慌发生时的堆栈跟踪

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
