<!--yml

分类: 未分类

日期: 2024-10-13 06:24:54

-->

# 在Go (Golang)中比较错误或错误相等

> 来源：[https://golangbyexample.com/comparing-error-go/](https://golangbyexample.com/comparing-error-go/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

首先，什么是错误的相等性？正如您所知道的，错误在Go中由错误接口表示。在Go中，如果两个接口是相等的

+   两者指向相同的底层类型

+   底层值相等（或两个都是nil）

所以上述两点也适用于比较错误。有两种方法可以检查给定的错误是否相等。

+   使用相等运算符 (==)

+   使用错误包的Is函数 – [https://golang.org/pkg/errors/](https://golang.org/pkg/errors/)。 使用Is函数优于使用相等运算符，因为它通过逐步解包第一个错误来检查相等性，并在每一步解包时与目标错误匹配。稍后我们将看到一个例子，以充分理解为什么它更可取。下面是Is函数的语法。

```
func Is(err, target error) bool
```

# **代码**

让我们看一个例子

```
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

```
Equality Operator: Both errors are equal
Is function: Both errors are equal
```

在上面的程序中，我们创建了**errorOne**结构，它定义了**Error**方法，从而实现了**error**接口。我们创建了**err1**变量，它是**errorOne**结构的一个实例。我们还创建了一个**do()**函数，该函数引发**errorOne**类型的错误，并在主函数中捕获到**err2**变量中。

然后我们使用

+   使用相等运算符

```
err1 == err2
```

+   使用错误包的**Is**函数

```
errors.Is(err1, err2)
```

这两种方法都正确输出错误是相等的，因为**err1**和**err2**都相同。

+   指向相同的底层类型，即**errorOne**

+   具有相同的底层值

我们在上面提到，使用**Is**函数优于使用相等运算符，因为它通过逐步解包第一个错误来检查相等性，并在每一步解包时与目标错误匹配。让我们看一个例子。

```
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

```
Equality Operator: Both errors are not equal
Is function: Both errors are equal
```

上面的程序几乎与前一个程序相同，唯一的区别在于在**do()**函数中我们也包装了错误。

```
return fmt.Errorf("E2: %w", errorOne{})
```

+   相等运算符输出

```
Equality Operator: Both errors are not equal
```

+   当**Is**函数输出时

```
Is function: Both errors are equal
```

这是因为返回的**err2**包装了一个**errorOne**实例，而相等运算符无法捕获，但**Is**函数可以捕获。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
