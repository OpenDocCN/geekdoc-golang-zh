<!--yml

分类：未分类

日期：2024-10-13 06:28:41

-->

# Go中的IOTA（Golang）

> 来源：[https://golangbyexample.com/iota-in-golang/](https://golangbyexample.com/iota-in-golang/)

这是Golang综合教程系列的第22章。请参考此链接以获取该系列的其他章节– [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程**– [Goroutines](https://golangbyexample.com/goroutines-golang/)

**上一个教程**– [接口](https://golangbyexample.com/interface-in-golang/)

现在让我们看看当前的教程。下面是当前教程的目录。

目录

**[概述](#Overview "Overview")**

+   [更多关于IOTA的信息](#More_about_IOTA "More about IOTA")

+   [Golang中的枚举](#Enum_in_Golang "Enum in Golang")

+   [结论](#Conclusion "Conclusion")*  *# **概述**

Iota是一个标识符，用于常量，能够简化使用自动递增数字的常量定义。**IOTA**关键字表示从零开始的整数常量。因此，它可以用于在Go中创建有效的常量。它们还可以用于在Go中创建枚举，正如我们在本教程后面将看到的那样。

没有IOTA的自动递增常量。

```
const (
    a = 0
    b = 1
    c = 2
)
```

带IOTA的自动递增常量。

```
const (
    a = iota
    b
    c
)
```

两者将设置。

```
a=0
b=1
c=2
```

所以IOTA是。

+   一个从零开始的计数器。

+   每行增加1。

+   仅用于常量。

IOTA从零开始，每行增加1，但也有一些注意事项。首先，让我们看一个简单的例子，其中iota从零开始并在每一行后递增1。

```
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

```
0
1
2
```

Iota将**a**的值设置为零。然后在每一行上递增该值1。因此，输出为0，接着是1，再接着是2。

# **更多关于IOTA的信息**

让我们看看与iota相关的其他几点。

+   iota关键字也可以在每一行上使用。在这种情况下，iota将从零开始并在每一行递增。这将与上述情况相同。

```
const (
	a = iota
	b = iota
	c = iota
)
```

将输出。

```
0
1
2
```

+   iota关键字也可以跳过。在这种情况下，iota将从零开始并在每一行递增。这与上述两种情况相同。

```
const (
    a = iota
    b
    c = iota
)
```

将输出。

```
0
1
2
```

+   如果存在空行或注释行，则不会递增。

```
const (
	a = iota

	b
	//comment
	c
)
```

将输出。

```
0
1
2
```

+   如果再次使用const关键字，iota值将重置并重新从零开始。

```
const (
	a = iota
	b
)
const (
	c = iota
)
```

将输出。

```
0
1
0
```

+   可以使用空标识符跳过iota递增。

```
const (
	a = iota
	_
	b
	c
)
```

将输出。

```
0
2
3
```

+   iota表达式–iota允许表达式，可以用于为常量设置任何值。

```
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

```
0
5
8
```

第一次的iota值是零，因此输出为零。

下一行的iota值为1，因此输出为1+4=5。

下一行，iota值为2，因此输出为2*4=8。

+   iota也可以从非零数字开始，iota表达式也可以用于从任何数字开始iota。

```
const (
	a = iota + 10
	b
	c
)
```

将输出

```
10
11
12
```

# **Golang中的枚举**

**IOTA**提供了一种自动创建Golang中的枚举的方法。让我们看看一个例子。

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

在上述程序中，我们创建了一个新类型。

```
type Size uint8
```

然后我们声明了一些 **Size** 类型的常量。第一个常量 **small** 被设置为 iota，因此它的值将为零。

```
small Size = iota
```

这就是原因。

```
fmt.Println(small)      >> outputs 0  
fmt.Println(medium)     >> outputs 1
fmt.Println(large)      >> outputs 2
fmt.Println(extraLarge) >> outputs 3
```

没有 IOTA，我们必须显式地设置每个枚举值的值。

```
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

```
0
1
2
3
```

我们还可以在 Size 类型上定义一个 **toString** 方法，以打印枚举的确切值。见下方程序。

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

我们为 **Size** 类型定义了一个 **toString** 方法。它可以用于打印 Size 类型常量的字符串值。

# **结论**

这就是关于 IOTA 和枚举在 Golang 中的所有内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [协程](https://golangbyexample.com/goroutines-golang/)

**上一篇教程** – [接口](https://golangbyexample.com/interface-in-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
