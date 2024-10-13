<!--yml

分类：未分类

日期：2024-10-13 06:30:58

-->

# Go（Golang）中的所有基本数据类型

> 来源：[`golangbyexample.com/all-basic-data-types-golang/`](https://golangbyexample.com/all-basic-data-types-golang/)

这是 Golang 综合教程系列的第七章。有关该系列其他章节的信息，请参阅此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一教程** – [函数](https://golangbyexample.com/function-golang-complete-guide/)

**前一教程** –[变量](https://golangbyexample.com/variables-in-golang-complete-guide/)

现在让我们查看当前教程。以下是当前教程的目录。

目录

**概述**

+   基本类型

    +   整数

        +   有符号整数

        +   无符号

    +   浮点数

    +   复数

    +   字节

    +   字符

    +   字符串

    +   布尔值

+   结论*  *# **概述**

Golang 是一种静态类型的编程语言，意味着每个变量都有一个类型。Go 有几种内置类型。Go 中的数据类型可以分为两种类型。

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

    +   字符

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

在本文中，我们将仅讨论基本类型。

## **整数**

整数可以是有符号或无符号。

### **有符号整数**

有符号整数有 5 种类型，如下所示

| **类型** | **大小** |
| --- | --- |
| int | 依赖于平台 |
| int8 | 8 位/1 字节 |
| int16 | 16 位/2 字节 |
| int32 | 32 位/4 字节 |
| int64 | 64 位/8 字节 |

**int**

**大小：** 依赖于平台。

+   在 32 位机器上，int 的大小将为 32 位或 4 字节。

+   在 64 位机器上，int 的大小将为 64 位或 8 字节。

**范围**：再次取决于平台

+   在 32 位机器上，int 的大小将为 32 位或 4 字节。

+   在 64 位机器上，int 的大小将为 64 位或 8 字节。

**使用时机：**

+   在使用有符号整数时，建议使用 int，除了下面提到的情况。

    +   当机器是 32 位且所需范围大于-231 到 231-1 时，则使用 int64 而不是 int。请注意，在这种情况下，int64 需要两个 32 位内存地址来共同形成一个 64 位数字。

    +   当范围较小时，使用适当的整数类型。

**属性：**

+   声明一个 int

```go
var a int
```

+   int 是**整数**的默认类型。当你没有指定类型时，默认类型为 int。

```go
b := 2 //The default is also intfmt.Println(reflect.TypeOf(b)) => int
```

+   **bits**包可以帮助了解系统上**int**的大小

```go
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64sizeOfIntInBits := bits.UintSizefmt.Println(sizeOfIntInBits) => 32 0r 34
```

+   **unsafe.Sizeof()** 函数也可以用于查看 int 的字节大小。

**完整工作代码**

下面是上述属性的完整工作代码

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

**范围**： -2⁷ 到 2⁷ - 1。

**何时使用：**

+   当已知整数范围在 -2⁷ 到 2⁷ - 1 之间时，使用 int8。对于临时值，如循环不变量，尽管可能占用更多空间，仍建议使用 int，因为在某些操作或库调用中它可能会被提升为 int。

+   对于范围在 -27 到 27 - 1 之间的数组值，使用 int8 是一个好的用例。例如，如果你要存储小写字母的 ASCII 索引，则可以使用 int8。

+   对于数据值，使用 int8 是个好主意。

**int16**

**大小：** 16 位或 2 字节

**范围**： -2¹⁵ 到 2¹⁵ - 1。**何时使用：**

+   当已知整数范围在 -2¹⁵ 到 2¹⁵ - 1 之间时，使用 int16。对于临时值，如循环不变量，尽管可能占用更多空间，仍建议使用 int，因为在某些操作或库调用中它可能会被提升为 int。

+   对于范围在 -215 到 215 - 1 之间的数组值，使用 int8 是一个好的用例。例如，如果你要存储小写字母的 ASCII 索引，则可以使用 int16。

**int32**

**大小：** 32 位或 4 字节

**范围**： -2³¹ 到 2³¹ - 1。

**int64**

**大小：** 64 位或 8 字节

**范围**： -2⁶³ 到 2⁶³ - 1。**何时使用：**

+   **int64** 在范围更高时使用。例如，**time.Duration** 是 **int64** 类型。

### **无符号**

无符号整数有 5 种类型，如下所示。

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

+   在 64 位机器上，int 的大小将是 64 位或 8 字节。

**范围**：再次平台相关。

+   在 32 位机器上，int 的范围将是 -2³¹ 到 2³¹ - 1。

+   在 64 位机器上，int 的范围将是 -2⁶³ 到 2⁶³ - 1

**何时使用：**

+   每当使用带符号整数时，除了以下提到的情况，使用 uint 是个好主意。

    +   当机器为 32 位且所需范围大于 -2³¹ 到 2³¹ - 1 时，使用 **int64** 而不是 **int**。请注意，在这种情况下，int64 由 2 个 32 位内存地址组合而成一个 64 位数字。

    +   当范围较小时，使用适当的 int 类型。

**属性：**

+   声明一个 uint

```go
var a uint
```

+   golang 的 **bits** 包可以帮助了解系统中 **uint** 的大小。

```go
//This is computed as const uintSize = 32 << (^uint(0) >> 32 & 1) // 32 or 64sizeOfUintInBits := bits.UintSizefmt.Println(sizeOfIntInBits) => 32 or 64
```

+   **unsafe.Sizeof()** 函数也可以用于查看 uint 的字节大小。

**完整工作代码**

下面是上述属性的完整工作代码

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

这是一种无符号整数类型，足够大以容纳任何指针地址。因此其大小和范围与平台相关。

**大小：** 平台相关。

+   在 32 位机器上，int 的大小将是 32 位或 4 字节。

+   在 64 位机器上，int 的大小将是 64 位或 8 字节。

**范围**：再次取决于平台

+   在 32 位机器上，int 的范围将是 -2³¹ 到 2³¹ - 1。

+   在 64 位机器上，int 的范围将是 -2⁶³ 到 2⁶³ - 1。

**属性：**

+   uintptr 可以转换为 unsafe.Pointer，反之亦然。

+   可以对 uintptr 执行算术运算

+   uintptr 尽管它持有指针地址，但仅仅是一个值，并不引用任何对象。因此

    +   如果相应的对象移动，其值不会被更新。例如，当 goroutine 堆栈改变时。

    +   相应的对象可以被垃圾回收。

****何时使用：****

*** 其目的主要是与 unsafe.Pointer 一起使用，用于不安全的内存访问。

+   当你想保存指针地址值以进行打印或存储时。由于地址仅被存储并不引用任何对象，相应的对象可以被垃圾回收。

**完整工作代码**

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

**输出**

```go
test
```

**uint8**

**大小：** 8 位或 1 字节

**范围**：0 到 255 或 0 到 2⁸ - 1。**何时使用：**

+   当已知 int 的范围将介于 2⁸ - 1 之间时使用 uint8。对于临时值，如循环不变量，尽管可能占用更多空间，仍建议使用 int，因为它在某些操作或库调用中可能会提升为 int。

+   对于值在 2⁸ - 1 之间的数组，使用 uint8 是一个不错的选择。例如，如果你在数组中存储 ASCII 索引，则可以使用 **uint8**。

**uint16**

**大小：** 16 位或 2 字节

**范围**：0 到 2¹⁶ - 1。**何时使用：**

+   当已知 int 的范围将介于 0 到 2¹⁶ - 1 之间时使用 int16。对于临时值，如循环不变量，尽管可能占用更多空间，仍建议使用 int，因为它在某些操作或库调用中可能会提升为 int。

+   对于值在 -0 到 2¹⁶ - 1 之间的数组，使用 int8 是一个不错的选择。

**uint32**

**大小：** 32 位或 4 字节

**范围**：0 到 2³² - 1

**uint64**

**大小：** 64 位或 8 字节

**范围**：0 到 2⁶⁴ - 1。**何时使用：**

+   当范围较高时使用 uint64。

## **浮点数**

浮点数是带有小数的数字。它有两种类型。

| **类型** | **大小** |
| --- | --- |
| float32 | 32 位或 4 字节 |
| float64 | 64 位或 8 字节 |

**float64** 是默认的浮点类型。当你用小数值初始化一个变量且未指定浮点类型时，推断出的默认类型将是 **float64**。

**float32**

**float32** 使用单精度浮点格式来存储值。基本上，它是所有 IEEE-754 32 位浮点数的集合。32 位被分为：1 位符号位、8 位指数位和 23 位尾数。float 32 的大小是 float 64 的一半，并且在某些机器架构上速度相对更快。

**大小**：32 位或 4 字节

**范围**：1.2E-38 到 3.4E+38

**默认值**：0.0

**何时使用：**

如果在你的系统中内存是瓶颈且范围较小，则可以使用 **float32**。

**示例：**

以下代码示例说明了以下几点。

+   声明一个 float32。

+   打印 float32 的大小（字节）。

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

float64 使用双精度浮点格式来存储值。基本上它是所有 IEEE-754 64 位浮点数的集合。这 64 位分为 1 位符号、11 位指数和 52 位尾数。float64 的大小是 float32 的两倍，但能比 float32 更准确地表示数字。

**大小**：32 位或 4 字节。

**范围**：1.2E-38 到 3.4E+38。

**默认值**：0.0。

**何时使用：**

当所需精度很高时。

**示例：**

以下代码示例说明了以下几点。

+   声明一个 float64。

+   打印 float64 的大小（字节）。

+   默认是 float64，当你不指定类型时。

**代码：**

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

复数有两种类型。

| **类型** | **属性** |
| --- | --- |
| complex64 | 实部和虚部均为 float32 |
| complex128 | 实部和虚部均为 float64 |

默认复数类型是 complex128。

**初始化**

复数可以通过两种方式初始化。

+   使用 complex 函数。它具有以下签名。确保 a 和 b 的类型相同，即它们都应为 float32 或都应为 float64。

```go
complext(a, b)
```

+   使用简写语法。这在使用直接数字创建复数时使用。如果未指定类型，使用以下方法创建的复数类型将为**complex128**。

```go
a := 5 + 6i
```

**complex64**

对于 complex64，实部和虚部均为 float32。

**大小**：实部和虚部的大小与 float32 相同。它的大小为 32 位或 4 字节。

**范围**：实部和虚部的范围与 float32 相同，即 1.2E-38 到 3.4E+38。

**示例**

以下是一个示例代码，显示了。

+   如何使用上述两种方法创建一个 complex64 数字。

+   打印一个 complex64 数字的大小。大小将是 8 字节（4 + 4），相当于两个 float32 数字。

+   打印一个 complex64 数字的类型。

+   对复数进行+操作。

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

对于 complex128，**实部**和**虚部**均为**float64**。

**大小**：**实部**和**虚部**的大小与 float64 相同。它的大小为 64 位或 8 字节。

**范围**：**实部**和**虚部**的范围与**float64**相同，即-1.7E+308 到+1.7E+308。

**示例**

以下是一个示例代码，显示了。

+   如何使用上述两种方法创建一个 complex128 数字。当未指定类型时，默认类型将是**complex128**。

+   打印一个 complex128 数字的大小。大小将是 16 字节（8 + 8），相当于两个 float64 数字。

+   打印一个 complex128 数字的类型。

+   对复数进行不同操作。

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

Go 中的 byte 是**uint8**的别名，意味着它是一个整数值。这个整数值为 8 位，表示一个字节（即 0-255 之间的数字）。因此，一个字节可以表示 ASCII 字符。Golang 没有‘char’的数据类型。因此。

+   字节用于表示 ASCII 字符

+   rune 用于表示所有 UNICODE 字符，包括所有存在的字符。我们将在本教程后面研究 rune。

**定义字节**

```go
var rbyte byte := 'a'
```

在声明字节时，我们必须指定类型，就像我们在上面的程序中那样。如果不指定类型，则默认类型为 **rune**。

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

## **Rune**

Go 中的 rune 是 **int32** 的别名，意味着它是一个整数值。这个整数值用于表示一个 Unicode 码点。要理解 rune，你必须知道 Unicode 是什么。下面是简短的描述，但你可以参考著名的博文 – [每个软件开发者绝对、肯定必须了解的关于 Unicode 和字符集的最低限度知识（无借口！）](http://www.joelonsoftware.com/articles/Unicode.html)

**什么是 Unicode**

Unicode 是 ASCII 字符的超集，为每个存在的字符分配一个唯一的数字。这个唯一的数字称为 Unicode 码点。

例如

+   数字 **0** 被表示为 Unicode 点 **U+0030 (十进制值 – 48)**

+   小写 **b** 被表示为 Unicode 点 **U+0062 (十进制值 – 98)**

+   英镑符号 **£** 被表示为 Unicode 点 **U+00A3 (十进制值 – 163)**

访问 [`en.wikipedia.org/wiki/List_of_Unicode_characters`](https://en.wikipedia.org/wiki/List_of_Unicode_characters) 以了解其他字符的 Unicode 点。但是 Unicode 不会说明这些码点将如何在内存中保存。这就是 **utf-8** 进入的地方。

**UTF-8**

utf-8 使用 1、2、3 或 4 个字节保存每个 Unicode 点。ASCII 点使用 1 个字节存储。这就是为什么 rune 是 int32 的别名，因为在 Go 语言中，Unicode 点最多可以占用 4 个字节，且每个字符串都使用 utf-8 编码。

每个 rune 旨在指代一个 Unicode 点。例如，如果你将一个字符串类型转换为 rune 数组并打印它，那么它将打印出每个字符的 Unicode 点。对于下面的字符串 **“0b£”**，输出将是 – **[U+0030 U+0062 U+00A3]**

```go
fmt.Printf("%U\n", []rune("0b£"))
```

**声明 Rune**

rune 使用单引号声明，如下所示，声明一个名为 **‘rPound’** 的变量。

```go
rPound := '£'
```

声明 Rune 后，你还可以执行以下操作：

+   **打印类型 –** 输出将是 **int32**

```go
fmt.Printf("Type: %s\n", reflect.TypeOf(rPound))
```

+   **打印 Unicode 码点 –** 输出将是 **U+00A3**

```go
fmt.Printf("Unicode CodePoint: %U\n", rPound)
```

+   **打印字符 –** 输出将是 **£**

```go
fmt.Printf("Character: %c\n", r)
```

**何时使用**

当你打算在值中保存 Unicode 码点时，应该使用 rune。当数组中的所有值都表示一个 Unicode 码点时，应该使用 rune 数组。

**代码：**

以下是说明我们讨论的每个点的代码

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

+   使用双引号 “” 例如 “this”

双引号中的字符串尊重转义序列。例如，如果字符串包含 \n，则打印时会有新行。

+   使用反引号 ` eg \`this`

反引号中的字符串只是一个原始字符串，它不遵循任何类型的转义序列。

字符串中的每个字符将根据所使用的编码占用一些字节。例如，在 utf-8 编码的字符串中，每个字符将占用 1 到 4 个字节。你可以在这篇必读的著名博客中阅读关于 utf-8 的内容——[每个软件开发者绝对、肯定必须了解的 Unicode 和字符集的绝对最少知识（没有借口！）](http://www.joelonsoftware.com/articles/Unicode.html)。在 utf-8 中，字符 **a** 或 **b** 使用 1 个字节编码，而字符英镑符号 **£** 使用 2 个字节编码。因此，当你将字符串“ab£”转换为字节数组并像下面那样打印时，将输出 4 个字节。

```go
s := "ab£"
fmt.Println([]byte(s))
```

**输出**

```go
[48 98 194 163]
```

当你尝试使用 **len(“ab£”)** 打印上面字符串的长度时，它将输出 4 而不是 3，因为它包含 4 个字节。

还要注意，**range** 循环遍历每个字符形成的字节序列，因此对于下面的 range 循环

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

对字符串可以执行许多操作。其中一种操作是连接，它将两个字符串结合在一起。连接使用符号‘+’。让我们看看我们讨论过的所有内容的完整工作代码。

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

数据类型是 **bool**，并且有两个可能的值 true 或 false。

默认值：false

操作：

+   并且 – &&

+   或者 – ||

+   取反 – !

**示例**

以下代码示例展示了

+   如果未初始化，默认值是 **false**

+   上述所有操作在布尔值上

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

# 结论

这就是关于 golang 的基本类型。希望你喜欢这篇文章。请在评论中分享反馈、改进意见或错误。

**下一个教程** – [函数](https://golangbyexample.com/function-golang-complete-guide/)

**上一个教程** – [变量](https://golangbyexample.com/variables-in-golang-complete-guide/)***
