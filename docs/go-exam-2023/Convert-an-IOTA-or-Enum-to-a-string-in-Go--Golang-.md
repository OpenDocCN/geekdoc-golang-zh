<!--yml

分类：未分类

日期：2024-10-13 06:52:30

-->

# 将 IOTA 或枚举转换为 Go 语言中的字符串

> 来源：[`golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/`](https://golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/)

目录

**   概述

+   示例*  *# 概述

在 Golang 中，可以通过使用 IOTA 创建枚举。请参考这篇文章以了解更多关于 IOTA 的信息。

> [Go 语言中的 IOTA](https://golangbyexample.com/iota-in-golang/)

[`golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT`](https://golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT)

在这篇文章中，我们将看到如何将 IOTA 或枚举转换为字符串值。默认情况下，当打印 IOTA 或枚举值时，它将打印其整数部分。请看这个例子。稍后我们将看到如何定义自定义的**toString**方法以打印 IOTA 或枚举的字符串值。

# 示例

```go
package main

import "fmt"

type Size uint8

const (
	small Size = iota
	medium
	large
	extraLarge
)

func main() {
	fmt.Println(small)
	fmt.Println(medium)
	fmt.Println(large)
	fmt.Println(extraLarge)
}
```

**输出**

```go
0
1
2
3
```

我们还可以在 Size 类型上定义一个**toString**方法，以打印枚举的确切值。请看下面的程序。

```go
package main
import "fmt"
type Size int
const (
    small = Size(iota)
    medium
    large
    extraLarge
)
func main() {
    var m Size = 1
    m.toString()
}
func (s Size) toString() {
    switch s {
    case small:
        fmt.Println("Small")
    case medium:
        fmt.Println("Medium")
    case large:
        fmt.Println("Large")
    case extraLarge:
        fmt.Println("Extra Large")
    default:
        fmt.Println("Invalid Size entry")
    }
}
```

**输出**

```go
medium
```

现在我们将为**Size**类型定义一个**toString**方法。它可以用来打印 Size 类型常量的字符串值。
