<!--yml

分类：未分类

日期：2024-10-13 06:30:21

-->

# Go程序的执行顺序

> 来源：[https://golangbyexample.com/order-execution-program-golang/](https://golangbyexample.com/order-execution-program-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

下面是Go程序的执行顺序。

+   程序从主包开始。

+   主包源文件中所有导入的包都被初始化。同样的事情也会递归发生在进一步导入的包中。

+   然后这些包中的全局变量声明被初始化。初始化依赖关系将启动这些变量的初始化。[https://golang.org/ref/spec#Order_of_evaluation](https://golang.org/ref/spec#Order_of_evaluation)

+   之后，这些包中的init()函数将被执行。

+   主包中的全局变量被初始化。

+   如果存在，主包中的init函数将被运行。

+   主包中的main函数被执行。

请注意，即使包被多次导入，包的初始化也只会执行一次。

例如，如果主包导入包**a**，而包**a**又导入包**b**，那么以下是执行顺序。

+   包b中的全局变量将被初始化。包b源文件中的init函数将被执行。

+   包**a**中的全局变量将被初始化。包**a**源文件中的init函数将被执行。

+   主包中的全局变量将被初始化。主包源文件中的init函数将被执行。

+   main函数将开始执行。

# **示例**

让我们看看一个相同的程序。

**go.mod**

```go
module sample

go 1.14
```

**sample/b/b1.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b1")
}

func TestB() error {
	return nil
}
```

**sample/b/b2.go**

```go
package b

import (
	"fmt"
)

func init() {
	fmt.Println("Init: b2")
}
```

**sample/a/a1.go**

```go
package a

import (
	"fmt"
	"sample/b"
)

func init() {
	fmt.Println("Init: a1")
}

func TestA() error {
	return b.TestB()
}
```

**sample/a/a2.go**

```go
package a

import (
	"fmt"
)

func init() {
	fmt.Println("Init: a2")
}
```

**sample/main.go**

```go
package main

import (
	"fmt"
	"sample/a"
)

func init() {
	fmt.Println("Init: main")
}
func main() {
	fmt.Println("Main Function Executing")
	a.TestA()
}
```

**输出**

```go
Init: b1
Init: b2
Init: a1
Init: a1
Init: main
Main Function Executing
```

注意在上述示例中，包**b**的源文件中的init函数首先被执行。然后包**a**的源文件中的init函数被执行，最后主包的源文件中的init函数被执行。之后main函数被执行。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
