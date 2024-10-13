<!--yml

类别：未分类

日期：2024-10-13 06:07:38

-->

# 理解 Golang 中的 uintptr

> 来源：[`golangbyexample.com/understanding-uintptr-golang/`](https://golangbyexample.com/understanding-uintptr-golang/)

目录

**   概述

+   属性

+   目的*  *# **概述**

这是一种无符号整数类型，足够大以保存任何指针地址。因此它的大小是平台相关的。它只是一个地址的整数表示。

# **属性**

+   **uintptr**可以转换为**unsafe.Pointer**，反之亦然。稍后我们将讨论**uintptr**转换为**unsafe.Pointer**的用途。

+   可以对**uintptr**进行算术运算。请注意，Go 中的**指针**或**unsafe.Pointer**不能进行算术运算。

+   **uintptr**虽然保存了一个指针地址，但只是一个值，并不引用任何对象。因此

    +   如果对应的对象移动，其值将不会更新。例如，当 goroutine 栈变化时。

    +   相应的对象可以被垃圾回收。GC 不会将**uintptr**视为活引用，因此它们可以被垃圾回收。

# **目的**

**uintptr**可以用于以下目的：

+   uintptr 的一个目的是与 unsafe.Pointer 一起用于不安全的内存访问。

    +   unsafe.Pointer 被转换为 uintptr。

    +   然后在 uintptr 上进行算术运算。

    +   uintptr 被转换回 unsafe.Pointer 以访问现在由地址指向的对象。

注意，上述步骤应与垃圾回收器原子性相关，否则可能导致问题。例如，在第一步后，引用的对象可能会被回收。如果发生这种情况，则在第三步之后，指针将成为无效的 Go 指针，并可能导致程序崩溃。查看 unsafe 包文档。

[`golang.org/pkg/unsafe/#Pointer`](https://golang.org/pkg/unsafe/#Pointer)

它列出了上述转换何时可以安全。请参见下面提到的场景的代码。

在下面的代码中，我们进行如下运算，以获取结构体 sample 中字段**“b”**的地址，然后打印该地址处的值。下面的代码与垃圾回收器是原子的。

```go
p := unsafe.Pointer(uintptr(unsafe.Pointer(s)) + unsafe.Offsetof(s.b))
```

```go
package main
import (
    "fmt"
    "unsafe"
)
type sample struct {
    a int
    b string
}
func main() {
    s := &sample{a: 1, b: "test"}

   //Getting the address of field b in struct s
    p := unsafe.Pointer(uintptr(unsafe.Pointer(s)) + unsafe.Offsetof(s.b))

    //Typecasting it to a string pointer and printing the value of it
    fmt.Println(*(*string)(p))
}
```

**输出：**

```go
test
```

+   uintptr 的另一个目的，是当你想保存指针地址值以进行打印或存储时。由于地址仅被存储而不引用任何东西，因此相应的对象可以被垃圾回收。

参见下面的代码，我们将 unsafe.Pointer 转换为 uintptr 并打印它。同时，注意如前所述，一旦 unsafe.Pointer 被转换为 uintptr，引用就丢失了，引用变量可以被垃圾回收。

```go
package main

import (
    "fmt"
    "unsafe"
)

type sample struct {
    a int
    b string
}

func main() {
    s := &sample{
        a: 1,
        b: "test",
    }
    //Get the address as a uintptr
    startAddress := uintptr(unsafe.Pointer(s))
    fmt.Printf("Start Address of s: %d\n", startAddress)
}
```

**输出：**

输出将依赖于机器，因为它是一个地址。

```go
Start Address of s: 824634330992
```

+   [数据](https://golangbyexample.com/tag/data/) *   [无符号整数指针](https://golangbyexample.com/tag/uintptr/) *
