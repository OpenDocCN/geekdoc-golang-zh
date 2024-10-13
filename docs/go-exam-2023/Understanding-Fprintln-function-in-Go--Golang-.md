<!--yml

分类：未分类

日期：2024-10-13 06:36:31

-->

# 理解Go（Golang）中的Fprintln函数

> 来源：[https://golangbyexample.com/fprintln-golang/](https://golangbyexample.com/fprintln-golang/)

![fprintln 图像](img/3cd9912b7a0733f8200683fac3715644.png)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

**Fprintln**定义在**fmt**包中，用于使用默认格式说明符格式化字符串，并将其写入传递给它的**io.Writer**实例。它还添加了一个新行。

[https://golang.org/pkg/fmt/#Fprintln](https://golang.org/pkg/fmt/#Fprintln)

下面是**Fprintln**的函数原型

```go
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

**Fprintln**也是一个可变参数函数，这意味着它可以有多个参数。这里是关于其参数的详细信息。

+   第一个参数是要写入的**io.Writer**实例

+   接下来是可变数量的参数。这个列表中的每个参数可以是字符串、整数、结构体或其他任何东西。这就是它是一个空接口的原因。每个参数都使用默认说明符进行格式化。

**Fprintln**使用默认格式说明符格式化字符串，并在字符串后添加新行。**Fprintln**接受可变数量的参数，每个参数都是一个空接口。它返回打印的字符数量和发生的任何错误。由于参数类型是空接口，我们可以传递任何数据类型。我们可以传递字符串、整数、浮点数、结构体或任何其他数据类型。每个传递给**Fprintln**函数的参数都根据该参数类型的默认格式说明符进行格式化。例如，结构体将根据以下说明符进行格式化。

**fmt**包中还有另一个函数**Fprint**，与**Fprintln**相同。唯一的区别是

+   **Fprintln**在末尾添加新行，而**Fprint**则不添加新行。

```go
%v
```

该格式说明符仅打印结构体中的值部分。让我们看一个例子。

## **程序**

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
	fmt.Fprintln(os.Stdout, "Name is: ", name)
	fmt.Fprintln(os.Stdout, "Age is: ", age)
	e := employee{
		Name: name,
		Age:  age,
	}
	fmt.Fprintln(os.Stdout, e)
	fmt.Fprintln(os.Stdout, "a", 12, "b", 12.0)

	bytesPrinted, err := fmt.Fprintln(os.Stdout, "Name is: ", name)
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
}
```

**输出**

```go
Name is:  John
Age is:  21
{John 21}
a 12 b 12
Name is:  John
15
```

关于**FPrintln**函数的一些重要事项

+   在上述所有**Fprintln**函数中，我们将**os.Stdout**的实例传递给它，该实例实现了**io.Writer**接口。基本上，使用**os.Stdout**，**Fprintln**写入标准输出。这就是**os.Stdout**的定义。

```go
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

+   它在末尾添加一个新行。这就是为什么每个输出在不同的行上

+   输出中每个参数将以空格分隔。这就是为什么

```go
fmt.Fprintln(os.Stdout,"Name is: ", name)
```

打印

```go
Name is: John
```

在两个参数之间自动引入空格。

+   它返回打印的字符数量或发生的任何错误

```go
bytesPrinted, err := fmt.Fprintln(os.Stdout, "Name is: ", name)
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

**bytesPrinted**的数量是14，因为输出了14个字符

**Fprintln**还可以用于写入文件。由于文件实例在Golang中实现了**io.Writer**，这不是问题。以下是相应的程序

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
	fmt.Fprintln(file, "Name is: ", name)
	fmt.Fprintln(file, "Age is: ", age)
	e := employee{
		Name: name,
		Age:  age,
	}
	fmt.Fprintln(file, e)
	fmt.Fprintln(file, "a", 12, "b", 12.0)
}
```

**输出**

它将在当前目录中创建名为temp.txt的文件，内容如下。在这个程序中，我们将**os.Stdou**t替换为创建的文件。

```go
Name is:  John
Age is:  21
{John 21}
a 12 b 12
```

另外，查看我们的Golang进阶教程系列 – [Golang进阶教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

+   [fprintln](https://golangbyexample.com/tag/fprintln/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
