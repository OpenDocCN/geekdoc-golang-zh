<!--yml

分类：未分类

日期：2024-10-13 06:25:13

-->

# Go 中的错误 (Golang)

> 来源：[`golangbyexample.com/error-in-golang/`](https://golangbyexample.com/error-in-golang/)

这是 Golang 综合教程系列的第二十六章。有关系列的其他章节，请参考此链接 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一教程** – [错误 - 第二部分](https://golangbyexample.com/error-in-golang-advanced/)

**上一教程** – [选择语句](https://golangbyexample.com/select-statement-golang/)

现在让我们来看看当前的教程。以下是当前教程的目录。

目录

+   概述

+   使用实现错误接口的类型

+   将错误作为类型的优点

+   创建错误的不同方式

    +   使用 errors.New(“some_error_message”)")

    +   使用 fmt.Errorf(“error is %s”, “some_error_message”).. ")

    +   创建自定义错误

+   忽略错误

+   结论

# **概述**

在本教程中，我们将学习如何在 Golang 中进行错误处理。与其他传统语言相比，Go 并没有异常和 try-catch 来处理错误。

Golang 中的错误处理可以通过两种方式进行。

+   使用实现 **error** 接口的类型 –   这是一种传统的表示错误的方式，也是习惯用法。

+   使用 panic 和 recover

在本文中我们只讨论第一部分。

# **使用实现错误接口的类型**

Go 处理错误的方式是显式地将错误作为一个单独的值返回。

理解 Golang 中错误的关键是理解 **error** 接口。以下是 **builtin** 包中定义的 **error** 接口的样子。

[`golang.org/pkg/builtin/#error`](https://golang.org/pkg/builtin/#error)

```go
type error interface {
    Error() string
}
```

**error** 接口是 Golang 中表示错误条件的传统方式。如果它是 nil，则意味着没有错误。因此，在 Go 中，错误被视为一个值。这个值实现了 **error** 接口，可以传递给函数，可以从函数返回，也可以存储在变量中。

任何定义了**Error**方法的类型都被视为实现了**error**接口。正如我们之前提到的，Go 没有异常和 try-catch，因此处理错误条件的惯用方式是将错误作为最后一个返回值返回。然后可以检查该值是否为 nil。如果是 nil，则调用的函数没有返回错误，否则返回了错误。现在让我们看看一个演示我们刚才讨论内容的程序。

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	file, err := os.Open("non-existing.txt")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(file.Name() + "opened succesfully")
	}
}
```

**输出**

```go
open non-existing.txt: no such file or directory
```

在上面的程序中，我们调用**os.Open**函数打开一个不存在的文件。由于该文件不存在，它在输出中给出了上述错误消息。但是，这条错误消息是从哪里来的呢？让我们理解一下。

以下是**os.Open**函数的签名。

```go
func Open(name string) (*File, error)
```

它接受一个参数作为输入，即要打开的文件路径。它有两个返回值。

+   如果文件存在，则返回文件结构的指针。

+   如果有错误，则返回类型为***PathError**，它实现了**error**接口。

请查看**PathError**结构体类型这里- [`golang.org/src/os/error.go`](https://golang.org/src/os/error.go)

```go
type PathError struct {
   Op   string
   Path string
   Err  error
}
```

**PathError**结构体的指针定义了**Error()**方法。

```go
func (e *PathError) Error() string { return e.Op + " " + e.Path + ": " + e.Err.Error() }
```

由于它定义了**Error**方法，因此***PathError**实现了**error**接口。因此，**error**可以作为**os.Open**函数的第二个返回值返回。现在注意，在**main**函数中，我们通过仅与 nil 比较来检查错误的存在。

我们之所以与 nil 进行比较，是因为接口的默认零值是 nil，且**error**也是接口，其默认零值也是 nil。

另外，**fmt.Println**函数内部检查传递给它的类型是否实现了**error**接口。如果是，则会调用该类型的**Error**方法。这就是这一行的意思。

```go
fmt.Println(err)
```

输出结果

```go
open non-existing.txt: no such file or directory
```

**使用任何类型作为错误**

虽然我们在 Go 中有一个错误接口，并且我们期望 Go 中的每个错误都遵循这个接口，如上所见，但这仍然不是强制要求。在 Go 中，任何类型都可以被视为错误，但通常不推荐这样做，这也不是 Go 中惯用的编码方式。我们在这里提及是为了完整性和讨论。

# **使用错误作为类型的优点**

+   它允许更好地控制错误处理。可以在每一步检查错误。

+   避免了 try-catch 和异常处理的丑陋代码。

# **创建错误的不同方法**

现在让我们看看创建**错误**的不同方法。

## **使用 errors.New("some_error_message")**

```go
package main

import (
    "errors"
    "fmt"
)

func main() {
    sampleErr := errors.New("error occured")
    fmt.Println(sampleErr)
```

**输出：**

```go
error occured
```

## **使用 fmt.Errorf("error is %s", "some_error_message")。**

这种方式创建格式化的错误消息。

```go
package main

import (
    "fmt"
)

func main() {
    sampleErr := fmt.Errorf("Err is: %s", "database connection issue")
    fmt.Println(sampleErr)
}
```

**输出：**

```go
Err is: database connection issue
```

## **创建自定义错误**

下面的示例说明了自定义错误的使用。在下面的示例中。

+   inputError 是一个具有 Error()方法的结构体，因此它属于错误接口类型。

+   你还可以通过扩展其字段或添加新方法来为自定义错误添加额外信息。inputError 有一个名为**missingFields**的额外字段和一个**getMissingFields**函数。

+   我们可以使用类型断言将**错误**转换为**输入错误**。

**示例：**

```go
package main

import "fmt"

type inputError struct {
    message      string
    missingField string
}

func (i *inputError) Error() string {
    return i.message
}

func (i *inputError) getMissingField() string {
    return i.missingField
}

func main() {
    err := validate("", "")
    if err != nil {
        if err, ok := err.(*inputError); ok {
            fmt.Println(err)
            fmt.Printf("Missing Field is %s\n", err.getMissingField())
        }
    }
}

func validate(name, gender string) error {
    if name == "" {
        return &inputError{message: "Name is mandatory", missingField: "name"}
    }
    if gender == "" {
        return &inputError{message: "Gender is mandatory", missingField: "gender"}
    }
    return nil
}
```

**输出：**

```go
Name is mandatory
Missing Field is name
```

# **忽略错误**

下划线（‘_’）操作符可用于忽略函数调用返回的错误。在查看程序之前，重要的是要注意错误绝不应该被忽略。这并不是推荐的做法。让我们来看一个程序。

```go
package main
import (
    "fmt"
    "os"
)
func main() {
    file, _ := os.Open("non-existing.txt")
    fmt.Println(file)
}
```

**输出**

```go
{nil}
```

在上述程序中，我们使用了下划线操作符来忽略打开不存在的文件时的错误。这就是为什么函数返回的文件实例是 nil。因此，在使用函数返回的任何其他参数之前，最好检查是否有错误，因为它可能是 nil，这会导致不必要的问题，有时还可能导致恐慌。

# **结论**

这就是关于 golang 中错误的全部内容。我们将在下一章讨论与错误相关的高级主题。

希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [错误 - 第二部分](https://golangbyexample.com/error-in-golang-advanced/)

**上一篇教程** – [选择语句](https://golangbyexample.com/select-statement-golang/)


