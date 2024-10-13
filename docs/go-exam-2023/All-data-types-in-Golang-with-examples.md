<!--yml

类别：未分类

日期：2024-10-13 06:08:44

-->

# Golang中的所有数据类型及示例

> 来源：[https://golangbyexample.com/all-data-types-in-golang-with-examples/](https://golangbyexample.com/all-data-types-in-golang-with-examples/)

**注意：** 如果你对学习Golang感兴趣，我们有一个全面的Golang教程系列，欢迎查看 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)。现在让我们来看一下当前的教程。以下是目录。

目录

**   [概述](#Overview "Overview")

+   [基本类型](#Basic_Types "Basic Types")

    +   [整数（有符号和无符号）](#Integers_Signed_and_UnSigned "Integers (Signed and UnSigned)")

    +   [浮点数](#Floats "Floats")

    +   [复数](#Complex_Numbers "Complex Numbers")

    +   [字节](#Byte "Byte")

    +   [Rune](#Rune "Rune")

    +   [字符串](#String "String")

    +   [布尔值](#Booleans "Booleans")

+   [复合类型](#Composite_Types "Composite Types")

    +   [非引用类型](#Non-Reference_Types "Non-Reference Types")

        +   [数组](#Arrays "Arrays")

        +   [结构体](#Structs "Structs")

    +   [引用类型](#Reference_Types "Reference Types")

        +   [切片](#Slices "Slices")

        +   [通道](#Channels "Channels")

        +   [映射](#Maps "Maps")

        +   [指针](#Pointers "Pointers")

        +   [函数](#Functions "Functions")

    +   [接口](#Interface "Interface")

        +   [空接口的特殊情况](#Special_case_of_empty_interface "Special case of empty interface")

+   [结论](#Conclusion "Conclusion")*  *# 概述

Golang是一种静态类型编程语言，意味着每个变量都有一个类型。Go有几种内置类型，我们将在本文中探讨。Go中的数据类型可以分为两种类型。

1.  基本类型

1.  复合类型

+   基本类型

    +   整数

        +   有符号

            +   int

            +   int8

            +   int16

            +   int32

            +   int64

        +   无符号

            +   uint

            +   uint8

            +   uint16

            +   uint32

            +   uint64

            +   uintptr

    +   浮点数

        +   float32

        +   float64

    +   复数

        +   complex64

        +   complex128

    +   字节

    +   Rune

    +   字符串

    +   布尔值

+   复合类型

    +   集合/聚合或非引用类型

        +   数组

        +   结构体

    +   引用类型

        +   切片

        +   映射

        +   通道

        +   指针

        +   函数/方法

    +   接口

        +   空接口的特殊情况

# **基本类型**

让我们先讨论一下GO中的基本类型。

## **整数**（有符号和无符号）

整数可以是有符号或无符号。

**有符号**

有符号整数有如下五种类型：

| **类型** | **大小** |
| --- | --- |
| int | 平台依赖 |
| int8 | 8位/1字节 |
| int16 | 16位/2字节 |
| int32 | 32位/4字节 |
| int64 | 64位/8字节 |

**int**

**大小：** 平台依赖。

+   在32位机器上，int的大小为32位或4字节。

+   在64位机器上，int的大小为64位或8字节。

**范围**：再次依赖于平台。

+   在32位机器上，int的范围为-2^(31)到2^(31)-1。

+   在64位机器上，int的范围为-2^(63)到2^(63)-1。

**何时使用：**

+   在使用有符号整数时，建议使用int，除非在以下提到的情况下。

    +   当机器为 32 位且所需范围大于 -2^(31) 到 2^(31) - 1 时，使用 **int64** 而不是 **int**。请注意，在这种情况下，int64 需要 2 个 32 位内存地址来组合成一个 64 位数字。

    +   当范围较小时，使用适当的整数类型。

**属性：**

+   声明一个 int

```go
 var a int
```

+   int 是**整数**的默认类型。当你不指定类型时，默认类型为 int

```go
b := 2 //The default is also int
fmt.Println(reflect.TypeOf(b)) => int
```

+   golang 的 **bits** 包可以帮助了解系统上 **int** 的大小

```go
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
sizeOfIntInBits := bits.UintSize
fmt.Println(sizeOfIntInBits) => 32 0r 34
```

+   **unsafe.Sizeof()** 函数也可以用来查看 int 的字节大小

**完整工作代码**

以下是上述属性的完整工作代码

```go
package main

import (
    "fmt"
    "math/bits"
    "reflect"
    "unsafe"
)

func main() {
    //This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
    sizeOfIntInBits := bits.UintSize
    fmt.Printf("%d bits\n", sizeOfIntInBits)

    var a int
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))

    b := 2
    fmt.Printf("b's typs is %s\n", reflect.TypeOf(b))
}
```

**输出：**

```go
64 bits
8 bytes
a's type is int
b's typs is int
```

**int8**

**大小：** 8 位或 1 字节

**范围**：-2⁷ 到 2⁷ - 1。

**何时使用：**

+   当已知 int 范围在 -2⁷ 到 2⁷ - 1 之间时使用 int8。对于临时值，如循环不变量，仍然建议使用 int，尽管它可能占用更多空间，因为在某些操作或库调用中，它可能会被提升为 int。

+   对于值范围在 -27 到 27 - 1 的数组，使用 int8 是一个好用例。例如，如果你存储小写字母的 ASCII 索引，则可以使用 int8。

+   对于数据值，使用 int8 是一个好主意。

**示例：**

以下代码示例说明了以下几点

+   声明一个 int8

+   打印 int8 的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a int 8
    var a int8 = 2

    //Size of int8 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
1 bytes
a's type is int8
```

**int16**

**大小：** 16 位或 2 字节

**范围**：-2^(15) 到 2^(15) - 1。

**何时使用：**

+   当已知 int 范围在 -2^(15) 到 2^(15) - 1 之间时使用 int16。对于临时值，如循环不变量，仍然建议使用 int，尽管它可能占用更多空间，因为在某些操作或库调用中，它可能会被提升为 int。

+   对于值范围在 -215 到 215 - 1 的数组，使用 int8 是一个好用例。例如，如果你存储小写字母的 ASCII 索引，则可以使用 int16。

**示例：**

以下代码示例说明了以下几点

+   声明一个 int16

+   打印 int16 的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a int16
    var a int16 = 2

    //Size of int8 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
2 bytes
a's type is int16
```

**int32**

**大小：** 32 位或 4 字节

**范围**：-2^(31) 到 2^(31) - 1。

**示例：**

以下代码示例说明了以下几点

+   声明一个 int32

+   打印 int8 的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a int32
    var a int32 = 2

    //Size of int32 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
4 bytes
a's type is int32
```

**int64**

**大小：** 64 位或 8 字节

**范围**：-2^(63) 到 2^(63) - 1

**何时使用：**

+   **int64** 用于范围较高的情况。例如，**time.Duration** 的类型是 **int64**

**示例：**

以下代码示例说明了以下几点

+   声明一个 int64

+   打印 int64 的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a int64
    var a int64 = 2

    //Size of int64 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
8 bytes
a's type is int64
```

**无符号**

无符号整数有 5 种类型，如下所示

| **类型** | **大小** |
| --- | --- |
| uint | 平台相关 |
| uint8 | 8 位/1 字节 |
| uint16 | 16 位/2 字节 |
| uint32 | 32 位/4 字节 |
| uint64 | 64 位/8 字节 |

**uint**

**大小：** 平台相关。

+   在 32 位机器上，int 的大小将是 32 位或 4 字节。

+   在 64 位机器上，int 的大小将是 64 位或 8 字节

**范围**：再次平台相关

+   在 32 位机器上，int 的范围为 -2^(31) 到 2^(31) - 1。

+   在64位机器上，int的范围将是 -2^(63) 到 2^(63) -1。

**何时使用：**

+   在使用其他有符号整数的情况下，建议使用uint。

    +   当机器为32位且所需范围大于 -231 到 231 -1 时，请使用int64而不是int。请注意，在这种情况下，两个32位内存地址合成一个64位数字。

    +   当范围较小时，请使用适当的int类型。

**属性：**

+   声明一个uint。

```go
 var a uint
```

+   golang的**bits**包可以帮助了解系统上**uint**的大小。

```go
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64
sizeOfUintInBits := bits.UintSize
fmt.Println(sizeOfIntInBits) => 32 or 64
```

+   **unsafe.Sizeof()** 函数也可用于查看uint的字节大小。

**完整工作代码**

以下是上述属性的完整工作代码。

```go
package main

import (
    "fmt"
    "math/bits"
    "reflect"
    "unsafe"
)

func main() {
    //This is computed as const uintSize = 32 << (^uuint(0) >> 32 & 1) // 32 or 64
    sizeOfuintInBits := bits.UintSize
    fmt.Printf("%d bits\n", sizeOfuintInBits)

    var a uint
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
64 bits
8 bytes
a's type is uint
```

**uintptr**

这是一种无符号整数类型，足够大以容纳任何指针地址。因此，其大小和范围依赖于平台。

**大小：** 依赖于平台。

+   在32位机器上，int的大小将为32位或4字节。

+   在64位机器上，int的大小将为64位或8字节。

**范围**：再次依赖于平台。

+   在32位机器上，int的范围将是 -2^(31) 到 2^(31) -1。

+   在64位机器上，int的范围将是 -2^(63) 到 2^(63) -1。

**属性：**

+   uintptr可以转换为**unsafe.Pointer**，反之亦然。

+   可以对uintptr执行算术运算。

+   uintptr虽然存储指针地址，但只是一个值，不引用任何对象。因此。

    +   如果相应对象移动，其值将不会更新。例如，当goroutine栈变化时。

    +   相应对象可以被垃圾收集。

****何时使用：****

***其目的主要是与unsafe.Pointer一起使用，以便进行不安全的内存访问。

+   当你想保存指针地址值以进行打印或存储时。由于地址只是被存储并不引用任何东西，相应的对象可以被垃圾收集。

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

**uint8**

**大小：** 8位或1字节。

**范围**：0 到 255 或 0 到 2⁸ -1。

**何时使用：**

+   当已知int范围将在 2⁸ -1之间时，使用uint8。对于临时值，如循环不变量，尽管可能占用更多空间，但仍建议使用int，因为在某些操作或库调用中可能会提升为int。

+   对于位于 2⁸ -1 之间的数组值，使用uint8是一个不错的用例。例如，如果你在数组中存储ASCII索引，则可以使用**uint8**。

**示例：**

以下代码示例说明了以下几点。

+   声明一个uint8。

+   打印uint8的字节大小。

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a uint8

    var a uint8 = 2

    //Size of uint8 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
1 bytes
a's type is uint8
```

**uint16**

**大小：** 16位或2字节。

**范围**：0 到 2^(16) -1。

**何时使用：**

+   当已知int范围将在0到2^(16) -1之间时，使用int16。对于临时值，如循环不变量，尽管可能占用更多空间，但仍建议使用int，因为在某些操作或库调用中可能会提升为int。

+   对于位于 -0 到 2^(16) -1 之间的数组值，使用int8是一个不错的用例。

**示例：**

以下代码示例说明了以下要点

+   声明一个uint16

+   打印uint16的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    //Declare a uint16
    var a uint16 = 2

    //Size of uint16 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
2 bytes
a's type is uint16
```

**uint32**

**大小：** 32位或4字节

**范围**：0到2^(32) -1

**示例：**

以下代码示例说明了以下要点

+   声明一个uint32

+   打印uint32的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a uint32
    var a uint32 = 2

    //Size of uint32 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
1 bytes
a's type is uint32
```

**uint64**

**大小：** 64位或8字节

**范围**：0到2^(64) -1

**使用时机：**

+   uint64在范围更高时使用。

**示例：**

以下代码示例说明了以下要点

+   声明一个uint64

+   打印uint64的字节大小

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a uint64
    var a uint64 = 2

//Size of uint64 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
8 bytes
a's type is uint64
```

## **浮点数**

浮点数是带小数的数字。它有两种类型

| **类型** | **大小** |
| --- | --- |
| float32 | 32位或4字节 |
| float64 | 64位或8字节 |

**float64**是默认的浮点类型。当你用小数值初始化变量而不指定浮点类型时，推断的默认类型将是**float64**。

**float32**

**float32**使用单精度浮点格式存储值。基本上，它是所有IEEE-754 32位浮点数的集合。32位被划分为– 1位符号位，8位指数，和23位尾数。float32占用的空间是float64的一半，在某些机器架构上速度相对较快。

**大小**：32位或4字节

**范围**：1.2E-38到3.4E+38

**默认值**：0.0

**使用时机：**

+   如果系统内存是瓶颈且范围较小，则可以使用**float32**。

**示例：**

以下代码示例说明了以下要点

+   声明一个float32

+   打印float32的字节大小

**代码：**

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a float32
    var a float32 = 2

    //Size of float32 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))
}
```

**输出：**

```go
4 bytes
a's type is float32
```

**float64**

float64使用双精度浮点格式存储值。基本上，它是所有IEEE-754 64位浮点数的集合。64位被划分为– 1位符号位，11位指数，52位尾数。float64占用的空间是float32的两倍，但可以比float32更准确地表示数字。

**大小**：32位或4字节

**范围**：1.2E-38到3.4E+38

**默认值**：0.0

**使用时机：**

+   当需要高精度时

**示例：**

以下代码示例说明了以下要点

+   声明一个float64

+   打印float64的字节大小

+   当你不指定类型时，默认是float64

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    //Declare a float64
    var a float64 = 2

    //Size of float64 in bytes
    fmt.Printf("%d bytes\n", unsafe.Sizeof(a))
    fmt.Printf("a's type is %s\n", reflect.TypeOf(a))

    //Default is float64 when you don't specify a type
    b := 2.3
    fmt.Printf("b's type is %s\n", reflect.TypeOf(b))
}
```

**输出：**

```go
8 bytes
a's type is float64
b's type is float64
```

## **复数**

复数有两种类型

| **类型** | **属性** |
| --- | --- |
| complex64 | 实部和虚部都是float32 |
| complex128 | 实部和虚部都是float64 |

默认的复数类型是complex128。

**初始化**

复数可以通过两种方式初始化

+   使用复数函数。它具有以下签名。请确保a和b的类型相同，意味着它们要么都是float32，要么都是float64。

```go
complext(a, b)
```

+   使用简写语法。这在使用直接数字创建复数时使用。如果未指定类型，则使用下面的方法创建的复数类型将为**complex128**。

```go
a := 5 + 6i
```

**complex64**

对于complex 64，实部和虚部都是float32

**大小**：实部和虚部的大小与 float32 相同。大小为 32 位或 4 字节

**范围**：实部和虚部的范围与 float32 相同，即 1.2E-38 到 3.4E+38

**示例**

以下是显示的示例代码

+   如何使用上述两种方法创建 complex64 数字

+   打印 complex64 数字的大小。大小将为 8 字节（4 + 4），相当于两个 float32 数字

+   打印 complex64 数字的类型

+   + 操作符在复数上

**代码：**

```go
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    var a float32 = 3
    var b float32 = 5

    //Initialize-1
    c := complex(a, b)

    //Initialize-2
    var d complex64
    d = 4 + 5i

    //Print Size
    fmt.Printf("c's size is %d bytes\n", unsafe.Sizeof(c))
    fmt.Printf("d's size is %d bytes\n", unsafe.Sizeof(d))

    //Print type
    fmt.Printf("c's type is %s\n", reflect.TypeOf(c))
    fmt.Printf("d's type is %s\n", reflect.TypeOf(d))

    //Operations on complex number
    fmt.Println(c+d, c-d, c*d, c/d)
}
```

**输出：**

```go
c's size is 8 bytes
d's size is 8 bytes
c's type is complex64
d's type is complex64
(7+10i) (-1+0i) (-13+35i) (0.902439+0.12195122i)
```

**complex128**

对于 complex128，实部和虚部都是 **float64**

**大小**：实部和虚部的大小与 float64 相同。大小为 64 位或 8 字节

**范围**：实部和虚部的范围与 **float64** 相同，即 -1.7E+308 到 +1.7E+308

**示例**

以下是显示的示例代码

+   如何使用上述两种方法创建 complex128 数字。它还显示当类型未指定时，默认类型将为 **complex128**

+   打印 complex128 数字的大小。大小将为 16 字节（8 + 8），相当于两个 float64 数字

+   打印 complex128 数字的类型

+   对复数进行不同操作

**代码：**

```go
package main

import (
    "fmt"
    "reflect"
    "unsafe"
)

func main() {
    var a float64 = 3
    var b float64 = 5

    //Initialize-1
    c := complex(a, b)

    //Initialize-2\. When don't specify a type , the default type will be complex128
    d := 4 + 5i

    //Print Size
    fmt.Printf("c's size is %d bytes\n", unsafe.Sizeof(c))
    fmt.Printf("d's size is %d bytes\n", unsafe.Sizeof(d))

    //Print type
    fmt.Printf("c's type is %s\n", reflect.TypeOf(c))
    fmt.Printf("d's type is %s\n", reflect.TypeOf(d))

    //Operations on complex number
    fmt.Println(c+d, c-d, c*d, c/d)
}
```

**输出：**

```go
c's size is 16 bytes
d's size is 16 bytes
c's type is complex128
d's type is complex128
(7+10i) (-1+0i) (-13+35i) (0.902439024390244+0.12195121951219513i)
```

## **字节**

Go 中的字节是 **uint8** 的别名，意味着它是一个整数值。这个整数值为 8 位，表示一个字节，即 0-255 之间的数字。因此，一个字节可以表示 ASCII 字符。Golang 没有 ‘char’ 的数据类型。因此

+   字节用于表示 ASCII 字符

+   rune 用于表示所有 Unicode 字符，包括所有存在的字符。我们将在本教程后面学习 rune。

**定义字节**

```go
var rbyte byte := 'a'
```

在声明字节时，我们必须指定类型，如上面的程序所示。如果不指定类型，则默认类型被视为 **rune。**

**示例**

在下面的代码示例中：

+   如何定义字节

+   打印字节类型

+   打印字节大小

```go
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    var r byte = 'a'

    //Print Size
    fmt.Printf("Size: %d\n", unsafe.Sizeof(r))

    //Print Type
    fmt.Printf("Type: %s\n", reflect.TypeOf(r))

    //Print Character
    fmt.Printf("Character: %c\n", r)
    s := "abc"

    //This will the decimal value of byte
    fmt.Println([]byte(s))
}
```

**输出：**

```go
Size: 1
Type: uint8
Character: a
[97 98 99]
```

## **符文**

Go 中的 rune 是 **int32** 的别名，意味着它是一个整数值。这个整数值用来表示 Unicode 代码点。要理解 rune，您必须了解 Unicode。下面是简短的描述，您可以参考著名的博客文章 – [每个软件开发人员绝对必须了解的 Unicode 和字符集的绝对最小知识（没有借口！）](http://www.joelonsoftware.com/articles/Unicode.html)

**什么是 Unicode**

Unicode 是 ASCII 字符的超集，它为每个存在的字符分配一个唯一的数字。这个唯一的数字称为 Unicode 代码点。

例如

+   数字 **0** 表示为 Unicode 点 **U+0030 (十进制值 – 48)**

+   小写 **b** 表示为 Unicode 点 **U+0062 (十进制值 – 98)**

+   英镑符号 **£** 表示为 Unicode 点 **U+00A3 (十进制值 – 163)**

访问 [https://en.wikipedia.org/wiki/List_of_Unicode_characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) 了解其他字符的 Unicode 点。但是 Unicode 不讨论这些代码点如何保存在内存中。这就是 **utf-8** 进入的地方。

**UTF-8**

utf-8 使用 1、2、3 或 4 个字节保存每个 Unicode 点。ASCII 点使用 1 个字节存储。这就是为什么 rune 是 int32 的别名，因为在 Go 中，Unicode 点的最大字节数为 4，而 Go 中的每个字符串都是使用 utf-8 编码的。

每个 rune 旨在指代一个 Unicode 点。例如，如果你在将字符串强制转换为 rune 数组后打印它，则会打印每个字符的 Unicode 点。对于下面的字符串 **“0b£”**，输出将是 – **[U+0030 U+0062 U+00A3]**。

```go
fmt.Printf("%U\n", []rune("0b£"))
```

**声明 Rune**

一个 rune 是通过在单引号中声明的字符，例如声明一个名为 **‘rPound’** 的变量。

```go
rPound := '£'
```

声明 Rune 后，你还可以执行以下操作。

+   **打印类型 –** 输出将是 **int32**。

```go
fmt.Printf("Type: %s\n", reflect.TypeOf(rPound))
```

+   **打印 Unicode 代码点 –** 输出将是 **U+00A3**。

```go
fmt.Printf("Unicode CodePoint: %U\n", rPound)
```

+   **打印字符 –** 输出将是 **£**。

```go
fmt.Printf("Character: %c\n", r)
```

**何时使用**

当你打算在值中保存 Unicode 代码点时，应使用一个 rune。当数组中的所有值都旨在表示 Unicode 代码点时，应使用 rune 数组。

**代码：**

以下是说明我们讨论的每一点的代码。

```go
package main
import (
    "fmt"
    "reflect"
    "unsafe"
)
func main() {
    r := 'a'

    //Print Size
    fmt.Printf("Size: %d\n", unsafe.Sizeof(r))

    //Print Type
    fmt.Printf("Type: %s\n", reflect.TypeOf(r))

    //Print Code Point
    fmt.Printf("Unicode CodePoint: %U\n", r)

    //Print Character
    fmt.Printf("Character: %c\n", r)
    s := "0b£"

    //This will print the Unicode Points
    fmt.Printf("%U\n", []rune(s))

    //This will the decimal value of Unicode Code Point
    fmt.Println([]rune(s))
}
```

**输出：**

```go
Size: 4
Type: int32
Unicode CodePoint: U+0061
Character: a
[U+0030 U+0062 U+00A3]
[48 98 163]
```

## **字符串**

字符串是 golang 中只读的字节切片。字符串可以通过两种方式初始化。

+   使用双引号 “” 例如 “this”。

双引号中的字符串会遵循转义序列。例如，如果字符串包含 \n，那么在打印时会产生新的一行。

+   使用反引号 ` 例如 \`this`。

反引号中的字符串只是一个原始字符串，不会遵循任何类型的转义序列。

字符串中的每个字符将根据使用的编码占用一些字节。例如，在 utf-8 编码的字符串中，每个字符将占用 1 到 4 个字节。你可以阅读关于 utf-8 的必读博客——[每个软件开发者必须了解的 Unicode 和字符集的绝对最小知识（没有借口！）](http://www.joelonsoftware.com/articles/Unicode.html)。在 utf-8 中，字符 **a** 或 **b** 使用 1 个字节编码，而字符英镑符号 **£** 使用 2 个字节编码。因此，字符串 “ab£” 在转换为字节数组并打印时将输出 4 个字节。

```go
s := "ab£"
fmt.Println([]byte(s))
```

**输出**

```go
[48 98 194 163]
```

当你尝试使用 **len(“ab£”)** 打印上述字符串的长度时，它将输出 4 而不是 3，因为它包含 4 个字节。

还要注意，**range** 循环遍历每个字符形成的字节序列，因此对于下面的 range 循环。

```go
for _, c := range s {
   fmt.Println(string(c))
}
```

输出将是

```go
a
b
£
```

可以对字符串执行许多操作。其中一种操作是连接，它将两个字符串合并。连接使用符号 ‘+’。让我们看看我们讨论的所有内容的完整工作代码。

**代码：**

```go
package main
import (
    "fmt"
)
func main() {
    //String in double quotes
    x := "this\nthat"
    fmt.Printf("x is: %s\n", x)

    //String in back quotes
    y := `this\nthat`
    fmt.Printf("y is: %s\n", y)
    s := "ab£"

    //This will print the byte sequence. 
    //Since character a and b occupies 1 byte each and £ character occupies 2 bytes. 
    //The final output will 4 bytes
    fmt.Println([]byte(s))

    //The output will be 4 for same reason as above
    fmt.Println(len(s))

    //range loops over sequences of byte which form each character
    for _, c := range s {
        fmt.Println(string(c))
    }

    //Concatenation
    fmt.Println("c" + "d")
}
```

**输出：**

```go
x is: this
that
y is: this\nthat
[97 98 194 163]
4
a
b
£
cd
```

## **布尔值**

数据类型是**bool**，有两个可能的值：true 或 false。

默认值：false。

操作：

+   和 – &&

+   或 – ||

+   取反 – !

**示例**

以下代码示例显示了。

+   如果未初始化，默认值为**false**。

+   所有上述操作在 bool 上。

**代码**

```go
package main

import "fmt"

func main() {
    //Default value will be false it not initialized
    var a bool
    fmt.Printf("a's value is %t\n", a)

    //And operation on one true and other false
    andOperation := 1 < 2 && 1 > 3
    fmt.Printf("Ouput of AND operation on one true and other false %t\n", andOperation)

    //OR operation on one true and other false
    orOperation := 1 < 2 || 1 > 3
    fmt.Printf("Ouput of OR operation on one true and other false: %t\n", orOperation)

    //Negation Operation on a false value
    negationOperation := !(1 > 2)
    fmt.Printf("Ouput of NEGATION operation on false value: %t\n", negationOperation)
}
```

**输出：**

```go
a's value is false
Ouput of AND operation on one true and other false false
Ouput of OR operation on one true and other false: true
Ouput of NEGATION operation on false value: true
```

# **复合类型**

## **非引用类型**

### **数组**

Go 中的数组是值。它们是固定长度的同类型序列。由于 Go 中的数组是值，这就是原因。

+   当你将一个数组赋值给另一个变量时，它会复制整个数组。

+   当你将数组作为参数传递给一个函数时，它会复制整个数组，而不仅仅是传递地址。

数组的声明如下。假设 N 是数组的大小。

+   同时指定大小和数值。

```go
newArray := [n]Type{val1, val2, val3}
```

+   指定大小 – 无数值。值被设置为该类型的默认零值。

```go
newArray := [len]Type{}
```

如我们所说，数组的长度是固定的。因此。

+   我们不能将一个数组赋值给同类型但长度不同的另一个数组。

+   当你将数组作为函数参数传递时，大小也是其中的一部分。

让我们来看一个数组的例子。以下示例。

+   演示如何声明一个数组。

+   将数组作为函数参数传递。

```go
package main

import "fmt"

func main() {
    //Declare a array
    sample := [3]string{"a", "b", "c"}
    print(sample)
}

func print(sample [3]string) {
    fmt.Println(sample)
}
```

**输出：**

```go
[a b c]
```

### **结构体**

在 Go 中，结构体是字段的集合。这些字段可以是不同类型的。结构体作为异构数据类型相关数据的容器。例如，不同的属性用来表示组织中的员工。员工可以有。

+   字符串类型的名称。

+   整数类型的年龄。

+   时间类型的出生日期（DOB）。

..等等。结构体可以用来表示一个员工。

```go
type employee struct {
    name string
    age  int
    dob  time.Time
}
```

以下程序描述了。

+   声明一个结构体。

+   以不同方式初始化结构体。

+   结构体的大小是其字段大小的总和。

```go
package main
import (
    "fmt"
)
//Declare a struct
type employee struct {
    name   string
    age    int
    salary float64
}
func main() {
    //Initialize a struct without named fields
    employee1 := employee{"John", 21, 1000}
    fmt.Println(employee1)

    //Initialize a struct with named fields
    employee2 := employee{
        name:   "Sam",
        age:    22,
        salary: 1100,
    }
    fmt.Println(employee2)

    //Initializing only some fields. Other values are initialized to default zero value of that type
    employee3 := employee{name: "Tina", age: 24}
    fmt.Println(employee3)
}
```

**输出：**

```go
{John 21 1000}
{Sam 22 1100}
{Tina 24 0}
```

## **引用类型**

### **切片**

切片的大小是动态的，引用数组的元素。如上所述，数组是固定大小的，因此切片为数组提供了更灵活的接口。切片是一种引用类型，因为它内部引用一个数组。它内部由三个字段表示。

+   指向基础数组的地址。

+   切片的长度。

+   切片的容量。

切片的类型由基础数组元素的类型决定，而不是由其长度或容量决定。因此，如果基础数组的类型相同，不论其长度和容量如何，两个切片将具有相同的类型。内置的**append**函数可以用来向基础数组添加更多值。如果在使用**append**函数时切片的长度超过当前容量，则分配一个新的切片，容量为当前容量的两倍，当前切片的元素被复制到该新切片中。

内置函数**len**可以用来获取当前切片的长度，初始化切片。

+   使用 make – 它帮助你创建一个切片，指定数组的类型、长度和容量。指定长度和容量是可选的。如果指定了长度而未指定容量，则容量将等于长度。

```go
make([]TYPE, length, capacity)
```

+   直接初始化。以下示例创建一个字符串的切片。

```go
p := []string{"a", "b", "c"}
```

切片也可以从数组或其他切片创建。

下面是一个程序，展示了一个切片的示例

+   使用上述方法声明一个切片

+   显示追加函数

+   如何遍历一个切片

```go
package main

import "fmt"

func main() {
    //Declare a slice using make
    s := make([]string, 2, 3)
    fmt.Println(s)

    //Direct intialization
    p := []string{"a", "b", "c"}
    fmt.Println(p)

    //Append function
    p = append(p, "d")
    fmt.Println(p)

    //Iterate over a slcie
    for _, val := range p {
        fmt.Println(val)
    }
}
```

**输出：**

```go
[ ]
[a b c]
[a b c d]
a
b
c
d
```

### **通道**

通道提供了goroutine之间的同步和通信。你可以将其视为一个管道，通过这个管道，goroutine可以发送和接收值。操作符<-用于发送或接收，箭头的方向指定了数据流的方向。

```go
ch <- val    //Sending a value present in var variable to channel
val := <-cha  //Receive a value from  the channel and assign it to val variable
```

通道有两种类型

+   **无缓冲通道** - 它没有任何容量来存储值，因此

    +   在通道上发送操作会阻塞，除非有另一个goroutine来接收。

    +   接收在另一侧有另一个goroutine发送之前会阻塞。

+   **缓冲通道** - 你可以在这里指定缓冲区的大小

    +   仅当缓冲区满时，发送到缓冲通道才会阻塞

    +   当通道的缓冲区为空时，接收是唯一会阻塞的操作

一个通道一次只能持有特定类型的数据。在创建通道时，必须在初始化新通道时指定数据类型。在下面的示例中，我们创建了一个持有字符串类型数据的通道。

```go
events := make(chan string)  //Unbuffered channel
events2 := make(chan string, 2)  //Buffered channel of length 2
```

关闭通道

**close()** 函数可用于关闭通道。关闭通道意味着不能再向通道发送值

让我们看一个同时使用缓冲和无缓冲通道的工作代码示例

**缓冲通道示例：**

```go
package main

import "fmt"

func main() {
    //Creating a buffered channel of length 3
    eventsChan := make(chan string, 3)
    eventsChan <- "a"
    eventsChan <- "b"
    eventsChan <- "c"
    //Closing the channel
    close(eventsChan)
    for event := range eventsChan {
        fmt.Println(event)
    }
}
```

**输出：**

```go
a
b
c
```

**无缓冲通道示例：**

```go
package main

import "fmt"

func main() {
    eventsChan := make(chan string)
    go sendEvents(eventsChan)
    for event := range eventsChan {
        fmt.Println(event)
    }
}

func sendEvents(eventsChan chan<- string) {
    eventsChan <- "a"
    eventsChan <- "b"
    eventsChan <- "c"
    close(eventsChan)
}
```

**输出：**

```go
a
b
c
```

### **映射**

映射是Go语言内置的数据类型，类似于哈希，映射键到值。映射是引用数据类型。当你将一个映射赋值给另一个映射时，两个映射都引用同一个底层映射。

**零值**

映射的零值是nil

**声明**

+   可以使用var关键字声明一个映射，并指定其键和值的类型。例如，下面的映射声明了一个名称为

```go
var employeeSalary map[string]int
```

**初始化**

+   使用make

```go
var employeeSalary make(map[string]int)
```

+   使用大括号。你可以在映射中指定映射字面量值，也可以留空的大括号

```go
//Empty braces
employeeSalary := map[string]int{}

//Specify values
employeeSalary := map[string]int{
"John": 1000
"Sam": 2000
} 
```

**操作**

+   添加到一个映射

```go
employeeSalary["John"] = 1000
```

+   从映射中获取

```go
salary := employeeSalary["John"]
```

+   从映射中删除一个键

```go
delete(employeeSalary, "John")
```

**示例**

下面的示例展示了我们讨论过的所有要点

```go
package main

import "fmt"

func main() {
    //Declare
    var employeeSalary map[string]int
    fmt.Println(employeeSalary)

    //Intialize using make
    employeeSalary2 := make(map[string]int)
    fmt.Println(employeeSalary2)

    //Intialize using map lieteral
    employeeSalary3 := map[string]int{
        "John": 1000,
        "Sam":  1200,
    }
    fmt.Println(employeeSalary3)

    //Operations
    //Add
    employeeSalary3["Carl"] = 1500

    //Get
    fmt.Printf("John salary is %d\n", employeeSalary3["John"])

    //Delete
    delete(employeeSalary3, "Carl")

    //Print map
    fmt.Println("\nPrinting employeeSalary3 map")
    fmt.Println(employeeSalary3)
}
```

**输出**

```go
map[]
map[]
map[John:1000 Sam:1200]
John salary is 1000

Printing employeeSalary3 map
map[John:1000 Sam:1200]
```

### **指针**

指针是一个变量，它保存另一个变量的内存地址。指针的零值是nil。

**声明一个指针**

在下面的示例中，ex是int指针。

```go
var ex *int
```

**初始化**

**&** 用于获取变量的地址

```go
a := 2
b := &b
```

* 运算符可用于解引用指针，这意味着获取存储在指针地址中的值。

```go
fmt.Println(*b) //Print the value stored at address b 
```

指针也可以使用 **new** 运算符初始化

```go
a := new(int)
*a = 10
fmt.Println(*a) //Output will be 10
```

让我们来看一个涵盖上述所有要点的工作代码

```go
package main

import "fmt"

func main() {
    //Declare
    var b *int
    a := 2
    b = &a

    //Will print a address. Output will be different everytime.
    fmt.Println(b)
    fmt.Println(*b)
    b = new(int)
    *b = 10
    fmt.Println(*b) 
}
```

**输出：**

```go
0xc000018080
2
10
```

### **函数**

在Go中，函数是值，可以像值一样传递。基本上，函数可以作为一等对象使用并被传递。函数的签名是

```go
func some_func_name(arguments) return_values
```

函数有一个名称、参数和返回值。此外，请注意 Go 中方法和函数之间的一些重要区别。让我们看看一个方法的签名。

**方法：**

```go
func (receiver receiver_type) some_func_name(arguments) return_values
```

从上述签名可以看出，该方法有一个接收者参数。接收者可以是一个结构体或其他任何类型。该方法将访问接收者的属性，并可以调用接收者的其他方法。

下面是一个函数的工作示例。

```go
package main

import "fmt"

func main() {
    add := func(x, y int) int {
        return x + y
    }
    fmt.Println(add(1, 2))
}

func doOperation(fn func(int, int) int, x, y int) int {
    return fn(x, y)
}
```

**输出：**

```go
3
```

## **接口**

接口是 Go 中的一种类型，是方法签名的集合。任何实现了接口所有方法的类型都属于该接口类型。接口的零值是 nil。

**接口的签名**

```go
type name_of_interface interface{
//Method signature 1
//Method signature 2
}
```

**接口是隐式实现的**

类型实现接口没有明确的声明。事实上，在 Go 中并没有类似于 Java 的 **“implements”** 关键字。如果一个类型实现了接口的所有方法，那么它就实现了该接口。

定义一个接口类型的变量是正确的，如果具体类型实现了接口的所有方法，我们可以将任何具体类型的值分配给该变量。让我们来看一个接口的工作示例。在下面的程序中。

+   我们声明一个名为 shape 的接口，其中包含一个方法 **area**。

+   square 结构体实现了 area 方法，因此它隐式实现了 shape 接口。

+   我们声明一个名为 "s" 的 shape 类型变量。

+   s 被分配了具体类型 square 的值。这是可行的，因为 square 结构体实现了 shape 接口的所有方法。

```go
package main

import "fmt"

type shape interface {
    area() int
}

type square struct {
    side int
}

func (s *square) area() int {
    return s.side * s.side
}

func main() {
    var s shape
    s = &square{side: 4}
    fmt.Println(s.area())
}
```

**输出：**

```go
16
```

### **空接口的特殊情况**

一个空接口没有方法，因此默认情况下所有具体类型都实现了空接口。如果您编写一个接受空接口的函数，那么您可以将任何类型传递给该函数。请参见下面的工作代码：

```go
package main

import "fmt"

func main() {
    test("thisisstring")
    test("10")
    test(true)
}

func test(a interface{}) {
    fmt.Printf("(%v, %T)\n", a, a)
}
```

**输出：**

```go
(thisisstring, string)
(10, string)
(true, bool)
```

# **结论**

这就是 Go 中存在的内置数据类型。希望通过阅读本文，您能更好地理解 Go 中的数据类型。

+   [侧边目录](https://golangbyexample.com/tag/sidetoc/)***
