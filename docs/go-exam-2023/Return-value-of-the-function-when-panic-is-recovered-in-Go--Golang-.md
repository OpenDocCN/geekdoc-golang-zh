<!--yml

类别：未分类

日期：2024-10-13 06:26:12

-->

# 当恐慌在 Go（Golang）中被恢复时，函数的返回值

> 来源：[`golangbyexample.com/return-value-function-panic-recover-go/`](https://golangbyexample.com/return-value-function-panic-recover-go/)

目录

+   概述

+   程序 

# **概述**

当恐慌被恢复时，恐慌函数的返回值将是该函数返回类型的默认值。

# **程序**

让我们来看一个程序示例

```go
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (int, error) {
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    return a[index], nil
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**输出**

```go
Recovering from panic: Out of bound access for slice
Val: 0
Error: 
```

在上面的程序中，我们有一个**checkAndGet**函数，它获取 int 切片中特定索引的值。如果传递给此函数的索引大于（切片长度-1），则会引发恐慌。同时还有一个**handleOutOfBounds**函数用于从恐慌中恢复。因此，我们将索引 2 传递给**checkAndGet**函数，它引发了恐慌，并在**handleOutOfBounds**函数中得以恢复。这就是我们首先得到这个输出的原因。

```go
Recovering from panic: Out of bound access for slice
```

请注意在主函数中，我们以这样的方式重新获取**checkAndGet**的返回值。

```go
val, err := checkAndGet(a, 2)
```

**checkAndGet**有两个返回值

+   int

+   error

由于**checkAndGet**会引发恐慌，而该恐慌在 handleOutOfBounds 函数中被恢复，因此**checkAndGet**的返回值将是其类型的默认值。

因此

```go
fmt.Printf("Val: %d\n", val)
```

输出

```go
Val: 0
```

因为零是**int**类型的默认值。

而且

```go
fmt.Println("Error: ", err)
```

输出

```go
Error: 
```

因为 nil 是**error**类型的默认值。

如果你不想返回类型的默认零值，那么可以使用命名返回值。我们来看一个程序示例。

```go
package main
import (
    "fmt"
)
func main() {
    a := []int{5, 6}
    val, err := checkAndGet(a, 2)
    fmt.Printf("Val: %d\n", val)
    fmt.Println("Error: ", err)
}
func checkAndGet(a []int, index int) (value int, err error) {
    value = 10
    defer handleOutOfBounds()
    if index > (len(a) - 1) {
        panic("Out of bound access for slice")
    }
    value = a[index]
    return value, nil
}
func handleOutOfBounds() {
    if r := recover(); r != nil {
        fmt.Println("Recovering from panic:", r)
    }
}
```

**输出**

```go
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

这个程序与前面的程序相同，唯一的区别是我们在**checkAndGet**函数中使用了命名返回值。

```go
func checkAndGet(a []int, index int) (value int, err error)
```

我们在**checkAndGet**函数中将命名返回值设置为 10

```go
value = 10
```

这就是为什么我们在这个程序中得到以下输出，因为引发了恐慌并得以恢复

```go
Recovering from panic: Out of bound access for slice
Val: 10
Error: 
```

还要注意，如果程序中没有引发恐慌，那么它将输出正确的索引值。


