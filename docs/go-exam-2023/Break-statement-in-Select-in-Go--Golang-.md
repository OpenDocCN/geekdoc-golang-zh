<!--yml

类别: 未分类

date: 2024-10-13 06:23:58

-->

# Go (Golang)中的Select中的Break语句

> 来源：[https://golangbyexample.com/break-keword-select-golang/](https://golangbyexample.com/break-keword-select-golang/)

目录

**   [概述](#Overview "Overview")**

+   [代码](#Code "Code")*  *# **概述**

`break`关键字可以在select中使用，以终止执行最内层的语句，类似于switch和for循环。以下是select中**break**关键字的示例。

# **代码**

```go
package main

import "fmt"

func main() {
	ch := make(chan string, 1)
	ch <- "Before break"

	select {
	case msg := <-ch:
		fmt.Println(msg)
		break
		fmt.Println("After break")
	default:
		fmt.Println("Default case")
	}
}
```

**输出**

```go
Before break
```

**break**语句将终止最内层语句的执行，以下行将永远不会被执行。

```go
fmt.Println("After break")
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
