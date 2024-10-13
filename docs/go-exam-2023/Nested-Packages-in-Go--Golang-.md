<!--yml

类别：未分类

日期：2024-10-13 06:30:40

-->

# Go (Golang) 中的嵌套包

> 来源：[`golangbyexample.com/nested-packages-golang/`](https://golangbyexample.com/nested-packages-golang/)

目录

+   概述

+   示例

# **概述**

在 Go 中，可以创建嵌套包。嵌套包是指位于另一个包内部的那些包。让我们来看一个例子

# **示例**

创建一个名为 **learn** 的目录。我们将在 learn 目录中创建一个导入路径为 **“sample.com/learn”** 的模块。

```go
go mod init sample.com/learn
```

它将创建一个 go.mod 文件

**go.mod**

```go
module sameple.com/learn

go 1.14
```

让我们创建以下文件和目录

+   learn/main.go

+   learn/math/math.go

+   learn/math/advanced/advanced.go

你可以看到 **advanced** 是 **math** 包内的嵌套包。

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/advanced/advanced.go**

```go
package advanced
func Square(a int) int {
    return a * a
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
    "sample.com/learn/math/advanced"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(advanced.Square(2))
}
```

让我们运行这个程序

```go
learn $ go install
learn $ learn
3
1
4
```

关于上述程序的注意事项

+   我们在 main.go 中使用完整的合格路径导入了 **advanced** 包，即 **import “sample.com/learn/math/advanced”**

+   **Square** 函数是通过 **advanced** 包来调用的，即 **advanced.Square(2)**

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
