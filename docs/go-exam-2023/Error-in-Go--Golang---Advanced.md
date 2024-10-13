<!--yml

类别: 未分类

日期: 2024-10-13 06:25:25

-->

# Go中的错误（Golang）- 高级

> 来源：[https://golangbyexample.com/error-in-golang-advanced/](https://golangbyexample.com/error-in-golang-advanced/)

这是golang综合教程系列的第27章。有关系列其他章节的信息，请参考这个链接 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [恐慌与恢复](https://golangbyexample.com/panic-and-recover-golang/)

**上一个教程** – [错误 – 第1部分](https://golangbyexample.com/error-in-golang/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**   [概述](#Overview "Overview")

+   [错误的包装](#Wrapping_of_error "Wrapping of error")

+   [解包错误](#Unwrap_an_error "Unwrap an error")

+   [检查两个错误是否相等](#Check_if_two_error_are_equal "Check if two error are equal")

    +   [使用等于运算符 (==)](#Using_the_equality_operator "Using the equality operator (==)")

    +   [使用错误包的Is函数](#Using_the_Is_function_of_errors_package "Using the Is function of errors package")

+   [从错误接口表示的错误中获取基础错误](#Get_the_underlying_error_from_an_error_represented_by_the_error_interface "Get the underlying error from an error represented by the error interface")

    +   [使用.({type})断言](#Using_the_type_assert "Using the .({type}) assert")

    +   [使用错误包的As函数](#Using_the_As_function_of_errors_package "Using the As function of errors package")

+   [结论](#Conclusion "Conclusion")*  *# **概述**

在这篇文章中，我们将涵盖与go中错误相关的高级主题。

+   错误的包装与解包

+   错误比较

+   从错误中提取基础类型

+   **As**和**Is**函数的错误包

请首先参考下面的链接，该链接介绍了**go中的错误**的基础知识。

[https://golangbyexample.com/error-in-golang/](https://golangbyexample.com/error-in-golang/)

这篇文章将涵盖有关错误的基本内容，例如：

+   错误概述

+   错误接口

+   创建错误的不同方法。

+   忽略错误

# **错误的包装**

在go中，错误也可以包装另一个错误。

错误的包装是什么意思？这意味着创建一个错误层级，其中一个特定实例的错误包装了另一个错误，并且该特定实例本身也可以被包装在另一个错误中。下面是包装错误的语法。

```go
e := fmt.Errorf("... %w ...", ..., err, ...)
```

**%w**指令用于包装错误。 **fmt.Errorf**应仅与一个 %w 指令一起调用。让我们看一个例子。

```go
package main

import (
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happended"
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
E2: Error One happended
E3: E2: Error One happended
```

在上面的程序中，我们创建了一个结构体**errorOne**，它有一个**Error**方法，因此实现了**error**接口。然后我们创建了一个名为**e1**的**errorOne**结构体实例。接着我们将该实例**e1**包装到另一个错误**e2**中，如下所示。

```go
e2 := fmt.Errorf("E2: %w", e1)
```

然后我们将**e2**包装到**e3**中，如下所示。

```go
e3 := fmt.Errorf("E3: %w", e2)
```

因此我们创建了一个错误的层次结构，其中**e3**包装了**e2**，而**e2**又包装了**e1**。因此**e3**也通过传递方式包装了**e1**。当我们打印**e2**时，它也会打印来自**e1**的错误，并给出输出。

```go
E2: Error One happended
```

当我们打印**e3**时，它会打印来自**e2**和**e1**的错误，并给出输出。

```go
E3: E2: Error One happended
```

现在脑海中出现的问题是，包装错误的用例是什么。为了理解这一点，让我们看一个例子。

```go
package main

import (
	"fmt"
)

type notPositive struct {
	num int
}

func (e notPositive) Error() string {
	return fmt.Sprintf("checkPositive: Given number %d is not a positive number", e.num)
}

type notEven struct {
	num int
}

func (e notEven) Error() string {
	return fmt.Sprintf("checkEven: Given number %d is not an even number", e.num)
}

func checkPositive(num int) error {
	if num < 0 {
		return notPositive{num: num}
	}
	return nil
}

func checkEven(num int) error {
	if num%2 == 1 {
		return notEven{num: num}
	}
	return nil
}

func checkPostiveAndEven(num int) error {
	if num > 100 {
		return fmt.Errorf("checkPostiveAndEven: Number %d is greater than 100", num)
	}

	err := checkPositive(num)
	if err != nil {
		return err
	}

	err = checkEven(num)
	if err != nil {
		return err
	}

	return nil
}

func main() {
	num := 3
	err := checkPostiveAndEven(num)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("Givennnumber is positive and even")
	}

}
```

**输出**

```go
checkEven: Given number 3 is not an even number
```

在上述程序中，我们有一个函数**checkPostiveAndEven**，它检查一个数字是否是偶数且为正数。它会调用**checkEven**函数来检查数字是否为偶数。然后它会调用**checkPositive**函数来检查数字是否为正数。如果一个数字既不是偶数也不是正数，就会引发错误。

在上述程序中，无法确定错误的堆栈跟踪。我们知道这个错误来自于**checkEven**函数，但哪个函数调用了**checkEven**函数在错误中并不清楚。这就是包装错误的重要性。当项目较大且有许多函数相互调用时，这变得更加有用。让我们通过包装错误来重写程序。

```go
package main

import (
	"fmt"
)

type notPositive struct {
	num int
}

func (e notPositive) Error() string {
	return fmt.Sprintf("checkPositive: Given number %d is not a positive number", e.num)
}

type notEven struct {
	num int
}

func (e notEven) Error() string {
	return fmt.Sprintf("checkEven: Given number %d is not an even number", e.num)
}

func checkPositive(num int) error {
	if num < 0 {
		return notPositive{num: num}
	}
	return nil
}

func checkEven(num int) error {
	if num%2 == 1 {
		return notEven{num: num}
	}
	return nil
}

func checkPostiveAndEven(num int) error {
	if num > 100 {
		return fmt.Errorf("checkPostiveAndEven: Number %d is greater than 100", num)
	}

	err := checkPositive(num)
	if err != nil {
		return fmt.Errorf("checkPostiveAndEven: %w", err)
	}

	err = checkEven(num)
	if err != nil {
		return fmt.Errorf("checkPostiveAndEven: %w", err)
	}

	return nil
}

func main() {
	num := 3
	err := checkPostiveAndEven(num)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("Given number is positive and even")
	}

}
```

**输出**

```go
checkPostiveAndEven: checkEven: Given number 3 is not an even number
```

上述程序与之前的程序相同，只是在**checkPostiveAndEven**函数中，我们像下面这样包装错误。

```go
fmt.Errorf("checkPostiveAndEven: %w", err)
```

所以输出更加清晰，错误信息也更加详细。输出清楚地提到了调用的顺序。

```go
checkPostiveAndEven: checkEven: Given number 3 is not an even number
```

# **解包错误**

在上节中，我们研究了如何包装错误。同样也可以解包错误。错误包的Unwrap函数可以用来解包错误。下面是该函数的语法。

```go
func Unwrap(err error) error
```

如果**err**包装了另一个错误，那么将返回被包装的错误，否则**Unwrap**函数将返回nil。

让我们看一个程序来说明同样的情况。

```go
import (
    "errors"
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
    fmt.Println(errors.Unwrap(e3))
    fmt.Println(errors.Unwrap(e2))
    fmt.Println(errors.Unwrap(e1))
}
```

**输出**

```go
E2: Error One happended
Error One happended 
```

在上述程序中，我们创建了一个结构体**errorOne**，它有一个**Error**方法，因此它实现了**error**接口。然后我们创建了一个名为**e1**的**errorOne**结构体实例。接着，我们将该实例**e1**包装到另一个错误**e2**中，如下所示。

```go
e2 := fmt.Errorf("E2: %w", e1)
```

然后我们将**e2**包装到**e3**中，如下所示。

```go
e3 := fmt.Errorf("E3: %w", e2)
```

因此。

```go
fmt.Println(errors.Unwrap(e3))
```

将返回被包装的错误**e2**，因为**e3**包装了**e2**，输出将是

```go
E2: Error One happened
```

同时。

```go
fmt.Println(errors.Unwrap(e1))
```

将输出nil，因为**e1**没有包装任何错误。

```go
{nil}
```

# **检查两个错误是否相等**

首先，错误的相等性是什么意思？正如你所知道的，错误在Go中由错误接口表示。在Go中，当两个接口相等时：

+   两者指向相同的基础类型。

+   基础值相等（或两个都是nil）

所以上述两个要点同样适用于比较错误。有两种方法可以检查给定的错误是否相等。

## 使用相等运算符（==）。

**==**运算符可以用于比较两个Go语言中的错误。

## 使用错误包的**Is**函数。

[https://golang.org/pkg/errors/](https://golang.org/pkg/errors/)。使用 **Is** 函数比使用等于运算符更可取，因为它通过顺序展开第一个错误来检查相等性，并在每一步展开时与目标错误进行匹配。稍后我们将看到一个例子，以充分理解为什么这更可取。下面是 Is 函数的语法。

```go
func Is(err, target error) bool
```

让我们看一个例子。

```go
package main
import (
    "errors"
    "fmt"
)
type errorOne struct{}
func (e errorOne) Error() string {
    return "Error One happended"
}
func main() {
    var err1 errorOne
    err2 := do()
    if err1 == err2 {
        fmt.Println("Equality Operator: Both errors are equal")
    }
    if errors.Is(err1, err2) {
        fmt.Println("Is function: Both errors are equal")
    }
}
func do() error {
    return errorOne{}
}
```

**输出**

```go
Equality Operator: Both errors are equal
Is function: Both errors are equal
```

在上面的程序中，我们创建了 **errorOne** 结构体，该结构体定义了 **Error** 方法，因此实现了 **error** 接口。我们创建了一个 **err1** 变量，它是 **errorOne** 结构体的一个实例。我们还创建了一个 **do()** 函数，该函数引发了一个 **errorOne** 类型的错误，并在主函数中捕获到 **err2** 变量中。

然后我们使用以下方法比较两个错误。

+   使用等于运算符

```go
err1 == err2
```

+   使用 errors 包的 **Is** 函数

```go
errors.Is(err1, err2)
```

两种方法都正确地输出错误是相等的，因为 **err1** 和 **err2** 相同。

+   引用相同的基础类型，即 **errorOne**。

+   拥有相同的基础值

我们在上面提到，使用 **Is** 函数比使用等于运算符更可取，因为它通过顺序展开第一个错误来检查相等性，并在每一步展开时与目标错误进行匹配。让我们看一个例子。

```go
package main

import (
	"errors"
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happended"
}

func main() {
	err1 := errorOne{}

	err2 := do()

	if err1 == err2 {
		fmt.Println("Equality Operator: Both errors are equal")
	} else {
		fmt.Println("Equality Operator: Both errors are not equal")
	}

	if errors.Is(err2, err1) {
		fmt.Println("Is function: Both errors are equal")
	}
}

func do() error {
	return fmt.Errorf("E2: %w", errorOne{})
}
```

**输出**

```go
Equality Operator: Both errors are not equal
Is function: Both errors are equal
```

上面的程序几乎与之前的程序相同，唯一的区别是，在 **do()** 函数中我们也在包装错误。

```go
return fmt.Errorf("E2: %w", errorOne{})
```

+   等于运算符的输出

```go
Equality Operator: Both errors are not equal
```

+   而 **Is** 函数的输出

```go
Is function: Both errors are equal
```

这是因为 **err2** 返回了一个封装 **errorOne** 的实例，等于运算符无法捕获，但 **Is** 函数能够捕获。

# **从 error 接口表示的错误中获取基础错误**

获取基础类型有两种方法

## 使用 .({type}) 断言

如果断言成功，则将返回相应的错误，否则将出现恐慌。下面是语法。

```go
err := err.({type})
```

最好使用 **ok** 变量来防止在断言失败时出现恐慌。下面是相应的语法。如果错误的基础类型正确，**ok** 变量将被设置为 true。

```go
err, ok := err.({type})
```

## **使用 errors 包的 As 函数**

[https://golang.org/pkg/errors/](https://golang.org/pkg/errors/)。使用 **As** 函数比使用 .({type}) 断言更可取，因为它通过顺序展开第一个错误来检查匹配，并在每一步展开时与目标错误进行匹配。下面是 Is 函数的语法。

```go
func As(err error, target interface{}) bool
```

**As** 函数将在第一个参数中找到第一个可以匹配目标的错误。一旦找到匹配项，它将把目标设置为该错误值。

让我们看一个例子。

```go
package main

import (
	"errors"
	"fmt"
	"os"
)

func main() {

	err := openFile("non-existing.txt")

	if e, ok := err.(*os.PathError); ok {
		fmt.Printf("Using Assert: Error e is of type path error. Path: %v\n", e.Path)
	} else {
		fmt.Println("Using Assert: Error not of type path error")
	}

	var pathError *os.PathError
	if errors.As(err, &pathError) {
		fmt.Printf("Using As function: Error e is of type path error. Path: %v\n", pathError.Path)
	}
}

func openFile(fileName string) error {
	_, err := os.Open("non-existing.txt")
	if err != nil {
		return err
	}
	return nil
}
```

**输出：**

```go
Using Assert: Error e is of type path error. Path: non-existing.txt
Using As function: Error e is of type path error. Path: non-existing.txt
```

在上面的程序中，我们有一个 openFile 函数，我们试图打开一个不存在的类型，因此它会引发错误。然后我们通过两种方式断言该错误。

+   使用 . 断言运算符。如果错误的基础类型是 ***os.PathError**，那么 **ok** 变量将被设置为 true，否则将被设置为 false。

```go
e,ok := err.(*os.PathError); ok
```

+   使用errors包的**As**函数

```go
errors.As(err, &pathError)
```

两种方法都正确地断言错误为类型***os.PathError**，因为**openFile**函数返回的错误是类型***os.PathError**。

我们上面提到，使用**As**函数比使用.({type})断言更可取，因为它通过顺序解包第一个错误并在每一步解包时与目标错误进行匹配。让我们看一个例子来理解这一点。

```go
import (
	"errors"
	"fmt"
	"os"
)

func main() {
	var pathError *os.PathError
	err := openFile("non-existing.txt")

	if e, ok := err.(*os.PathError); ok {
		fmt.Printf("Using Assert: Error e is of type path error. Error: %v\n", e)
	} else {
		fmt.Println("Using Assert: Error not of type path error")
	}

	if errors.As(err, &pathError) {
		fmt.Printf("Using As function: Error e is of type path error. Error: %v\n", pathError)
	}
}

func openFile(fileName string) error {
	_, err := os.Open("non-existing.txt")
	if err != nil {
		return fmt.Errorf("Error opening: %w", err)
	}
	return nil
}
```

**输出：**

```go
Using Assert: Error not of type path error
Using As function: Error e is of type path error. Error: open non-existing.txt: no such file or directory
```

上述程序与之前的程序几乎相同，唯一的区别在于在**openFile**函数中我们也对错误进行了包装。

```go
return fmt.Errorf("Error opening: %w", err)
```

+   该断言输出

```go
Using Assert: Error not of type path error
```

+   当**As**函数输出时

```go
Using As function: Error e is of type path error. Error: open non-existing.txt: no such file or directory
```

这是因为**openFile**函数返回的错误包装了***os.PathError**错误，该错误未被点（‘.’）断言捕获，但被**As**函数捕获。

# **结论**

这就是关于Golang中错误的高级主题。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一教程** – [恐慌与恢复](https://golangbyexample.com/panic-and-recover-golang/)

**上一个教程** – [错误 – 第 1 部分](https://golangbyexample.com/error-in-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
