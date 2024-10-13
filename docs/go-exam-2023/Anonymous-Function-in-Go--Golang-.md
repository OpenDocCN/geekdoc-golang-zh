<!--yml

category: 未分类

date: 2024-10-13 06:11:11

-->

# Go（Golang）中的匿名函数

> 来源：[`golangbyexample.com/go-anonymous-function/`](https://golangbyexample.com/go-anonymous-function/)

目录

**   概述

+   代码*  *# **概述**

正如名称所示，匿名函数是没有名称的函数。在 Golang 中，函数是第一类变量，这意味着

+   它们可以被赋值给一个变量

+   作为函数参数传递

+   从函数返回

在 Go 中，函数是第一类变量，因此它也可以作为值使用。当将函数作为值使用时，它没有名称，可以赋值给变量。这种函数被称为匿名函数，因为函数没有名称。

它们通常是为了短期使用或有限功能而创建的。请看下面的例子。

在这个例子中，**max**变量被赋值为一个函数。赋值给**max**的函数没有任何名称。调用这个函数的唯一方式是使用**max**变量，这也是我们在这个程序中所做的。

还要注意，我们可以向匿名函数传递参数，并从中返回值。

# **代码**

```go
package main

import "fmt"

var max = func(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func main() {
    res := max(2, 3)
    fmt.Println(res)
}
```

**输出：**

```go
3
```

匿名函数也可以被执行为 IIF 或立即调用函数。在这种情况下，你不需要将它赋值给任何变量。请看下面的例子：

```go
package main

import "fmt"

func main() {
    func() {
        fmt.Println("From anoymous function")
    }()
}
```

**输出：**

```go
From anoymous function
```

+   [golang](https://golangbyexample.com/tag/golang/)*
