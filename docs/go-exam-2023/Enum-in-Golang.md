<!--yml

类别：未分类

日期：2024-10-13 06:52:25

-->

# Golang 中的枚举

> 来源：[`golangbyexample.com/enum-in-golang/`](https://golangbyexample.com/enum-in-golang/)

目录

+   概述

+   示例

# 概述

**IOTA** 提供了一种自动创建 Golang 中枚举的方式。让我们来看一个例子。

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

在上述程序中，我们创建了一个新类型。

```go
type Size uint8
```

然后我们声明了一些类型为**Size**的常量。第一个常量**small**被设置为 iota，因此它将被设置为零。

```go
small Size = iota
```

这就是原因。

```go
fmt.Println(small)      >> outputs 0  
fmt.Println(medium)     >> outputs 1
fmt.Println(large)      >> outputs 2
fmt.Println(extraLarge) >> outputs 3
```

没有 IOTA，我们必须显式设置每个枚举值的值。

```go
package main

import "fmt"

type Size uint8

const (
	small      Size = 0
	medium     Size = 1
	large      Size = 2
	extraLarge Size = 3
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

我们还可以在 Size 类型上定义一个**toString**方法，以打印枚举的确切值。请参见下面的程序。

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

我们为**Size**类型定义了一个**toString**方法。它可以用来打印 Size 类型常量的字符串值。*
