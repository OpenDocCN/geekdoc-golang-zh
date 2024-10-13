<!--yml

分类：未分类

日期：2024-10-13 06:34:01

-->

# 验证 Go（Golang）中结构体中整数的范围

> 来源：[`golangbyexample.com/range-int-struct-validate-golang/`](https://golangbyexample.com/range-int-struct-validate-golang/)

目录

**   概述

+   示例*  *# **概述**

下面的库可用于验证 Golang 中结构体中整数的范围

+   **gopkg.in/go-playground/validator.v9** – [`pkg.go.dev/github.com/go-playground/validator`](https://pkg.go.dev/github.com/go-playground/validator)

在本教程中，我们将使用以下员工结构体

```go
type employee struct {
    Age int
}
```

# **示例**

让我们看一个相同的例子。以下是代码

**go.mod**

```go
module sample.com/validate
go 1.14
require (
    github.com/go-playground/universal-translator v0.17.0 // indirect
    github.com/leodido/go-urn v1.2.1 // indirect
    gopkg.in/go-playground/assert.v1 v1.2.1 // indirect
    gopkg.in/go-playground/validator.v9 v9.31.0
)
```

**main.go**

```go
package main
import (
    "fmt"
    "gopkg.in/go-playground/validator.v9"
)
var validate *validator.Validate
type employee struct {
    Age int `validate:"required,gte=10,lte=20"`
}
func main() {
    e := employee{}
    err := validateStruct(e)
    if err != nil {
        fmt.Printf("Error: %s\n", err)
    }
    e = employee{Age: 5}
    err = validateStruct(e)
    if err != nil {
        fmt.Printf("Error: %s\n", err)
    }
    e = employee{Age: 25}
    err = validateStruct(e)
    if err != nil {
        fmt.Printf("Error: %s\n", err)
    }
}
func validateStruct(e employee) error {
    validate = validator.New()
    err := validate.Struct(e)
    if err != nil {
        return err
    }
    return nil
}
```

**输出**

```go
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'required' tag
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag
```

首先，我们需要声明 Validate 的实例

```go
var validate *validator.Validate
```

请注意，我们需要将元标签与结构体的字段关联，以让验证器知道您想要验证此字段。在上面的示例中，我们为年龄字段添加了标签。该标签由 playground validate 库解析。请注意，我们为**年龄**字段添加了三种验证

+   **required** – 验证字段是否存在

+   **gte** – 验证字段值是否大于等于特定值

+   **lte** – 验证字段值是否小于等于特定值

```go
type employee struct {
	Age int `validate:"required,gte=10,lte=20"`
}
```

然后调用 Struct 方法来验证结构体

```go
validate.Struct(e)
```

对于

```go
e := employee{}
```

输出如下，因为**年龄**字段为空

```go
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'required' tag
```

对于

```go
e := employee{Age: 5}
```

输出如下，因为**年龄**字段的值是 5，小于 10

```go
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag
```

对于

```go
e := employee{Age: 25}
```

输出如下，因为**年龄**字段的值是 25，超过了 20

```go
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [gp](https://golangbyexample.com/tag/gp/)*   [range](https://golangbyexample.com/tag/range/)*   [validation](https://golangbyexample.com/tag/validation/)*
