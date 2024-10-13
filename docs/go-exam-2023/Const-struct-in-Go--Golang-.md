<!--yml

类别：未分类

日期：2024-10-13 06:28:37

-->

# Go 中的常量结构体 (Golang)

> 来源：[https://golangbyexample.com/const-struct-go/](https://golangbyexample.com/const-struct-go/)

目录

**[概述](#Overview "Overview")**

+   [示例](#Example "Example")*  *# **概述**

Go 仅支持四种常量类型

+   数字（int, int64, float, float64, complex128 等）

+   字符串

+   字符或符文

+   布尔值

它不支持常量结构体。因此，下面的程序将引发编译错误。

# **示例**

```go
package main
import "fmt"
type employee struct {
    name string
    age  int
}
func main() {
    const e = employee{
        name: "John",
        age:  21,
    }
    fmt.Println(e)
}
```

**输出**

```go
const initializer employee literal is not a constant
```

然而，解决方法是创建一个可以返回结构体的函数。这在某种程度上满足了常量结构体的目的，因为它每次都会返回相同的结构体。

```go
package main
import "fmt"
type employee struct {
    name string
    age  int
}
func main() {
    e := baseEmployee()
    fmt.Println(e)
}
func baseEmployee() employee {
    return employee{
        name: "Unnamed",
        age:  0,
    }
}
```

**输出**

```go
{Unnamed 0}
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
