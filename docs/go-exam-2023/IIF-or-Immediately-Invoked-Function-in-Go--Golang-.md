<!--yml

类别：未分类

日期：2024-10-13 06:10:48

-->

# IIF 或在 Go (Golang) 中的立即调用函数

> 来源：[`golangbyexample.com/immediately-invoked-function-go/`](https://golangbyexample.com/immediately-invoked-function-go/)

目录

+   概述：

+   用例

+   代码：

# **概述：**

**IIF 或** 立即调用函数是指那些可以同时定义和执行的函数。通过在函数结束括号后添加 **()**，可以立即调用该函数。

# **用例**

IIF 函数的一个用途是在你不想暴露函数逻辑时，无论是在包内还是包外。例如，假设有一个函数用于设置某个值。你可以将所有设置逻辑封装在一个 IIF 函数中。这个函数在包外或包内都无法被调用。

# **代码：**

让我们看看工作代码。计算 2 的平方的函数通过在末尾加上 () 立即被调用，返回的值被赋给变量 **squareOf2**。

```go
package main

import "fmt"

func main() {
    squareOf2 := func() int {
        return 2 * 2
    }()
    fmt.Println(squareOf2)
}
```

**输出：**

```go
2
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [iif](https://golangbyexample.com/tag/iif/)*
