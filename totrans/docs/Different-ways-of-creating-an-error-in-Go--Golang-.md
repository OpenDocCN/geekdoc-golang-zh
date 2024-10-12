<!--yml
category: 未分类
date: 2024-10-13 06:02:42
-->

# Different ways of creating an error in Go (Golang)

> 来源：[https://golangbyexample.com/different-ways-of-creating-an-error-in-go-golang/](https://golangbyexample.com/different-ways-of-creating-an-error-in-go-golang/)

Before learning about different ways of creating am an error in Golang lets first understand about the error. Error is an interface type with below signature

```
type error interface {  
    Error() string
}
```

Any by definition of **interface** any type which implements the **Error()** method becomes of type **error**.

Let’s see now different methods of creating an **error**

# 1\. **Using errors.New(“some_error_message”)**

```
package main

import (
    "errors"
    "fmt"
)

func main() {
    sampleErr := errors.New("error occured")
    fmt.Println(sampleErr)
}
```

**Output**:

```
error occured
```

# 2\. **Using fmt.Errorf(“error is %s”, “some_error_message”). This way creates error with formatting of error messages**

```
package main

import (
    "fmt"
)

func main() {
    sampleErr := fmt.Errorf("Err is: %s", "database connection issue")
    fmt.Println(sampleErr)
}
```

**Output:**

```
Err is: database connection issue
```

# 3\. **Creating Custom error**

The below example illustrates the use of custom error. In below example

*   **inputError** is a struct that has the **Error()** method hence it is of type **error**
*   You can also add additional information to the custom error by extending its fields or by adding new methods. **inputError** has an additional field named **missingFields** and a function **getMissingFields** function.
*   We can use type assertion to convert from **error** to **inputError**

**Example:**

```
package main

import "fmt"

type inputError struct {
    message      string
    missingField string
}

func (i *inputError) Error() string {
    return i.message
}

func (i *inputError) getMissingField() string {
    return i.missingField
}

func main() {
    err := validate("", "")
    if err != nil {
        if err, ok := err.(*inputError); ok {
            fmt.Println(err)
            fmt.Printf("Missing Field is %s\n", err.getMissingField())
        }
    }
}

func validate(name, gender string) error {
    if name == "" {
        return &inputError{message: "Name is mandatory", missingField: "name"}
    }
    if gender == "" {
        return &inputError{message: "Gender is mandatory", missingField: "gender"}
    }
    return nil
}
```

**Output:**

```
Name is mandatory
Missing Field is name
```