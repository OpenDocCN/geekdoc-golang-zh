<!--yml

分类：未分类

日期：2024-10-13 06:35:59

-->

# 理解 Go（Golang）中的 Fprintf 函数

> 来源：[`golangbyexample.com/fprintf-golang/`](https://golangbyexample.com/fprintf-golang/)

![Fprintf 图像](img/9dcf336e99223726fb9611b41749a744.png)

目录

+   概述

    +   格式化字符串变量

    +   格式化整数

    +   格式化结构体

+   程序

## **概述**

**Fprintf**在**fmt**包中定义，用于格式化字符串并将格式化后的字符串写入传递给它的**io.Writer**实例。

[`golang.org/pkg/fmt/#Fprint`](https://golang.org/pkg/fmt/#Fprint)

Golang 提供了另一个类似的函数**Printf**。**Fprintf**和**Printf**之间唯一的区别是，**Fprintf**将数据写入传递给它的**io.Writer**实例，而**Printf**函数则写入标准输出。

以下是**Fprintf**的函数原型

```go
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
```

**Fprintf**也是一个可变参数函数，意味着它可以有多个参数。以下是关于其参数的详细信息

+   第一个参数是要写入的**io.Writer**实例

+   第二个参数是一个**format**或**template**字符串。该字符串包含自定义说明符，如**%s**用于**string**，**%d**用于**int**等。

+   接下来是一个可变数量的参数。这个列表中的每个参数可以是字符串、整数、结构体或其他任何类型。这就是为什么它是一个空接口。

**Fprintf**使用自定义说明符格式化字符串。**format**或**template**字符串包含需要格式化的实际字符串及一些格式动词。这些格式动词指示如何在最终字符串中格式化后续参数。因此，格式字符串参数包含某些符号，这些符号将被后续参数替换。

还需要注意的是**Fprintf**函数不会添加新行。我们需要使用新行标识符来添加新行**“\n”**。

示例

### **格式化字符串变量**

+   **%s**符号被使用

+   示例

```go
name := "John"
fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
```

输出将如下所示。它将写入**os.Stdout**，即标准输出。请注意，我们将**os.Stdout**作为第一个参数传递。

```go
Name is John
```

### **格式化整数**

+   **%d**符号被使用

+   示例

```go
age := 21
fmt.Fprintf(os.Stdout, "Age is: %d\n", age)
```

输出将如下所示。它将写入**os.Stdout**，即标准输出。

### **格式化结构体**

例如，打印结构体时有三种格式说明符。

+   **%v** – 它只会打印值。字段名称不会被打印。这是使用 Println 打印结构体时的默认方式。

+   **%+v – **它将打印字段和值。

+   **%#v – **它将打印结构体，包括字段名称和值

这就是为什么

```go
fmt.Fprintf(os.Stdout, "Employee is %v\n", e)
fmt.Fprintf(os.Stdout, "Employee is %+v\n", e)
fmt.Fprintf(os.Stdout,"Employee is %#v\n", e)
```

分别写入以下内容到**os.Stdout**实例

```go
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

这与上述解释相符。此外，请注意，此函数返回打印的字符数和任何发生的错误。它不会添加新行。你需要明确地添加**“\n”**。

## **程序**

这是相应的工作程序。

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

	fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
	fmt.Fprintf(os.Stdout, "Age is: %d\n", age)

	fmt.Fprintf(os.Stdout, "Name: %s Age: %d\n", name, age)

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Fprintf(os.Stdout, "Employee is %v\n", e)
	fmt.Fprintf(os.Stdout, "Employee is %+v\n", e)
	fmt.Fprintf(os.Stdout, "Employee is %#v\n", e)

	bytesPrinted, err := fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
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

在上面的所有**Fprintf**函数中，我们将**os.Stdout**的实例传递给它，该实例实现了**io.Writer**接口。基本上，使用**os.Stdout**时，**Fprintf**写入标准输出。这就是**os.Stdout**的定义。

```go
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

注意下面的**Fprintf**。

```go
fmt.Fprintf(os.Stdout, "Name: %s Age: %d\n", name, age)
```

+   **%s**被名字替换。

+   **%d**被年龄替换。

所以基本上，格式字符串参数中的符号或动词按照顺序被后续参数替换。

**Fprintf**也可以用于写入文件。由于文件实例在 golang 中实现了**io.Writer**，因此这不是问题。下面是相应的程序。

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
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Fprintf(file, "Name is: %s\n", name)
    fmt.Fprintf(file, "Age is: %d\n", age)
    fmt.Fprintf(file, "Name: %s Age: %d\n", name, age)
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Fprintf(file, "Employee is %v\n", e)
    fmt.Fprintf(file, "Employee is %+v\n", e)
    fmt.Fprintf(file, "Employee is %#v\n", e)
}
```

**输出**

它将在当前目录中创建文件名**temp.txt**，并包含以下内容。在此程序中，我们将**os.Stdout**替换为创建的文件。

```go
Name is: John
Age is: 21
Name: John Age: 21
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

如果格式字符串中的格式说明符数量与下一个可变参数的数量不匹配，则格式说明符将原样打印。例如，在下面的代码中，我们有两个格式说明符。

+   **%d**

+   **%s**

而下一个可变参数只有一个。因此，当我们调用它时，它将把第二个格式说明符原样写出，并发出 MISSING 警告。

```go
package main

import (
	"fmt"
	"os"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"

	fmt.Fprintf(os.Stdout, "Name is: %s %d\n", name)
}
```

**输出**

```go
Name is: John %!d(MISSING)
```

此外，请查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)


