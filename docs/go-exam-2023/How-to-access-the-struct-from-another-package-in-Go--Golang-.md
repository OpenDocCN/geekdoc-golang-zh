<!--yml

类别：未分类

日期：2024-10-13 06:34:44

-->

# 如何在Go（Golang）中从另一个包访问结构体

> 来源：[https://golangbyexample.com/struct-another-package-golang/](https://golangbyexample.com/struct-another-package-golang/)。

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *# **概述**

另一个包中的结构体名称必须以大写字母开头，这样它才能在包外公开。如果结构体名称以小写字母开头，则在包外不可见。

要在包外访问一个结构体，我们需要先导入包含该结构体的包。

# **程序**

这是相应的代码

**go.mod**

```go
module sample.com/learn
go 1.16
```

**model/person.go**

```go
package model
type Person struct {
    Name string
}
```

**main.go**

```go
package main
import (
    "fmt"
    "sample.com/learn/person"
)
func main() {
    p := &model.Person{
        Name: "John",
    }
    fmt.Println(p.Name)
}
```

在这个程序中，我们首先从**main**包导入**model**包，如下所示。

```go
"sample.com/learn/model"
```

然后我们从**main**包访问**Person**结构体如下。

```go
p := &model.Person{
   Name: "John",
}
```

之所以可行，是因为**Person**结构体名称是大写的。

让我们将结构体名称改为小写并运行这个程序。它会产生以下编译错误

```go
cannot refer to unexported name model.person
```

另请查看我们的Golang综合教程系列 – [Golang综合教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
