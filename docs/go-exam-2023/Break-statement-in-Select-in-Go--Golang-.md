<!--yml

类别: 未分类

date: 2024-10-13 06:23:58

-->

# Go (Golang)中的 Select 中的 Break 语句

> 来源：[`golangbyexample.com/break-keword-select-golang/`](https://golangbyexample.com/break-keword-select-golang/)

目录

**   概述**

+   代码*  *# **概述**

`break`关键字可以在 select 中使用，以终止执行最内层的语句，类似于 switch 和 for 循环。以下是 select 中**break**关键字的示例。

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
