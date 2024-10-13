<!--yml

类别：未分类

日期：2024-10-13 06:30:35

-->

# Go (Golang) 中的包名称和目录/文件夹名称 - 它们需要相同吗

> 来源：[`golangbyexample.com/package-folder-name-golang/`](https://golangbyexample.com/package-folder-name-golang/)

目录

**   概述

+   示例*  *# **概述**

包名称和包含该包的目录名称不一定必须相同。那么包的导入路径指定了什么呢？例如，如果我们有任何包，则导入路径可能如下所示：

```go
import "xyz.com/abc/sample"
```

上述陈述的本质是导入位于目录**“sample”**中的包。这并不意味着导入包**sample**。为了验证这一点，让我们看一个示例。

# **示例**

首先让我们创建一个名为**learn**的目录，然后创建一个导入路径为**“sample.com/learn”**的模块。

```go
go mod create sample.com/learn
```

让我们在**learn**目录中创建一个名为**math**的目录。在**math**目录中创建一个文件**math.go**，内容如下：

**learn/math/math.go**

```go
package mathematics

func Add(a, b int) int {
    return a + b
}
```

注意上面的包声明

```go
package mathematics
```

包名称是**数学**，但包含该包的目录名称是**math**。

现在让我们在**main.go**文件中使用**数学**包。

**learn/main.go**

```go
package main

import (
    "fmt"
    "sample.com/learn/math"
)

func main() {
    fmt.Println(mathematics.Add(2, 1))
}
```

在上面的**main.go**中，看看我们是如何导入包的。

```go
"sample.com/learn/math"
```

上述陈述的本质是导入位于目录**“math”**中的包。这并不意味着导入包**math**。

请查看在**main**函数中我们是如何使用该包的。

```go
fmt.Println(mathematics.Add(2, 1))
```

这就是我们所说的导入意味着导入位于该目录位置的包。

如果你运行这个程序，输出将是正确的。

```go
3
```

这表明包的名称不一定要与包含该包的目录名称相同。另一种方便的方法是在这种情况下使用包别名，如下所示。

```go
import (
    "fmt"
    mathematics "sample.com/learn/math"
)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
