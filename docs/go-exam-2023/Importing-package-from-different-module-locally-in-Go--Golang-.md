<!--yml

类别：未分类

日期：2024-10-13 06:29:27

-->

# 在 Go (Golang) 中从不同模块本地导入包

> 来源：[`golangbyexample.com/import-local-module-golang/`](https://golangbyexample.com/import-local-module-golang/)

有些情况下，我们希望导入一个本地存在的模块。让我们理解如何导入这样的模块。但首先，我们必须创建一个可以被他人使用的模块，然后将其导入到其他模块中。为此，让我们创建两个模块

+   **sample.com/math**模块

+   **school**模块

**school**模块将调用**sample.com/math**模块的代码

首先创建**sample.com/math**模块，该模块将被**school**模块使用

+   创建一个**math**目录

+   创建一个导入路径为**sample.com/math**的模块

```go
go mod init sample.com/math
```

+   在**math**目录中创建一个名为**math.go**的文件，内容如下

```go
package math

func Add(a, b int) int {
	return a + b
}
```

现在让我们创建 school 模块

+   在与**math**目录并排的同一路径下创建一个**school**目录

+   创建一个模块名为**school**

```go
go mod init school
```

+   现在让我们修改**go.mod**文件以在 school 模块中导入 math 模块。要导入一个未推送到版本控制系统的本地模块，我们将使用替换目录。替换目录将用你指定的路径替换模块路径。

```go
module school

go 1.14

replace sample.com/math => ../math
```

+   创建文件**school.go**，该文件将使用**[sample.com](http://sample.com)/math**模块中的**Add**函数

```go
package main

import (
	"fmt"
	"sample.com/math"
)

func main() {
	fmt.Println(math.Add(2, 4))
}
```

现在执行 go run

```go
go run school.go
```

它能够调用**[sample.com](http://sample.com)/math**模块的 Add 函数，并正确输出为 6。

此外，它还将更新**go.mod**，并包含**[sample.com](http://sample.com)/math**模块的版本信息

```go
module school

go 1.14

replace sample.com/math => ../math

require sample.com/math v0.0.0-00010101000000-000000000000
```

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)
