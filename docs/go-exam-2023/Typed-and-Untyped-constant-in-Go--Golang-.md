<!--yml

分类：未分类

日期：2024-10-13 06:27:57

-->

# Go (Golang) 中的已命名与未命名常量

> 来源：[`golangbyexample.com/typed-untyped-constant-golang/`](https://golangbyexample.com/typed-untyped-constant-golang/)

目录

**   已命名与未命名常量

+   已命名常量

+   未命名常量*  *# **已命名与未命名常量**

在 Go 中，常量的处理方式与其他语言不同。Go 具有非常强的类型系统，不允许类型之间的隐式转换。即使是相同的数值类型，也不允许在没有显式转换的情况下进行任何操作。例如，你不能将**int32**和**int64**的值相加。要相加，必须将**int32**显式转换为**int64**，或反之亦然。然而，未命名常量可以暂时摆脱 Go 的类型系统的限制，正如我们在本文中将看到的。

# **已命名常量**

在声明中指定类型的常量是已命名常量。例如，下面我们声明一个类型为 int32 的常量。

```go
const a int32 = 8
```

这个常量 a 只能赋值给类型为 int32 的变量。如果将其赋值给其他类型的变量，则会引发错误。有关说明，请参见下面的程序。

```go
package main

func main() {
    const a int32 = 8

    var i1 int32
    var i2 int64

    i1 = a
    i2 = a
}
```

**输出：**

```go
cannot use a (type int32) as type int64 in assignment
```

# **未命名常量**

未命名常量是未指定类型的常量。在 Go 中，未命名常量可以是已命名的或未命名的。在这两种情况下，它都没有与之关联的类型。

未命名未指定常量的示例。

```go
123        //Default hidden type is int
"circle"   //Default hidden type is string
5.6\.       //Default hidden type is float64
true       //Default hidden type is bool
'a'        //Default hidden type is rune
3+5i       //Default hidden type is complex128
```

已命名未命名常量的示例。

```go
const a = 123        //Default hidden type is int
const b = "circle"   //Default hidden type is string
const c = 5.6       //Default hidden type is float64
const d = true       //Default hidden type is bool
const e = 'a'        //Default hidden type is rune
const f = 3+5i       //Default hidden type is complex128
```

未命名常量确实有一个默认的隐式类型。以下表格说明了数值、字符串、字符和布尔值的隐式默认类型。

常量的默认隐式类型

| 整数 | int |
| --- | --- |
| 浮点数 | float64 |
| 复数 | complex128 |
| 字符串 | string |
| 布尔值 | bool |
| 字符 | int32 或 rune |

当你使用**fmt.Printf**打印任何未命名常量时，它将打印默认的隐式类型。请参见下面的程序及未命名和已命名的未命名常量的输出。

```go
package main

import "fmt"

func main() {
    //Unanamed untyped constant
    fmt.Printf("Type: %T Value: %v\n", 123, 123)
    fmt.Printf("Type: %T Value: %v\n", "circle", "circle")
    fmt.Printf("Type: %T Value: %v\n", 5.6, 5.6)
    fmt.Printf("Type: %T Value: %v\n", true, true)
    fmt.Printf("Type: %T Value: %v\n", 'a', 'a')
    fmt.Printf("Type: %T Value: %v\n", 3+5i, 3+5i)

    //Named untyped constant
    const a = 123      //Default hidden type is int
    const b = "circle" //Default hidden type is string
    const c = 5.6      //Default hidden type is float64
    const d = true     //Default hidden type is bool
    const e = 'a'      //Default hidden type is rune
    const f = 3 + 5i   //Default hidden type is complex128

    fmt.Println("")
    fmt.Printf("Type: %T Value: %v\n", a, a)
    fmt.Printf("Type: %T Value: %v\n", b, b)
    fmt.Printf("Type: %T Value: %v\n", c, c)
    fmt.Printf("Type: %T Value: %v\n", d, d)
    fmt.Printf("Type: %T Value: %v\n", e, e)
    fmt.Printf("Type: %T Value: %v\n", f, f)
}
```

**输出：**

```go
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)

Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

上述程序输出**int32**而不是 rune，因为 rune 是**int32**的别名。

已命名或未命名常量的默认类型将成为其赋值变量的类型。例如，在下面的代码中，变量 a 将获得来自未命名常量**123**的默认类型**int**。

```go
var a = 123
```

让我们看一个程序，说明上述所有未命名类型常量的要点。

```go
package main
import "fmt"
func main() {
    //Untyped
    var u = 123      //Default hidden type is int
    var v = "circle" //Default hidden type is string
    var w = 5.6      //Default hidden type is float64
    var x = true     //Default hidden type is bool
    var y = 'a'      //Default hidden type is rune
    var z = 3 + 5i   //Default hidden type is complex128
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

**输出：**

```go
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
```

现在，脑海中浮现的问题是未命名常量的用途是什么。未命名常量的用途在于常量的类型将根据其被赋值的变量的类型来决定。听起来令人困惑吗？让我们通过一个示例来看。

数学包中的**π**常量值如下所示。

```go
const Pi = 3.14159265358979323846264338327950288419716939937510582097494459
```

请注意，类型未指定，它仅具有一个隐式默认类型（在此为**float64**）。让我们来看一段代码。

```go
package main
import (
    "fmt"
    "math"
)
func main() {
    var f1 float32
    var f2 float64
    f1 = math.Pi
    f2 = math.Pi

    fmt.Printf("Type: %T Value: %v\n", math.Pi, math.Pi)
    fmt.Printf("Type: %T Value: %v\n", f1, f1)
    fmt.Printf("Type: %T Value: %v\n", f2, f2)
}
```

**输出：**

```go
Type: float64 Value: 3.141592653589793
Type: float32 Value: 3.1415927
Type: float64 Value: 3.141592653589793
```

请注意上面的程序。

+   由于**math.Pi**常量的无类型特性，它可以被赋值给**float32**和**float64**类型的变量。在 GO 中，类型固定后这是不可能的。

+   当我们打印**math.Pi**的类型时，它打印出默认类型，即**float64**。

根据使用案例，无类型常量可以被赋值给低精度类型（float32）或高精度类型（float64）。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
