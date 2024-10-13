<!--yml

类别：未分类

日期：2024-10-13 06:27:28

-->

# 在 Go (Golang) 中，`main` 函数内部可以使用 **defer** 吗？

> 来源：[https://golangbyexample.com/defer-inside-main-golang/](https://golangbyexample.com/defer-inside-main-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

正如名称所示，defer 用于延迟函数中的清理活动。这些清理活动将在函数结束时执行。

`defer` 也可以在 `main` 函数内部使用的原因。让我们看一个例子。

# **示例**

```go
package main
import "fmt"
func main() {
    defer test()
    fmt.Println("Executed in main")
}
func test() {
    fmt.Println("In Defer")
}
```

**输出**

```go
Executed in main
In Defer
```

在上述程序中，有一个 **defer** 语句调用名为 **test** 的自定义函数。从输出可以看出，**test** 函数在 `main` 中的所有内容执行完毕后被调用，并在 `main` 返回之前调用。这就是

```go
Executed in main
```

在输出之前打印。

```go
In Defer
```

上述函数同样表明在 `main` 函数中使用 defer 是完全可以的。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
