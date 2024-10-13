<!--yml

类别：未分类

日期：2024-10-13 06:30:53

-->

# Go 中的 Goto 语句

> 来源：[`golangbyexample.com/goto-statement-go/`](https://golangbyexample.com/goto-statement-go/)

目录

+   概述

+   示例 

# **概述**

Goto 语句允许无条件跳转到同一函数中的标记语句。下面是 goto 语句的格式

```go
goto label
...
...
label: statement
```

标签可以是有效的 go 语句，不能是关键字。一旦遇到 goto 语句，控制就会转移到指定的标签，并从那里继续执行。标签仅在声明它的函数内部可见，函数外部的任何引用都会导致编译错误。

# **示例**

让我们看看一个简单的 goto 语句示例

```go
package main
import "fmt"
func main() {
    learnGoTo()
}
func learnGoTo() {
    fmt.Println("a")
    goto FINISH
    fmt.Println("b")
FINISH:
    fmt.Println("c")
}
```

**输出**

```go
a
c
```

在上面的示例中，我们有一个**goto**语句，如下所示。

```go
goto FINISH
```

**FINISH**标签如下

```go
FINISH:
    fmt.Println("c")
```

一旦程序遇到**goto**语句，它就会跳转到指定的标签。因此，下面的代码行永远不会被执行，**b**也不会被打印。

```go
fmt.Println("b")
```

标签和 goto 需要属于同一函数，否则会引发编译错误。这是因为标签的作用域在声明它的函数内部。如上所述，下面的程序会引发编译错误。

```go
./main.go:11:7: label FINISH not defined
./main.go:17:1: label FINISH defined and not used
```

```go
package main

import "fmt"

func main() {
	learnGoTo()
}

func learnGoTo() {
	fmt.Println("a")
	goto FINISH
	fmt.Println("b")

}

func test() {
FINISH:
	fmt.Println("c")
}
```

标签也可以在 goto 语句之前。见下面的示例。该程序可以用于打印 10 之前的所有奇数。注意标签**START**在这里位于 goto 之前。

```go
package main
import "fmt"
func main() {
    learnGoTo()
}
func learnGoTo() {
    i := 0
    fmt.Println("here")
START:
    for i < 10 {
        if i%2 == 0 {
            i++
            goto START
        }  
        fmt.Println(i)
        i++
    }
}
```

不建议使用 Goto，因为可读性差，通常也是许多错误的来源。任何通过 goto 实现的功能都可以通过其他 go 结构来实现。

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/) *
