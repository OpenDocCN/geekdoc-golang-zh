<!--yml

类别：未分类

日期：2024-10-13 06:25:07

-->

# 在 Go（Golang）中错误的包装和解包

> 来源：[`golangbyexample.com/wrapping-and-unwrapping-error-golang/`](https://golangbyexample.com/wrapping-and-unwrapping-error-golang/)

目录

**   错误的包装

+   解包错误

# **错误的包装**

在 Go 中，错误可以包装另一个错误。错误的包装意味着什么？它意味着创建一个错误层次结构，其中特定实例的错误包装另一个错误，而该特定实例本身也可以被包装在另一个错误中。下面是包装错误的语法。

```go
e := fmt.Errorf("... %w ...", ..., err, ...)
```

**%w**指令用于包装错误。**fmt.Errorf**应仅调用一个%w 指令。让我们看一个例子。

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

在上面的程序中，我们创建了一个结构体**errorOne**，它有一个**Error**方法，因此实现了**error**接口。然后我们创建了一个名为**e1**的**errorOne**结构实例。接着我们将该实例**e1**包装成另一个错误**e2**，如下所示。

```go
e2 := fmt.Errorf("E2: %w", e1)
```

然后我们将**e2**包装到**e3**中，如下所示。

```go
e3 := fmt.Errorf("E3: %w", e2)
```

因此，我们创建了一个错误层次结构，其中**e3**包装**e2**，**e2**又包装**e1**。因此，**e3**也间接地包装了**e1**。当我们打印**e2**时，它也会打印**e1**的错误并给出输出。

```go
E2: Error One happended
```

当我们打印**e3**时，它会打印**e2**和**e1**的错误并给出输出。

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

在上面的程序中，我们有一个**checkPostiveAndEven**函数，用于检查一个数字是否为偶数和正数。它依次调用**checkEven**函数来检查该数字是否为偶数。然后它调用**checkPositive**函数来检查该数字是否为正数。如果一个数字既不是偶数也不是正数，就会引发错误。

在上面的程序中，不可能知道错误的堆栈跟踪。我们知道这个错误来自**checkEven**函数的输出。但是调用**checkEven**函数的具体函数从错误中并不清楚。这就是包装错误的意义所在。当项目变大并且有很多函数相互调用时，这就变得更加有用。让我们通过包装错误来重写程序。

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

上面的程序与之前的程序相同，只是在**checkPostiveAndEven**函数中，我们像下面这样包装错误。

```go
fmt.Errorf("checkPostiveAndEven: %w", err)
```

因此，输出更加清晰，错误信息也更加丰富。输出清楚地提到调用的顺序。

```go
checkPostiveAndEven: checkEven: Given number 3 is not an even number
```

# **解包错误**

在上面的部分，我们研究了如何包装错误。也可以解包错误。**解包**函数来自**errors**包，可以用来解包错误。下面是该函数的语法。

```go
func Unwrap(err error) error
```

如果**err**封装了另一个错误，则将返回封装的错误，否则**Unwrap**函数将返回 nil。

让我们看看一个程序来说明这一点

```go
import (
    "errors"
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

在上面的程序中，我们创建了一个结构体**errorOne**，它有一个**Error**方法，因此它实现了**error**接口。然后我们创建了一个名为**e1**的**errorOne**结构体实例。接着我们将该实例**e1**封装成另一个错误**e2**，如下所示。

```go
e2 := fmt.Errorf("E2: %w", e1)
```

然后我们将**e2**封装成**e3**，如下所示。

```go
e3 := fmt.Errorf("E3: %w", e2)
```

因此

```go
fmt.Println(errors.Unwrap(e3))
```

将返回封装的错误**e2**，因为**e3**封装了**e2**，输出将是

```go
E2: Error One happended
```

另外，

```go
fmt.Println(errors.Unwrap(e2))
```

将返回封装的错误**e1**，因为**e2**进一步封装了**e1**，输出将是

```go
Error One happened
```

当

```go
fmt.Println(errors.Unwrap(e1))
```

将输出 nil，因为**e1**并不封装任何错误

```go
{nil}
```


