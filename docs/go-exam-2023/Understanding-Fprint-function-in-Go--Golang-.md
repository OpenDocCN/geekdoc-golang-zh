<!--yml

类别：未分类

日期：2024-10-13 06:36:05

-->

# 理解 Go 中的 Fprint 函数（Golang）

> 来源：[`golangbyexample.com/fprint-golang/`](https://golangbyexample.com/fprint-golang/)

![](img/73f3f2483c5496743de2017b97b4b213.png)

目录

+   概述

+   程序

## **概述**

**Fprint**在**fmt**包中定义，用于使用默认格式说明符格式化字符串，并将其写入传递给它的**io.Writer**实例。

[`golang.org/pkg/fmt/#Fprint`](https://golang.org/pkg/fmt/#Fprint)

下面是**Fprint**的函数原型

```go
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
```

**Fprint**也是一个可变参数函数，这意味着它可以有多个参数。以下是有关其参数的详细信息

+   第一个参数是写入的**io.Writer**实例

+   接下来的参数是可变数量的参数。此列表中的每个参数可以是字符串、整数、结构体或其他任何东西。这就是为什么它是一个空接口。每个参数都使用默认说明符进行格式化

**Fprint**使用默认格式说明符格式化字符串，但在字符串后不添加新行。**Fprint**在第一个参数后接受可变数量的参数，其中每个参数都是一个空接口。由于参数类型是空接口，我们可以将任何数据类型传递给它。我们可以传递字符串、整数、浮点数、结构体或任何其他数据类型。传递给**Fprint**函数的每个参数都根据该参数类型的默认格式说明符进行格式化。例如，结构体将根据以下说明符进行格式化

```go
%v
```

这个格式说明符只打印结构中的值部分。**fmt**包中还有一个函数可以添加新行 – **Fprintln**。

## **程序**

让我们来看一个例子

```go
package main

import (
	"fmt"
	"log"
	"os"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21

	fmt.Fprint(os.Stdout, "Name is:", name, "\n")
	fmt.Fprint(os.Stdout, "Age is:", age, "\n")

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Fprint(os.Stdout, e, "\n")

	fmt.Fprint(os.Stdout, "a", 12, "b", 12.0, "\n")

	fmt.Fprint(os.Stdout, 12, 12.0, "\n")

	bytesPrinted, err := fmt.Fprint(os.Stdout, "Name is: ", name, "\n")
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
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

关于**Fprint**函数的一些重要点

+   在上面的所有**Fprint**函数中，我们将**os.Stdout**的实例传递给它，该实例实现了**io.Writer**接口。基本上，使用**os.Stdout**，**Fprint**写入标准输出。这就是**os.Stdout**的定义。

```go
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

+   它不会在末尾添加新行。这就是为什么需要使用**“\n”**来添加新行的原因。

+   只有当两个参数都是非字符串时，它才会在这两个参数之间添加空格。这就是为什么

```go
fmt.Fprint(os.Stdout, 12, 12.0, "\n")
```

打印

```go
12 12
```

while

```go
fmt.Fprint(os.Stdout, "a", 12, "b", 12.0, "\n")
```

打印

```go
a12b12
```

+   它还返回打印的字符数以及发生的任何错误

```go
bytesPrinted, err := fmt.Fprint(os.Stdout, "Name is: ", name, "\n")
if err != nil {
    log.Fatalln("Error occured", err)
}
fmt.Fprint(bytesPrinted)
```

将输出以下内容

```go
Name is: John
14
```

**bytesPrinted**的数量为 14，因为输出了 14 个字符

**Fprint**也可以用于写入文件。由于在 golang 中的文件实例实现了**io.Writer**，这不是问题。下面是相应的程序

```go
package main
import (
    "fmt"
    "log"
    "os"
)
type employee struct {
    Name string
    Age  int
}
func main() {
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    name := "John"
    age := 21
    fmt.Fprint(file, "Name is:", name, "\n")
    fmt.Fprint(file, "Age is:", age, "\n")
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Fprint(file, e, "\n")
    fmt.Fprint(file, "a", 12, "b", 12.0, "\n")
    fmt.Fprint(file, 12, 12.0, "\n")
}
```

**输出**

它将在当前目录中创建文件名 temp.txt，内容如下。在这个程序中，我们用创建的文件替换了**os.Stdout**。

```go
Name is:John
Age is:21
{John 21}
a12b12
12 12
```

另外，请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


