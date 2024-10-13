<!--yml

分类：未分类

日期：2024-10-13 06:28:41

-->

# Go 中的 IOTA（Golang）

> 来源：[`golangbyexample.com/iota-in-golang/`](https://golangbyexample.com/iota-in-golang/)

这是 Golang 综合教程系列的第二十二章。请参考此链接以获取该系列的其他章节– [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程**– [Goroutines](https://golangbyexample.com/goroutines-golang/)

**上一个教程**– [接口](https://golangbyexample.com/interface-in-golang/)

现在让我们看看当前的教程。下面是当前教程的目录。

目录

**概述**

+   更多关于 IOTA 的信息

+   Golang 中的枚举

+   结论*  *# **概述**

Iota 是一个标识符，用于常量，能够简化使用自动递增数字的常量定义。**IOTA**关键字表示从零开始的整数常量。因此，它可以用于在 Go 中创建有效的常量。它们还可以用于在 Go 中创建枚举，正如我们在本教程后面将看到的那样。

没有 IOTA 的自动递增常量。

```go
const (
    a = 0
    b = 1
    c = 2
)
```

带 IOTA 的自动递增常量。

```go
const (
    a = iota
    b
    c
)
```

两者将设置。

```go
a=0
b=1
c=2
```

所以 IOTA 是。

+   一个从零开始的计数器。

+   每行增加 1。

+   仅用于常量。

IOTA 从零开始，每行增加 1，但也有一些注意事项。首先，让我们看一个简单的例子，其中 iota 从零开始并在每一行后递增 1。

```go
package main

import "fmt"

const (
    a = iota
    b
    c
)
func main() {
    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
}
```

**输出**

```go
0
1
2
```

Iota 将**a**的值设置为零。然后在每一行上递增该值 1。因此，输出为 0，接着是 1，再接着是 2。

# **更多关于 IOTA 的信息**

让我们看看与 iota 相关的其他几点。

+   iota 关键字也可以在每一行上使用。在这种情况下，iota 将从零开始并在每一行递增。这将与上述情况相同。

```go
const (
	a = iota
	b = iota
	c = iota
)
```

将输出。

```go
0
1
2
```

+   iota 关键字也可以跳过。在这种情况下，iota 将从零开始并在每一行递增。这与上述两种情况相同。

```go
const (
    a = iota
    b
    c = iota
)
```

将输出。

```go
0
1
2
```

+   如果存在空行或注释行，则不会递增。

```go
const (
	a = iota

	b
	//comment
	c
)
```

将输出。

```go
0
1
2
```

+   如果再次使用 const 关键字，iota 值将重置并重新从零开始。

```go
const (
	a = iota
	b
)
const (
	c = iota
)
```

将输出。

```go
0
1
0
```

+   可以使用空标识符跳过 iota 递增。

```go
const (
	a = iota
	_
	b
	c
)
```

将输出。

```go
0
2
3
```

+   iota 表达式–iota 允许表达式，可以用于为常量设置任何值。

```go
package main

import "fmt"

const (
	a = iota
	b = iota + 4
	c = iota * 4
)

func main() {
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
}
```

将输出。

```go
0
5
8
```

第一次的 iota 值是零，因此输出为零。

下一行的 iota 值为 1，因此输出为 1+4=5。

下一行，iota 值为 2，因此输出为 2*4=8。

+   iota 也可以从非零数字开始，iota 表达式也可以用于从任何数字开始 iota。

```go
const (
	a = iota + 10
	b
	c
)
```

将输出

```go
10
11
12
```

# **Golang 中的枚举**

**IOTA**提供了一种自动创建 Golang 中的枚举的方法。让我们看看一个例子。

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

然后我们声明了一些 **Size** 类型的常量。第一个常量 **small** 被设置为 iota，因此它的值将为零。

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

没有 IOTA，我们必须显式地设置每个枚举值的值。

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

我们还可以在 Size 类型上定义一个 **toString** 方法，以打印枚举的确切值。见下方程序。

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

我们为 **Size** 类型定义了一个 **toString** 方法。它可以用于打印 Size 类型常量的字符串值。

# **结论**

这就是关于 IOTA 和枚举在 Golang 中的所有内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [协程](https://golangbyexample.com/goroutines-golang/)

**上一篇教程** – [接口](https://golangbyexample.com/interface-in-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
