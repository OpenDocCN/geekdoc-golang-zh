<!--yml
category: 未分类
date: 2024-10-13 06:34:01
-->

# Validate the range of the integer in a struct in Go (Golang)

> 来源：[https://golangbyexample.com/range-int-struct-validate-golang/](https://golangbyexample.com/range-int-struct-validate-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

The below library can be used to validate the range of an integer in a struct in Golang

*   **gopkg.in/go-playground/validator.v9** – [https://pkg.go.dev/github.com/go-playground/validator](https://pkg.go.dev/github.com/go-playground/validator)

For this tutorial, we will use the below employee struct

```
type employee struct {
    Age int
}
```

# **Example**

Let’s see an example for the same. Below is the code

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

**Output**

```
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'required' tag
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag
```

First, we need to declare the instance of Validate

```
var validate *validator.Validate
```

Notice here that we need to associate meta tags with fields of the struct to let the validator know that you want to validate this field. In the above example, we added the tag with the Age field. This tag is interpreted by the playground validate library. Notice we added three validations for the **Age** field

*   **required** – validates that the field is present

*   **gte** – validate that the field value is greater than equal to a particular value

*   **lte** – validate that the field value is less than equal to a particular value

```
type employee struct {
	Age int `validate:"required,gte=10,lte=20"`
}
```

Then call the Struct method to validate the struct

```
validate.Struct(e)
```

For

```
e := employee{}
```

it gives the output as below is **Age** field is empty

```
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'required' tag
```

For

```
e := employee{Age: 5}
```

it gives the output as below as **Age** field value is 5 which is less than 10

```
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'gte' tag
```

For

```
e := employee{Age: 25}
```

it gives the output as below as **Age** field value is 25 which is greater than 20

```
Error: Key: 'employee.Age' Error:Field validation for 'Age' failed on the 'lte' tag
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [gp](https://golangbyexample.com/tag/gp/)*   [range](https://golangbyexample.com/tag/range/)*   [validation](https://golangbyexample.com/tag/validation/)*