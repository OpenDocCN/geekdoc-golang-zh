<!--yml

类别：未分类

日期：2024-10-13 06:30:49

-->

# 在 Go (Golang) 中导入同名包或在导入包时使用别名

> 来源：[https://golangbyexample.com/import-same-package-name-golang/](https://golangbyexample.com/import-same-package-name-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

导入包时的别名意味着给导入的包一个不同的名称。其语法为

```go
import <new_name> <directory_path></directory_path></new_name>
```

上述语句的意思是，无论 <directory_path> 目录中存在什么包，都用 <new_name> 导入该包。别名对于命名非常有用。

+   在当前上下文中给导入的包一个更相关的名称。

+   当两个不同的导入路径包含相同的包名时，将其中一个作为不同的名称导入以防止冲突。

# **示例**

创建一个模块，导入路径为 [sample.com](http://sample.com)/learn

```go
go mod init sample.com/learn
```

在下面的示例中，我们创建了两个目录 math 和 math2。

+   math 目录包含文件 math.go，包声明为

```go
package math
```

+   math2 目录包含文件 math2.go，包声明为

请注意，包名（即 **math**）在 **math** 文件夹和 **math2\.** 文件夹中是相同的。因此，两个文件夹 **math2** 和 **math** 都包含相同的包，即 **math.** 由于这两个目录具有相同的包名，别名是将两个包用于同一文件的唯一方法。这就是我们在 main.go 中如何别名和使用两个包。

```go
import (
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
```

我们将 **math** 包在 **“sample.com/learn/math2”** 中别名为 **math2\.** 如果没有这样做，GO 将会引发编译问题，因为它无法从两个不同的文件夹导入同名的包。这是使用别名的一个优点。

让我们看看完整的工作代码

**go.mod**

```go
module sameple.com/learn

go 1.14
```

**learn/math2/math2.go**

```go
package math
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/math.go**

```go
package math
func Add(a, b int) int {
    return a + b
}
```

**learn/main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/math"
    math2 "sample.com/learn/math2"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math2.Subtract(2, 1))
}
```

让我们运行这个程序

```go
go install
learn $ learn
3
1
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
