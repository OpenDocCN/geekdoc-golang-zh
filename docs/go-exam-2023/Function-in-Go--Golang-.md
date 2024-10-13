<!--yml

类别：未分类

日期：2024-10-13 06:16:51

-->

# Go语言中的函数

> 来源：[https://golangbyexample.com/function-golang-complete-guide/](https://golangbyexample.com/function-golang-complete-guide/)

这是Go语言综合教程系列的第8章。有关该系列其他章节，请参考此链接 – [Go语言综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一教程** – [常量](https://golangbyexample.com/constant-golang/)

**上一教程** – [所有基本数据类型](https://golangbyexample.com/all-basic-data-types-golang/)

现在让我们来查看当前的教程。以下是当前教程的目录。

目录

**[概述](#Overview "Overview")**

+   **[函数的签名](#Signature_of_a_function "Signature of a function")**

+   **[调用函数](#Calling_a_function "Calling a function")**

+   **[函数参数](#Function_Parameters "Function Parameters")**

+   **[返回值](#Return_Values "Return Values")**

    +   **[命名返回值](#Named_Return_Values "Named Return Values")**

+   **[函数用法](#Function_Usages "Function Usages")**

    +   **[泛型用法](#Generic_Usage "Generic Usage")**

    +   **[函数作为类型](#Function_as_Type "Function as  Type")**

        +   **[函数作为用户定义类型](#Function_as_User_Defined_Type "Function as User Defined Type")**

    +   **[函数作为值（或匿名函数）](#Function_as_values_or_Anonymous_functions "Function as values (or Anonymous functions)")**

+   **[函数的特殊用法](#Special_Usages_of_Function "Special Usages of Function")**

    +   **[函数闭包](#Function_Closures "Function Closures")**

    +   **[高阶函数](#Higher_Order_Function "Higher Order Function")**

    +   **[IIF或立即调用函数](#IIF_or_Immediately_Invoked_Function "IIF or Immediately Invoked Function")**

    +   **[可变参数函数](#Variadic_Function "Variadic Function")**

    +   **[方法](#Methods "Methods")**

+   **[结论](#Conclusion "Conclusion")** *# **概述**

函数是一组执行特定任务的语句。在Go中，函数是一级变量，可以像其他变量一样传递。

关于函数名称的一些注意事项

+   函数名称不能以数字开头

+   函数名称是区分大小写的。因此，sum、Sum、SUM是不同的函数。

+   名称以大写字母开头的函数将被导出到其包外，并可以从其他包调用。名称以小写字母开头的函数不会被导出，仅在其包内可见。

# **函数的签名**

```go
func func_name(input_parameters) return_values{
  //body
}
```

Go语言中的函数

+   使用**func**关键字声明

+   它有一个名称

+   逗号分隔的零个或多个输入参数

+   逗号分隔的零个或多个返回值

+   函数体

+   可以返回多个值

一个函数的例子。下面的函数是

+   名称为**“sum”**

+   接受两个int类型的参数

+   返回一个int类型的单一值

```go
func sum(a int, b int) int {
  return a + b 
}
```

# **调用函数**

Go中的函数可以如下调用

```go
results := sum(2, 3)
```

调用函数时需要注意的一些要点

+   如果调用的是其他包中的函数，则必须在前面加上包名。另外，请注意，在不同包之间只能调用那些被导出的函数，即名称以大写字母开头的函数。

+   在同一个包内，可以直接使用名称后缀 () 调用函数。

# **函数参数**

+   如上所述，函数可以有零个或多个参数。

+   连续相同类型的类型可以只指定一次。例如，上面的求和函数也可以写成。

```go
func sum(a, b int)
```

+   在调用函数时会创建所有参数的副本。

# **返回值**

+   如上所述，函数可以有一个或多个返回值。假设有一个函数 sum_avg 返回两个值：和与平均值。多个返回值的示例。

```go
func sum_avg(a, b int) (int, int)
```

+   按照约定，错误作为函数的最后一个参数返回。示例。

```go
func sum(a, b int) (int, error)
```

+   在调用函数中收集多个返回值。在下面的示例中。

```go
result, err := sum(2, 3) 
```

## **命名返回值**

Go 函数可以具有命名返回值。对于命名返回值，返回值在函数中不需要初始化。命名变量在签名中指定。没有命名值时，仅指定返回类型。对某些返回值命名也是可以的。对于其他返回值，只能指定类型。

+   见下面的示例：**result** 是命名返回值。

```go
func sum(a, b int) (result int)
```

+   对于命名返回值，连续相同类型的类型只能指定一次。

```go
func sum_avg(a, b int) (sum, avg int)
```

# **函数用法**

+   通用用法

+   函数作为类型

+   函数作为值

函数作为类型与函数作为值之间的区别在于，在类型中我们只使用函数签名，而在函数作为值时则使用签名和主体。让我们详细了解每个内容，以便更清晰。

## **通用用法**

以下是函数一般用法的示例。我们在下面的示例中有一个函数 sum，它接受两个整数作为输入参数并返回它们的和。

```go
package main

import "fmt"

func main() {
    res := sum(2, 3)
    fmt.Println(res)
}

func sum(a, b int) int {
    return a + b
}
```

**输出：**

```go
5
```

## **函数作为类型**

在 Go 中，函数也是一种类型。如果两个函数是同一类型的。

+   它们有相同数量的参数，每个参数的类型相同。

+   它们有相同数量的返回值，并且每个返回值的类型相同。

函数类型在以下情况下非常有用。

+   在上述例子中，我们看到高阶函数的情况。参数和返回类型是通过函数类型来指定的。

+   在 Go 中定义接口的情况下，接口中仅指定函数类型。实现此接口的任何东西都必须定义相同类型的函数。

让我们看一个接口中函数类型的示例。注意接口 **shape** 只定义了函数的类型。

```go
area() int
getType() string
```

**代码：**

```go
package main
import "fmt"
func main() {
    var shapes []shape
    s := &square{side: 2}
    shapes = append(shapes, s)
    r := &rectangle{length: 2, breath: 3}
    shapes = append(shapes, r)
    for _, shape := range shapes {
        fmt.Printf("Type: %s, Area %d\n", shape.getType(), shape.area())
    }
}
type shape interface {
    area() int
    getType() string
}
type rectangle struct {
    length int
    breath int
}
func (r *rectangle) area() int {
    return r.length * r.breath
}
func (r *rectangle) getType() string {
    return "rectangle"
}
type square struct {
    side int
}
func (s *square) area() int {
    return s.side * s.side
}
func (s *square) getType() string {
    return "square"
}
```

**输出：**

```go
Type: square, Area 4
Type: rectangle, Area 6
```

### **作为用户定义类型的函数**

自定义类型的函数可以使用 type 关键字声明。

```go
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    print(3, 4, areaF)
}

type area func(int, int) int

func print(x, y int, a area) {
    fmt.Printf("Area is: %d\n", a(x, y))
}

func getAreaFunc() area {
    return func(x, y int) int {
        return x * y
    }
}
```

**输出：**

```go
12
```

## **作为值的函数（或匿名函数）**

Go 语言中的函数是一个一阶变量，因此也可以作为值使用。它也被称为匿名函数，因为函数没有名称，可以被赋值给一个变量并传递。

它们通常用于短期使用或有限功能。请看下面的示例。在这个示例中，变量 **max** 被赋值为一个函数。由于 **max** 是由一个没有名称的函数创建的，所以它是一个匿名函数。

```go
package main

import "fmt"

var max = func(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func main() {
    res := max(2, 3)
    fmt.Println(res)
}
```

**输出：**

```go
3
```

# **函数的特殊用法**

## **函数闭包**

函数闭包就是可以访问在函数外声明的变量，并且在不同函数调用之间保持当前值的匿名函数。我们来看一个例子。在下面的案例中，**modulus** 函数的不同调用之间 **count** 的值得以保留。

```go
package main

import (
    "fmt"
)

func main() {
    modulus := getModulus()
    modulus(-1)
    modulus(2)
    modulus(-5)
}

func getModulus() func(int) int {
    count := 0
    return func(x int) int {
        count = count + 1
        fmt.Printf("modulus function called %d times\n", count)
        if x < 0 {
            x = x * -1
        }
        return x
    }
}
```

**输出：**

```go
modulus function called 1 times
modulus function called 2 times
modulus function called 3 times
```

## **高阶函数**

高阶函数是那些接受函数作为参数或返回函数的函数。由于函数在 Go 语言中是一阶变量，它们可以被传递，也可以从某个函数返回并赋值给一个变量。在下面的示例中。

+   **print** 函数接受一个类型为 **func(int, int) int** 的函数作为参数。

+   **getAreafunc** 返回一个类型为 **func(int, int) int** 的函数。

```go
package main

import "fmt"

func main() {
    areaF := getAreaFunc()
    print(3, 4, areaF)
}

func print(x, y int, area func(int, int) int) {
    fmt.Printf("Area is: %d\n", area(x, y))
}

func getAreaFunc() func(int, int) int {
    return func(x, y int) int {
        return x * y
    }
}
```

**输出：**

```go
12
```

## **立即调用函数（IIF）**

**立即调用函数（IIF）** 是那些可以同时定义和执行的函数。

```go
package main

import "fmt"

func main() {
    squareOf2 := func() int {
        return 2 * 2
    }()
    fmt.Println(squareOf2)
}
```

**输出：**

```go
4
```

**IIF 函数的使用案例**

+   当你不想在包内或包外暴露函数的逻辑时。例如，假设有一个函数设置某个值。你可以将所有设置逻辑封装在一个 IIF 函数中。这个函数在包内外都不可调用。

## **变参函数**

在 Go 语言中，可以接受动态数量参数的函数称为变参函数。下面是变参函数的语法。在类型前使用三个点作为前缀。

```go
func add(numbers ...int)
```

**代码：**

```go
package main

import "fmt"

func main() {
    fmt.Println(add(1, 2))
    fmt.Println(add(1, 2, 3))
    fmt.Println(add(1, 2, 3, 4))
}

func add(numbers ...int) int {
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    return sum
}
```

**输出：**

```go
3
6
10
```

## **方法**

方法具有接收者参数。当你将一个函数附加到一个类型时，这个函数就成为该类型的方法。接收者可以是结构体或任何其他类型。方法将访问接收者的属性，并可以调用接收者的其他方法。

**函数：**

```go
func some_func_name(arguments) return_values
```

**方法：**

```go
func (receiver receiver_type) some_func_name(arguments) return_values
```

函数与方法之间唯一的区别在于它们提供的功能不同。

+   函数可以作为一阶对象使用并被传递，而方法则不能。

+   方法可以在接收者上进行链式调用，而函数则不能用于相同的用途。

+   可以存在不同接收者的同名方法，但在同一个包中不能存在同名的两个不同函数。

# **结论**

这篇文章全部关于 Go 语言中的函数。希望你喜欢这篇文章。请在评论中分享反馈或错误/改进意见。

**下一个教程** – [常量](https://golangbyexample.com/constant-golang/)

**上一个教程** – [所有基本数据类型](https://golangbyexample.com/all-basic-data-types-golang/)

+   [完整](https://golangbyexample.com/tag/complete/) * [函数](https://golangbyexample.com/tag/function/) * [Go](https://golangbyexample.com/tag/go/) * [Golang](https://golangbyexample.com/tag/golang/) * [指南](https://golangbyexample.com/tag/guide/) *
