<!--yml

分类：未分类

日期：2024-10-13 06:29:32

-->

# 在Go（Golang）中导入同一模块内的包

> 来源：[https://golangbyexample.com/importing-package-same-module-go/](https://golangbyexample.com/importing-package-same-module-go/)

同一模块内的任何包都可以使用模块的导入路径加上包含该包的目录来导入。为了说明，让我们创建一个模块

+   创建一个**learn**目录

+   创建一个导入路径为**“learn”**的模块

```go
go mod init learn
```

+   现在创建main.go（具有主包和主函数）

+   以及math/math.go – math包

**main.go**

```go
package main

import (
	"fmt"
	"learn/math"
)

func main() {
	fmt.Println(math.Add(1, 2))
}
```

**math/math.go**

```go
package math

func Add(a, b int) int {
    return a + b
}
```

查看我们如何在main.go文件中导入math包

```go
"learn/math"
```

这里的导入路径是模块的导入路径**learn**加上包含该包的目录**math**。因此是“learn/math”。嵌套目录中的包也可以以同样的方式导入。其工作原理是由于前缀是模块导入路径，因此Go会知道你试图从同一模块中导入。这样它会直接引用，而不是下载。

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)
