<!--yml
category: 未分类
date: 2024-10-13 06:33:56
-->

# Validate the presence of the field in a struct in Go (Golang)

> 来源：[https://golangbyexample.com/struct-field-validate-presence-golang/](https://golangbyexample.com/struct-field-validate-presence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [First Library (go-playground/validator)](#First_Library_go-playgroundvalidator "First Library (go-playground/validator)")
*   [Second Library (asaskevich/govalidator)](#Second_Library_asaskevichgovalidator "Second Library (asaskevich/govalidator)")*  *# **Overview**

In this tutorial, we will explore two libraries that can be used to validate the field of a struct in Golang. The two libraries are

*   gopkg.in/go-playground/validator.v9 – [https://pkg.go.dev/github.com/go-playground/validator](https://pkg.go.dev/github.com/go-playground/validator)

*   [github.com](http://github.com)/asaskevich/govalidator – [https://github.com/asaskevich/govalidator](https://github.com/asaskevich/govalidator)

For this tutorial, we will use the below employee struct

```
type employee struct {
    Name string
}
```

# **First Library (go-playground/validator)**

Let’s first see the playground validator library. Below is the code for the same.

**go.mod**

```
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

```
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

**Output**

```
Error: Key: 'employee.Name' Error:Field validation for 'Name' failed on the 'required' tag
```

First, we need to declare the instance of Validate

```
var validate *validator.Validate
```

Notice here that we need to associate meta tags with fields of the struct to let the validator know that you want to validate this field. In the above example, we added the tag with the Name field. This tag is interpreted by the playground validate library.

```
type employee struct {
    Name string `validate:"required"`
}
```

Then call the Struct method to validate the struct

```
validate.Struct(e)
```

It raises the correct error as we have passed the **Name** field as nil

# **Second Library (asaskevich/govalidator)**

**go.mod**

```
module sample.com/validator
go 1.14

require github.com/asaskevich/govalidator v0.0.0-20200907205600-7a23bdc65eef
```

**main.go**

```
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

**Output**

```
Error: Name: non zero value required
```

Similar to the above example we associated tags with the Name field which govalidator can interpret

```
Name string `valid:"required"`
```

Then we call the **ValidateStruct** function and it raises the correct error as we have passed the **Name** field as nil

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [validation](https://golangbyexample.com/tag/validation/)*