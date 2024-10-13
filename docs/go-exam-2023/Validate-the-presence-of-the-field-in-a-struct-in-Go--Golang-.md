<!--yml

类别：未分类

日期：2024-10-13 06:33:56

-->

# 在Go（Golang）中验证结构体中字段的存在。

> 来源：[https://golangbyexample.com/struct-field-validate-presence-golang/](https://golangbyexample.com/struct-field-validate-presence-golang/)

目录

**   [概述](#Overview "概述")

+   [第一个库 (go-playground/validator)](#First_Library_go-playgroundvalidator "第一个库 (go-playground/validator)")

+   [第二个库 (asaskevich/govalidator)](#Second_Library_asaskevichgovalidator "第二个库 (asaskevich/govalidator)")*  *# **概述**

在本教程中，我们将探讨两个可以用于验证Golang中结构体字段的库。这两个库是

+   gopkg.in/go-playground/validator.v9 – [https://pkg.go.dev/github.com/go-playground/validator](https://pkg.go.dev/github.com/go-playground/validator)

+   [github.com](http://github.com)/asaskevich/govalidator – [https://github.com/asaskevich/govalidator](https://github.com/asaskevich/govalidator)

对于本教程，我们将使用下面的员工结构体。

```go
type employee struct {
    Name string
}
```

# **第一个库 (go-playground/validator)**

让我们首先看看playground验证库。下面是相应的代码。

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
    Name string `validate:"required"`
}

func main() {
    e := employee{}
    err := validateStruct(e)
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
Error: Key: 'employee.Name' Error:Field validation for 'Name' failed on the 'required' tag
```

首先，我们需要声明Validate的实例。

```go
var validate *validator.Validate
```

注意，我们需要将元标签与结构体字段关联，以让验证器知道你想验证这个字段。在上述示例中，我们为Name字段添加了标签。该标签由playground验证库解释。

```go
type employee struct {
    Name string `validate:"required"`
}
```

然后调用Struct方法来验证结构体。

```go
validate.Struct(e)
```

当我们将**Name**字段传递为nil时，它会引发正确的错误。

# **第二个库 (asaskevich/govalidator)**

**go.mod**

```go
module sample.com/validator
go 1.14

require github.com/asaskevich/govalidator v0.0.0-20200907205600-7a23bdc65eef
```

**main.go**

```go
package main

import (
	"fmt"

	"github.com/asaskevich/govalidator"
)

type employee struct {
	Name string `valid:"required"`
}

func main() {
	e := employee{}
	err := validateStruct(e)
	if err != nil {
		fmt.Printf("Error: %s\n", err)
	}
}

func validateStruct(e employee) error {
	_, err := govalidator.ValidateStruct(e)
	if err != nil {
		return err
	}
	return nil

}
```

**输出**

```go
Error: Name: non zero value required
```

与上述示例类似，我们将标签与Name字段关联，govalidator可以解释这些标签。

```go
Name string `valid:"required"`
```

然后我们调用**ValidateStruct**函数，当我们将**Name**字段传递为nil时，它会引发正确的错误。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [验证](https://golangbyexample.com/tag/validation/)*
