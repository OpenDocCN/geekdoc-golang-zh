<!--yml

category: 未分类

date: 2024-10-13 06:25:20

-->

# 在 Go (Golang) 中忽略错误

> 来源：[`golangbyexample.com/ignoring-errors-golang/`](https://golangbyexample.com/ignoring-errors-golang/)

目录

**   概述

+   代码*  *# **概述**

下划线（‘_’）运算符可用于忽略函数调用返回的错误。在查看程序之前，需要注意的是，错误不应该被忽略。这不是推荐的做法。让我们来看一个程序。

# **代码**

```go
package main
import (
    "fmt"
    "os"
)
func main() {
    file, _ := os.Open("non-existing.txt")
    fmt.Println(file)
}
```

**输出**

```go
{nil}
```

在上述程序中，我们使用下划线运算符忽略了打开不存在文件时返回的错误。这就是为什么函数返回的文件实例为 nil。因此，在使用函数返回的任何其他参数之前，最好先检查错误，因为它可能为 nil，并导致不必要的问题，有时甚至可能导致恐慌。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
