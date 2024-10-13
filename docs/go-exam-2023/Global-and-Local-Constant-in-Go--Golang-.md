<!--yml。

类别：未分类。

日期：2024-10-13 06:27:53。

-->。

# Go（Golang）中的全局和局部常量。

> 来源：[https://golangbyexample.com/global-local-constant-golang/](https://golangbyexample.com/global-local-constant-golang/)。

目录。

**   [概述](#Overview "Overview")**。

+   [示例](#Example "Example")*  *# **概述**。

与其他变量一样，如果常量在文件顶部的任何函数范围外声明，则它在包内是全局的。例如，在下面的程序中，**name** 将是一个全局常量，可以在主包的任何函数中使用。请注意，const **name** 在主包外不可用。要使其在主包外可用，必须以大写字母开头。请查看下面的代码。它还显示了包内局部常量的示例。

# **示例**。

```
package main

import "fmt"

const name = "test"

func main() {
    const a = 8
    fmt.Println(a)
    testGlobal()
}

func testGlobal() {
    fmt.Println(name)
    //The below line will give compiler error as a is a local constant
    //fmt.Println(a)
}
```

**输出**。

```
8
test
```

让我们看一个示例，尝试从不同的包访问全局常量。假设。

+   **main.go** 位于 **$GOPATH/src/github.com/learn**。

+   **data.go** 位于 **$GOPATH/src/github.com/learn/data**。

**main.go**。

```
package main

import (
	"fmt"

	"github.com/learn/data"
)

func main() {
	val := data.PI
	fmt.Println(val)
}
```

**data.go**。

```
package data

const PI = 3.142
```

前往 **learn** 目录并运行命令。

```
go run main.go
```

**输出**。

```
3.142
```

在上述程序中，我们尝试从主包访问在 data.go 中定义的常量 PI。该程序正常工作，并成功打印出值，因为 const PI 以大写字母开头，因此它在其包外可见。

如果将常量名称从 **PI** 更改为 **pi**，则上述程序将引发编译错误。

```
cannot refer to unexported name data.pi
./main.go:11:7: undefined: data.pi
```

让我们看一个程序来说明这一点。

**main.go**。

```
package main

import (
	"fmt"

	"github.com/learn/data"
)

func main() {
	val := data.pi
	fmt.Println(val)
}
```

**data.go**。

```
package data

const pi = 3.142
```

前往 **learn** 目录并运行命令。

```
go run main.go
```

**输出**。

```
cannot refer to unexported name data.pi
./main.go:11:7: undefined: data.pi
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)。
