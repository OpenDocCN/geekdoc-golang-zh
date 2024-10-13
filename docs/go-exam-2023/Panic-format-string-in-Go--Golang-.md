<!--yml

类别: 未分类

日期: 2024-10-13 06:25:56

-->

# Go (Golang) 中的 Panic 格式字符串

> 来源：[`golangbyexample.com/panic-format-string-go/`](https://golangbyexample.com/panic-format-string-go/)

目录

+   概述**

+   示例

# **概述**

下面是 panic 函数的语法

```go
func panic(v interface{})
```

它将空接口作为参数。它并没有提供任何格式化错误字符串的方法。不过有一个变通办法。可以使用**fmt**包的**Sprintf**函数在将错误消息传递给 panic 函数之前进行格式化。让我们看看一个程序

# **示例**

```go
package main

import (
	"fmt"
)

func main() {
	a := []string{"a", "b"}
	checkAndPrint(a, 2)
	fmt.Println("Exiting normally")
}

func checkAndPrint(a []string, index int) {
	if index > (len(a) - 1) {
		errorString := fmt.Sprintf("Out of bounds access for slice. Index passed: %d", index)
		panic(errorString)
	}
	fmt.Println(a[index])
}
```

**输出**

```go
panic: Out of bounds access for slice. Index passed: 2

goroutine 1 [running]:
main.checkAndPrint(0xc00009af58, 0x2, 0x2, 0x2)
        main.go:17 +0x157
main.main()
        main.go:10 +0x81
exit status 2
```

在上面的程序中，我们有一个函数**checkAndPrint**，它检查并打印在参数中传递的索引处的切片元素。如果传递的索引大于数组的长度，则程序将发生 panic。注意我们是如何在**checkAndPrint**函数中格式化错误字符串然后传递给 panic 的

```go
errorString := fmt.Sprintf("Out of bounds access for slice. Index passed: %d", index)
```

程序也输出正确格式化的消息

```go
panic: Out of bounds access for slice. Index passed: 2
```


