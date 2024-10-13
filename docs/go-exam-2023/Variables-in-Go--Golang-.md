<!--yml

分类：未分类

日期：2024-10-13 06:18:02

-->

# Go 语言中的变量

> 来源：[https://golangbyexample.com/variables-in-golang-complete-guide/](https://golangbyexample.com/variables-in-golang-complete-guide/)

这是 Go 语言综合教程系列的第 6 章。请参考此链接了解该系列的其他章节 – [Go 语言综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [所有基本数据类型](https://golangbyexample.com/all-basic-data-types-golang/)

**上一个教程** – [包和模块 – 第2部分](https://golangbyexample.com/packages-modules-go-second/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**   [什么是变量](#What_is_variable "什么是变量")

+   [命名约定](#Naming_Conventions "命名约定")

+   [声明变量](#Declaring_a_variable "声明变量")

    +   [无初始值的单变量声明](#Single_variable_declaration_without_initial_value "无初始值的单变量声明")

    +   [带初始值的单变量声明](#Single_variable_declaration_with_initial_value "带初始值的单变量声明")

    +   [无初始值的多变量声明](#Multiple_variable_declaration_without_initial_value "无初始值的多变量声明")

    +   [带初始值的多变量声明](#Multiple_variable_declaration_with_initial_value "带初始值的多变量声明")

    +   [声明不同类型的变量](#Declare_variables_of_different_types "声明不同类型的变量")

    +   [无类型声明或类型推断](#Variable_Declaration_with_no_type_or_Type_Inference "无类型声明或类型推断")

    +   [短变量声明](#Short_variable_declaration "短变量声明")

+   [重要点](#Important_Points "重要点")

+   [变量的作用域（局部变量和全局变量）](#Scope_of_a_Variable_Local_and_Global_Variable "变量的作用域（局部变量和全局变量）")

    +   [局部变量](#Local_Variable "局部变量")

    +   [全局变量](#Global_Variable "全局变量")

+   [结论](#Conclusion "结论")*  *# **什么是变量**

变量是内存位置的名称。该内存位置可以存储任何类型的值。因此，每个变量都有一个与之相关的类型，这决定了该变量的大小和范围，以及在该变量上定义的操作。

# **命名约定**

+   变量名只能以字母或下划线开头。

+   变量名后可以跟任意数量的字母、数字或下划线。

+   Go 语言区分大小写，因此大写字母和小写字母被视为不同的字符。

+   变量名不能是 Go 语言中的任何关键字。

+   变量名的长度没有限制。

+   但建议变量名应保持在最佳长度。

# **声明变量**

在GO中，变量是使用**var**关键字声明的，但还有其他声明变量的方法，稍后在本教程中我们将看到。让我们深入探索声明变量的不同方式。

## **单变量声明，无初始值**

以下是没有分配初始值的单变量声明格式。第一是**var**关键字，第二是**变量名**，第三是**变量类型**。还要注意，当未提供值时，变量会被初始化为该类型的默认值，这也被称为该类型的零值。在Go中，**int**的默认值或零值是零。

**var <变量名> <类型>**

见下面的示例，它声明了一个名为**aaa**的**int**类型的变量。

```go
package main

import "fmt"

func main() {
    var aaa int
    fmt.Println(aaa)
}
```

**输出：** 将打印int的默认值，即零。

```go
0
```

## **带初始值的单变量声明**

以下是分配初始值的单变量声明格式。与上面相同，唯一不同的是在末尾为变量分配值。

**var <变量名> <类型> = <值>**

见下面的示例，它声明了一个名为**aaa**的**int**类型的变量，并给它赋值为**8**。

```go
package main

import "fmt"

func main() {
    var aaa int = 8
    fmt.Println(aaa)
}
```

**输出：**

```go
8
```

## **没有初始值的多个变量声明**

以下是没有分配初始值的多个变量声明的格式。请注意，只有相同类型的变量可以一起声明，类型位于末尾。

**var <名称1>, <名称2>**,….**<名称N> <类型>**

见下面的示例

```go
package main

import "fmt"

func main() {
    var aaa, bbb int
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**输出：** 将打印**int**的默认值或零值，对**aaa**和**bbb**都是零。

```go
0
0
```

## **带初始值的多个变量声明**

以下是分配初始值的多个变量声明的格式。请注意，只有相同类型的变量可以一起声明，类型位于末尾。

**var <名称1>, <名称2>, …..,<名称N> <类型> = <值1>, <值2>, …..,<值N>**

见下面的代码示例。变量**aaa**和**bbb**在一次声明中分别被赋值为8和9。

```go
package main

import "fmt"

func main() {
    var aaa, bbb int = 8, 9
    fmt.Println(aaa)
    fmt.Println(bbb)
}
```

**输出：**

```go
8
9
```

## **声明不同类型的变量**

以下是声明不同类型多个变量的格式。可以在那时为变量分配值，也可以不分配。没有分配值的变量将获得该类型的默认值。在下面的示例中，我们看到三个声明。

```go
package main

import "fmt"

func main() {
    var (
        aaa int
        bbb int    = 8
        ccc string = "a"
    )

    fmt.Println(aaa)
    fmt.Println(bbb)
    fmt.Println(ccc)
}
```

**输出：**

```go
0
8
a
```

## **没有类型或类型推断的变量声明**

变量也可以在不指定类型的情况下声明。

GO编译器会根据分配给变量的值确定类型。因此，如果变量有初始值，则可以省略类型。这也称为**类型推断**。以下是这种声明的格式。

**var <变量名> = <值>**

下面是基本类型 **int**、**float**、**复数**、**string**、**boolean**、**字符**的类型推断表。这基本上意味着，如果值是整数，则推断出的变量类型将是 **int**；如果值是浮点数，则推断出的变量类型将是 **float64**，依此类推。

| 整数 | int |
| --- | --- |
| 浮点数 | float64 |
| 复数 | complex128 |
| 字符串 | string |
| 布尔值 | bool |
| 字符 | int32 或 rune |

对于 **数组**、**指针**、**结构体** 等其他类型，类型推断将根据值进行。让我们看看上述观点的一个工作示例。注意，变量 t 的类型被正确推断为 int，因为赋值给它的是 123，这个值是 int。同样，变量 u 的类型也被正确推断为 **string**，因为赋值给它的是一个 **string**。

还注意到变量 **z** 的类型被正确推断为结构体 **main.sample**。

```go
package main

import "fmt"

func main() {
    var t = 123      //Type Inferred will be int
    var u = "circle" //Type Inferred will be string
    var v = 5.6      //Type Inferred will be float64
    var w = true     //Type Inferred will be bool
    var x = 'a'      //Type Inferred will be rune
    var y = 3 + 5i   //Type Inferred will be complex128
    var z = sample{name: "test"}  //Type Inferred will be main.Sample

    fmt.Printf("Type: %T Value: %v\n", t, t)
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}

type sample struct {
    name string
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
Type: main.sample Value: &{test}
```

## **短变量声明**

Go 还提供了另一种声明变量的方法，即使用 **:=** 运算符。当使用 **:=** 运算符时，可以省略 **var** 关键字和类型信息。下面是这种声明的格式。

```go
 <variable_name>:= <value></value></variable_name>
```

类型推断将如上所述进行。让我们看看一个工作示例。

```go
package main

import "fmt"

func main() {
    t := 123      //Type Inferred will be int
    u := "circle" //Type Inferred will be string
    v := 5.6      //Type Inferred will be float64
    w := true     //Type Inferred will be bool
    x := 'a'      //Type Inferred will be rune
    y := 3 + 5i   //Type Inferred will be complex128
    z := sample{name: "test"}  //Type Inferred will be main.Sample

    fmt.Printf("Type: %T Value: %v\n", t, t)
    fmt.Printf("Type: %T Value: %v\n", u, u)
    fmt.Printf("Type: %T Value: %v\n", v, v)
    fmt.Printf("Type: %T Value: %v\n", w, w)
    fmt.Printf("Type: %T Value: %v\n", x, x)
    fmt.Printf("Type: %T Value: %v\n", y, y)
    fmt.Printf("Type: %T Value: %v\n", z, z)
}

type sample struct {
    name string
}
```

**输出**

```go
Type: int Value: 123
Type: string Value: circle
Type: float64 Value: 5.6
Type: bool Value: true
Type: int32 Value: 97
Type: complex128 Value: (3+5i)
Type: main.sample Value: &{test}
```

关于 := 运算符的一些注意事项。

+   := 运算符仅在函数内部可用，函数外部不允许使用。

+   一旦使用 := 声明的变量，不能再次使用 := 运算符重新声明。因此，下面的语句将引发编译器错误“**左侧没有新变量**”。

```go
a := 8
a := 16
```

+   := 运算符还可以用来在一行中声明多个变量。见下面的例子。

```go
a,b := 1, 2
```

+   在多重声明的情况下，如果左侧至少有一个变量是新的，则可以再次使用 := 进行特定变量的声明。见下面的例子。注意 b 又是通过 := 声明的。这只有在至少有一个变量是新的情况下才能实现，这里是变量 **c**。在这种情况下，它作为变量 **b** 的赋值。

```go
package main

import "fmt"

func main() {
    a, b := 1, 2
    b, c := 3, 4
    fmt.Println(a, b, c)
}
```

**输出：**

```go
1, 3, 4
```

# **重要事项**

+   未使用的变量将被报告为编译器错误。Go 编译器不允许任何未使用的变量。这是 Go 的一种优化。同样的规则适用于常量，稍后我们将看到。例如，下面的程序将引发编译器错误。

```go
a declared but not used
```

```go
package main

func main() {
    var a = 1
}
```

+   在内层作用域中声明的变量如果与外层作用域中声明的变量同名，将会遮蔽外层作用域的变量。

```go
package main

import "fmt"

var a = 123

func main() {
    var a = 456
    fmt.Println(a)
}
```

**输出：**

```go
456
```

+   变量表达式 – 在声明变量时，也可以将表达式或函数调用赋值给它。见下面的例子。

    +   变量 **a** 是通过表达式 5 + 3 声明的。

    +   变量 **b** 是通过函数调用 math.Max(4, 5) 声明的，其结果将在运行时赋值给 b。

```go
package main
import (
    "fmt"
    "math"
)
func main() {
    a := 5 + 3
    b := math.Max(4, 5)
    fmt.Println(a)
    fmt.Println(b)
}
```

**输出：**

```go
8
5
```

+   一旦初始化为特定类型的变量，不能再赋值为不同类型的值。这对于简写声明同样适用。见下面的例子。

```go
package main

func main() {
    var aaa int = 1
    aaa = "atest"

    bbb := 1
    bbb = "btest"
}
```

**输出：**

```go
cannot use "atest" (type untyped string) as type int in assignment
cannot use "btest" (type untyped string) as type int in assignment
```

变量 **aaa** 被赋予了 **int** 类型，因此在将字符串类型的值赋给它时，编译器会引发错误。对于变量 **bbb**，推断的类型是 **int**，因此在将字符串类型的值赋给它时也会引发编译器错误。

# **变量的作用域（局部和全局变量）**

变量声明可以在包级、函数级或块级进行。变量的作用域定义了该变量可以访问的位置以及变量的生命周期。Golang 变量可以根据作用域分为两类。

+   局部变量

+   全局变量

## **局部变量**

+   局部变量是在块或函数级别定义的变量。

+   块的一个例子是 for 循环或范围循环等。

+   这些变量只能在它们的块或函数内部访问。

+   这些变量仅在声明它们的块或函数结束时存在。之后，它们会被垃圾回收。

见下面的例子。

+   在 for 循环之后，i 不可用。

+   同样，aaa 在声明它的函数外部不可用。

因此，下面的程序将引发编译器错误。

```go
undefined: i
undefined: aaa
```

**代码：**

```go
package main

import "fmt"

func main() {
    var aaa = "test"
    fmt.Println(aaa)
    for i := 0; i < 3; i++ {
        fmt.Println(i)
    }
    fmt.Println(i)
}

func testLocal() {
    fmt.Println(aaa)
}
```

## **全局变量**

+   如果变量在文件顶部声明，并且在任何函数或块的作用域之外，则它将在包内成为全局变量。

+   如果这个变量名以小写字母开头，则可以在包含该变量定义的包内访问。

+   如果变量名以大写字母开头，则可以从不同于声明所在包的外部访问。

+   全局变量在程序的整个生命周期内可用。

例如，在下面的程序中，变量 **aaa** 将是一个全局变量，在 **main** 包中可用。它将在 **main** 包内的任何函数中可用。请注意，由于变量名以小写字母开头，因此该变量名在 **main** 包之外不可用。

```go
package main

import "fmt"

var aaa = "test"

func main() {
    testGlobal()
}

func testGlobal() {
    fmt.Println(aaa)
}
```

**输出：**

```go
test
```

# **结论**

这就是关于 Golang 中变量的全部内容。希望你喜欢这篇文章。请在评论中分享反馈、改进建议或错误。

****下一教程** – [所有基本数据类型](https://golangbyexample.com/all-basic-data-types-golang/)

**上一教程** – [包和模块 – 第 2 部分](https://golangbyexample.com/packages-modules-go-second/)**

+   [golang](https://golangbyexample.com/tag/golang/)*   [理解 Go 中的变量](https://golangbyexample.com/tag/understanding-variables-in-go/)*   [在 Go 中使用变量](https://golangbyexample.com/tag/using-variables-in-go/)*
