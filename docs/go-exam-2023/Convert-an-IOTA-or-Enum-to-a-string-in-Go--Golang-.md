<!--yml

分类：未分类

日期：2024-10-13 06:52:30

-->

# 将IOTA或枚举转换为Go语言中的字符串

> 来源：[https://golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/](https://golangbyexample.com/convert-an-iota-or-enum-to-a-string-in-go-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# 概述

在Golang中，可以通过使用IOTA创建枚举。请参考这篇文章以了解更多关于IOTA的信息。

> [Go语言中的IOTA](https://golangbyexample.com/iota-in-golang/)

[https://golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT](https://golangbyexample.com/iota-in-golang/embed/#?secret=0FEJ5OZNxf#?secret=4y3pVxudgT)

在这篇文章中，我们将看到如何将IOTA或枚举转换为字符串值。默认情况下，当打印IOTA或枚举值时，它将打印其整数部分。请看这个例子。稍后我们将看到如何定义自定义的**toString**方法以打印IOTA或枚举的字符串值。

# 示例

```
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

```
0
1
2
3
```

我们还可以在Size类型上定义一个**toString**方法，以打印枚举的确切值。请看下面的程序。

```
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

```
medium
```

现在我们将为**Size**类型定义一个**toString**方法。它可以用来打印Size类型常量的字符串值。
