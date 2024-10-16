<!--yml

类别：未分类。

日期：2024-10-13 06:25:00。

-->

# 从错误或 Go（Golang）中的错误断言获取底层类型。

> 来源：[`golangbyexample.com/error-assertion-golang/`](https://golangbyexample.com/error-assertion-golang/)

目录。

**   概述。

+   代码

# **概述**

获取底层类型有两种方式。

+   使用.({type})断言。

如果断言成功，它将返回相应的错误，否则将引发 panic。以下是语法。

```go
err := err.({type})
```

最好使用**ok**变量以防断言失败而导致 panic。以下是语法：如果错误的底层类型正确，**ok**变量将被设置为 true。

```go
err, ok := err.({type})
```

使用**errors**包的**As**函数 - [`golang.org/pkg/errors/`](https://golang.org/pkg/errors/)。使用**As**函数比使用.({type})断言更可取，因为它通过顺序解包第一个错误并在每一步解包时与目标错误进行匹配。以下是 Is 函数的语法。

```go
func As(err error, target interface{}) bool
```

**As**函数将在第一个参数中找到第一个可以匹配目标的错误。一旦找到匹配项，它将把目标设置为该错误值。

# **代码**。

让我们来看一个例子。

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

在上面的程序中，我们有一个 openFile 函数，其中我们尝试打开一个不存在的类型，因此会引发错误。然后我们以两种方式对错误进行断言。

+   使用.assert 运算符。如果底层错误类型是***os.PathError**，则 ok 变量将被设置为 true，否则将被设置为 false。

```go
e,ok := err.(*os.PathError); ok
```

+   使用 errors 包的**As**函数。

```go
errors.As(err, &pathError)
```

两种方法都正确地断言错误是类型为***os.PathError**，因为**openFile**函数返回的错误是类型为***os.PathError**。

我们上面提到，使用**As**函数比使用.({type})断言更可取，因为它通过顺序解包第一个错误并在每一步解包时与目标错误进行匹配。让我们看看一个例子来理解这一点。

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

上面的程序几乎与之前的程序相同，唯一的区别是我们在 openFile 函数中也包装了错误。

```go
return fmt.Errorf("Error opening: %w", err)
```

+   .assert 的输出。

```go
Using Assert: Error not of type path error
```

+   当 As 函数输出时。

```go
Using As function: Error e is of type path error. Error: open non-existing.txt: no such file or directory
```

这是因为**openFile**函数返回的错误包装了***os.Patherror**错误，而点（‘.’）断言没有捕获到该错误，但**As**函数捕获到了。


