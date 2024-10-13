<!--yml

类别：未分类

日期：2024-10-13 06:35:23

-->

# Go (Golang)中的 Println vs Print vs Printf

> 来源：[`golangbyexample.com/println-printf-print-golang/`](https://golangbyexample.com/println-printf-print-golang/)

目录

+   概述

+   关于 Println 函数

+   关于 Print 函数

+   关于 Printf 函数

    +   打印字符串变量

    +   打印整数

    +   打印结构体

## **概述**

**Println**、**Print**和**Printf**在**fmt**包中定义，用于格式化字符串并写入标准输出

[`golang.org/pkg/fmt/`](https://golang.org/pkg/fmt/)

它们之间的基本区别是

+   **Println**使用默认格式说明符格式化字符串，并在字符串后添加新行

+   **Print**使用默认格式说明符格式化字符串，但在字符串后不添加新行

+   **Printf**使用自定义说明符格式化字符串。它也不会添加新行

让我们详细了解每个例子

## **关于 Println 函数**

以下是**Println**的函数原型

```go
func Println(a ...interface{}) (n int, err error)
```

**Println**接受可变数量的参数，每个参数都是一个空接口。它返回打印的字符数和任何发生的错误。由于参数类型是空接口，我们可以传递任何数据类型。我们可以传递字符串、整型、浮点型、结构体或任何其他数据类型。每个传递给**Println**函数的参数都根据该参数类型的默认格式说明符进行格式化。例如，结构体将根据以下说明符进行格式化

```go
%v
```

此格式说明符仅打印结构体中的值部分。让我们看一个例子

```go
package main
import "fmt"
type employee struct {
    Name string
    Age  int
}
func main() {
    name := "John"
    age := 21
    fmt.Println("Name is: ", name)
    fmt.Println("Age is: ", age)
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Println(e)
    fmt.Println("a", 12, "b", 12.0)

    bytesPrinted, err := fmt.Println("Name is: ", name)
    if err != nil {
	log.Fatalln("Error occured", err)
    }
    fmt.Println(bytesPrinted)
}
```

**输出**

```go
Name is: John
Age is: 21
{John 21}
a 12 b 12
Name is: John
14
```

关于**Println**函数的一些重要注意事项

+   它在末尾附加新行。这就是为什么每个输出都在不同的行上

+   输出中的每个参数将用空格分隔。这就是为什么

```go
fmt.Println("Name is: ", name)
```

打印

```go
Name is: John
```

两个参数之间会自动引入空格。

+   它返回打印的字符数或任何发生的错误

```go
bytesPrinted, err := fmt.Println("Name is: ", name)
if err != nil {
    log.Fatalln("Error occured", err)
}
fmt.Println(bytesPrinted)
```

将输出如下

```go
Name is: John
14
```

**bytesPrinted**的数量为 14，因为输出了 14 个字符

## **关于 Print 函数**

**Print**的函数原型

```go
func Print(a ...interface{}) (n int, err error)
```

**Print**函数与**Println**函数完全相同，除了两个区别

+   它在末尾不附加新行。我们需要使用新行标识符来添加新行“\n”。

+   只有当两个操作数都不是字符串时，才会在参数之间添加空格

让我们看一个相同的例子

```go
package main

import "fmt"

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21
	fmt.Print("Name is:", name, "\n")
	fmt.Print("Age is:", age, "\n")

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Print(e, "\n")

	fmt.Print("a", 12, "b", 12.0, "\n")

	fmt.Print(12, 12.0, "\n")

        bytesPrinted, err := fmt.Print("Name is: ", name, "\n")
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Print(bytesPrinted)
}
```

**输出**

```go
Name is:John
Age is:21
{John 21}
a12b12
12 12
Name is: John
14
```

关于**Print**函数的一些重要注意事项

+   它在末尾不会附加新行。这就是为什么需要使用**“\n”**来添加新行。

+   只有在每个参数都是非字符串时，它才会在两个参数之间添加空格。这就是原因

```go
fmt.Print(12, 12.0, "\n")
```

打印

```go
12 12
```

当

```go
fmt.Print("a", 12, "b", 12.0, "\n")
```

打印

```go
a12b12
```

+   它还会返回打印的字符数以及任何可能发生的错误

## **关于 Printf 函数**

**Printf** 的函数原型

```go
func Printf(format string, a ...interface{}) (n int, err error)
```

**Printf** 也是一个可变参数函数，这意味着它可以有多个参数。关于它的参数列表有两个重要点

+   请注意，第一个参数是 **格式** 或 **模板** 字符串。

+   下一个是可变数量的参数。这些参数可以是字符串、整数、结构体或其他任何类型。出于上述相同的原因，这就是为什么它是一个空接口

**格式** 或 **模板** 字符串包含需要格式化的实际字符串加上一些格式化动词。这些格式化动词告诉后续参数在最终字符串中将如何格式化。因此，基本上格式字符串参数包含某些符号，这些符号会被后续参数替换。

例如

### **打印字符串变量**

+   **%s** 符号被使用

+   示例

```go
name := "John"
fmt.Printf("Name is: %s\n", name)
```

### **打印整数**

+   **%d** 符号被使用

+   示例

```go
age := 21
fmt.Printf("Age is: %d\n", age)
```

### **打印结构体**

例如，打印结构体有三种格式说明符。

+   **%v** – 它将只打印值。字段名称将不会被打印。这是使用 Println 打印结构体的默认方式

+   **%+v –** 它将打印字段和值。

+   **%#v –** 它将打印结构体，同时打印字段名称和对应的值

这就是为什么

```go
fmt.Printf("Employee is %v\n", e)
fmt.Printf("Employee is %+v\n", e)
fmt.Printf("Employee is %#v\n", e)
```

分别打印如下

```go
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

这与上述解释一致。

此外，请注意，此函数返回打印的字符数以及任何可能发生的错误。与 **Println** 不同，它不会自动添加新行。你需要显式添加 **“\n”**。

下面是相应的工作程序

```go
package main

import (
	"fmt"
	"log"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21

	fmt.Printf("Name is: %s\n", name)
	fmt.Printf("Age is: %d\n", age)

	fmt.Printf("Name: %s Age: %d\n", name, age)

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Printf("Employee is %v\n", e)
	fmt.Printf("Employee is %+v\n", e)
	fmt.Printf("Employee is %#v\n", e)

	bytesPrinted, err := fmt.Printf("Name is: %s\n", name)
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
}
```

**输出**

```go
Name is: John
Age is: 21
Name: John Age: 21
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
Name is: John
14
```

请注意下面的 **Printf**

```go
fmt.Printf("Name: %s Age: %d\n", name, age)
```

+   **%s** 被替换为名称。

+   **%d** 被替换为年龄。

所以基本上，格式字符串参数中的符号或动词会按顺序被后续参数替换**如果格式字符串中的格式说明符数量与后续可变参数的数量不匹配，则格式说明符将按原样打印。例如，在下面的代码中，我们有两个格式说明符**

***   **%d**

+   **%s**

而下一个可变数量的参数只有一个。因此，当我们打印时，它将按照原样打印第二个格式说明符，并显示“MISSING”作为警告

```go
package main
import "fmt"
type employee struct {
    Name string
    Age  int
}
func main() {
    name := "John"
    fmt.Printf("Name is: %s %d\n", name)
}
```

**输出**

```go
Name is: John %!d(MISSING)
```

另外，查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


