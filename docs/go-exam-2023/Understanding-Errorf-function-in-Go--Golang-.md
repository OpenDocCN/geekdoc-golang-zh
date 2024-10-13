<!--yml

类别：未分类

日期：2024-10-13 06:35:52

-->

# 理解 Go 中的 Errorf 函数

> 来源：[`golangbyexample.com/errorf-function-golang/`](https://golangbyexample.com/errorf-function-golang/)

目录

**   概述

    +   格式化字符串变量

    +   格式化一个整数

    +   格式化结构体

+   错误的封装*  *## **概述**

**Errorf**函数在**fmt**包中定义，用于根据提供的格式说明符创建带有格式化消息的自定义错误。

[`golang.org/pkg/fmt/#Errorf`](https://golang.org/pkg/fmt/#Errorf)

+   它的主要用途是用格式化消息在 Golang 中创建自定义错误。

+   它还用于创建可以封装另一个提供给它的错误的自定义错误。在 Golang 中，一个错误可以封装另一个错误。错误的封装是什么意思？这意味着创建一个错误的层次结构，其中特定的错误实例封装另一个错误，而该特定实例本身也可以被封装在另一个错误中。我们将在本教程后面详细讨论。

以下是**Errorf**的函数原型

```go
func Errorf(format string, a ...interface{}) error
```

如你所见，它返回一个错误的实例。

**Errorf**也是一个可变参数函数，这意味着它可以有多个参数。关于其参数列表有两个重要点

+   请注意第一个参数是一个**格式**或**模板**字符串。

+   下一个是可变数量的参数。此列表中的每个参数可以是字符串、整数、结构体或其他类型。这就是它是一个空接口的原因。

**Errrof**使用自定义说明符格式化字符串。第一个参数是**格式**或**模板**字符串，包含需要格式化的实际字符串以及一些格式化动词。这些格式化动词告诉后续参数在最终字符串中如何格式化。因此，基本上格式字符串参数包含某些符号，这些符号由后续参数替换。

例如

### **格式化字符串变量**

+   **%s**符号被使用

+   示例

```go
name := "J"
fmt.Errorf("Name has less then 3 character. Name: %s\n", name)
```

### **格式化一个整数**

+   **%d**符号被使用

+   示例

```go
age := 0
fmt.Errorf("Age is 0: Age:%d\n", age)
```

### **格式化结构体**

例如，有三种格式说明符用于打印结构体。

+   **%v** – 只打印值，字段名不会被打印。这是使用 Println 打印结构体的默认方式

+   **%+v – **它将打印字段和对应的值。

+   **%#v – **它将打印结构体，同时显示字段名和值

这就是为什么

```go
fmt.Errorf("Employee not found. Details: %v\n", e)
fmt.Errorf("Employee not found. Details: %+v\n", e)
fmt.Errorf("Employee not found. Details: %#v\n", e)
```

返回的错误信息如下格式化消息

这是相同功能的工作程序

```go
package main

import (
	"fmt"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	sampleErr := "database connection issue"

	err := fmt.Errorf("Err is: %s", sampleErr)
	fmt.Println(err)

	port := 8080

	err = fmt.Errorf("Err is: %s to port %d", sampleErr, port)
	fmt.Println(err)

	e := employee{
		Name: "John",
	}

	err = fmt.Errorf("Employee not found. Details %v", e)
	fmt.Println(err)
	err = fmt.Errorf("Employee not found. Details %+v", e)
	fmt.Println(err)
	err = fmt.Errorf("Employee not found. Details %#v", e)
	fmt.Println(err)
}
```

**输出**

```go
Err is: database connection issue
Err is: database connection issue to port 8080
Employee not found. Details {John 0}
Employee not found. Details {Name:John Age:0}
Employee not found. Details main.employee{Name:"John", Age:0}
```

请注意下面的**Errorf**

```go
err = fmt.Errorf("Err is: %s to port %d", sampleErr, port)
```

+   **%s**被**sampleErr**替换。

+   **%d**被**port**替换。

所以基本上格式字符串参数中的符号或动词按顺序被后续参数替换

如果格式字符串中的格式说明符数量与后续的变量参数数量不匹配，则格式说明符将原样打印。例如，在下面的代码中，我们有两个格式说明符。

+   %s

+   %d

而下一个变量参数的数量仅为一个。因此，当我们格式化它时，它返回带有第二个格式说明符的格式化错误，原样显示 MISSING 作为警告。

```go
package main

import "fmt"

func main() {
	name := "John"

	err := fmt.Errorf("Employee not found with name: %s and age %d", name)
	fmt.Println(err)
}
```

**输出**

```go
Employee not found with name: John and age %!d(MISSING)
```

## **错误的封装**

以下是封装错误的语法。

```go
e := fmt.Errorf("... %w ...", ..., err, ...)
```

**%w**指令用于封装错误。**fmt.Errorf**应该仅使用一个%w 指令调用。让我们看一个例子。

```go
package main

import (
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happened"
}

func main() {

	e1 := errorOne{}

	e2 := fmt.Errorf("E2: %w", e1)

	e3 := fmt.Errorf("E3: %w", e2)

	fmt.Println(e2)

	fmt.Println(e3)

}
```

**输出**

```go
E2: Error One happened
E3: E2: Error One happened
```

在上面的程序中，我们创建了一个名为**errorOne**的结构体，它具有一个**Error**方法，因此它实现了**error**接口。然后我们创建了一个名为**e1**的**errorOne**结构体实例。接着我们将该实例**e1**封装到另一个错误**e2**中，如下所示。

```go
e2 := fmt.Errorf("E2: %w", e1)
```

然后我们将**e2**封装到**e3**中，如下所示。

```go
e3 := fmt.Errorf("E3: %w", e2)
```

因此我们创建了一个错误层次结构，其中**e3**封装了**e2**，而**e2**封装了**e1**。因此，**e3**也以传递方式封装了**e1**。当我们打印**e2**时，它也会打印来自**e1**的错误并给出输出。

```go
E2: Error One happened
```

当我们打印**e3**时，它打印来自**e2**以及**e1**的错误并给出输出。

```go
E3: E2: Error One happened
```

你可以在这里详细了解错误的封装和解封装。

[`golangbyexample.com/wrapping-and-unwrapping-error-golang/`](https://golangbyexample.com/wrapping-and-unwrapping-error-golang/)

这就是关于 Errorf 函数的所有内容。希望你喜欢这篇文章。请在评论中分享反馈。同时，查看我们的 Golang 高级教程系列 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [errorf](https://golangbyexample.com/tag/errorf/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
