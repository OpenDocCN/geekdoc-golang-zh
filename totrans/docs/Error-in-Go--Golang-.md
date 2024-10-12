<!--yml
category: 未分类
date: 2024-10-13 06:25:13
-->

# Error in Go (Golang)

> 来源：[https://golangbyexample.com/error-in-golang/](https://golangbyexample.com/error-in-golang/)

This is the  chapter 26 of the golang comprehensive tutorial series. Refer to this link for other chapters of the series – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/)

**Next Tutorial** – [Error -Part 2](https://golangbyexample.com/error-in-golang-advanced/)
**Previous Tutorial** – [Select Statement](https://golangbyexample.com/select-statement-golang/)

Now let’s check out the current tutorial. Below is the table of contents for current tutorial.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Using type which implements error interface](#Using_type_which_implements_error_interface "Using type which implements error interface")
*   [Advantages of using error as a type](#Advantages_of_using_error_as_a_type "Advantages of using error as a type")
*   [Different ways of creating an error](#Different_ways_of_creating_an_error "Different ways of creating an error")
    *   [Using errors.New(“some_error_message”)](#Using_errorsNewsome_error_message "Using errors.New(“some_error_message”)")
    *   [Using fmt.Errorf(“error is %s”, “some_error_message”).](#Using_fmtErrorferror_is_s_some_error_message "Using fmt.Errorf(“error is %s”, “some_error_message”). ")
    *   [Creating Custom error](#Creating_Custom_error "Creating Custom error")
*   [Ignoring errors](#Ignoring_errors "Ignoring errors")
*   [Conclusion](#Conclusion "Conclusion")*  *# **Overview**

In this tutorial, we will study how to do error handling in golang. As compared to other conventional languages go doesn’t have exceptions and try-catch for handling the error.
The error handling in golang can be done in two ways

*   Using type which implements **error** interface –   it is a conventional way to represent an error and  also idiomatic

*   Using panic and recover

We will only be discussing the first part in this article.

# **Using type which implements error interface**

Go’s way of dealing with an error is to explicitly return the error as a separate value.

The crux to understanding error in golang is to understand the **error** interface. This is how the **error** interface looks as defined in the **builtin** package

[https://golang.org/pkg/builtin/#error](https://golang.org/pkg/builtin/#error)

```
type error interface {
    Error() string
}
```

The **error** interface is the conventional way to represent an error condition in golang. If this is nil then it means no error. So in go error is treated as a value. This value implements the **error** interface and which can be passed around to a function, returned from a function, and which can be stored in variables.

So any type which defines the **Error** method is set to be implementing the **error** interface.  As we mentioned earlier that go doesn’t have exceptions and try-catch so an idiomatic way of handling error condition is to return the error as the last return value. The value can then be checked for nil. If it is nil then the  called function did not return the  error otherwise the called function returned the error. So let’s see a program which demonstrates what we just talked.

```
package main

import (
	"fmt"
	"os"
)

func main() {
	file, err := os.Open("non-existing.txt")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(file.Name() + "opened succesfully")
	}
}
```

**Output**

```
open non-existing.txt: no such file or directory
```

In the above program, we are calling the **os.Open** function to open a non-existing file. Since the file does not exist it gives the above error message in the output. But from where is this error message coming from. Let’s understand that.

Below is the signature of **os.Open** function

```
func Open(name string) (*File, error)
```

It takes one argument as an input which is the file path to be opened. It has two return values.

*   The pointer to the file struct if the file is present

*   If there is an error, then it returns the type ***PathError** which implements the **error** interface. 

 Check out the **PathError** struct type here- [https://golang.org/src/os/error.go](https://golang.org/src/os/error.go)

```
type PathError struct {
   Op   string
   Path string
   Err  error
}
```

The pointer to **PathError** struct defines the **Error()** method

```
func (e *PathError) Error() string { return e.Op + " " + e.Path + ": " + e.Err.Error() }
```

Since it defines the **Error** method hence ***PathError** implements the **error** interface.   That is why **error** can be returned as the second return value from the **os.Open** function.  Now notice how in the **main** function we check for the presence of error by only comparing it with nil.   

The reason we compare it with nil because the default zero value of interface is nil and since **error** is also an interface its default zero value is also nil.

 Also **fmt.Println** function internally checks if a type passed as an argument to it implements the **error** interface. If yes then it calls the **Error** method on that type. That is what this line

```
fmt.Println(err)
```

gives output

```
open non-existing.txt: no such file or directory
```

**Using any type as error**

Even though we have an error interface in go and we expect every error in go to follow this interface as seen above, still it is not a mandatory requirement. Any type can be treated as an error in go, but generally, it is not recommended that as well as it is not an idiomatic way to code in go. We are mentioning here for the sake of completion and as well as the discussion.

# **Advantages of using error as a type**

*   It allows more control over handling the error. The error can be checked at each step

*   Prevents the ugly code of try-catch and exception handling

# **Different ways of creating an error**

Let’s see now different methods of creating an **error**

## **Using errors.New(“some_error_message”)**

```
package main

import (
    "errors"
    "fmt"
)

func main() {
    sampleErr := errors.New("error occured")
    fmt.Println(sampleErr)
```

**Output:**

```
error occured
```

## **Using fmt.Errorf(“error is %s”, “some_error_message”).**

This way creates error with formatting of error messages

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

## **Creating Custom error**

The below example illustrates the use of custom error. In below example

*   inputError is a struct that has the Error() method hence it is of type error interface

*   You can also add additional information to the custom error by extending its fields or by adding new methods. inputError has an additional field named **missingFields** and a function **getMissingFields** function.

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

# **Ignoring errors**

Underscore (‘_’) operator can be used to ignore the error returned from a function call.  Before we see a program it’s important to note that error should never be ignored. It is not a recommended way. Let’s see a program

```
package main
import (
    "fmt"
    "os"
)
func main() {
    file, _ := os.Open("non-existing.txt")
    fmt.Println(file)
}
```

**Output**

```
{nil}
```

In the above program, we used the underscore operator to ignore the error while opening a non-existing file. That is why the file instance returned by the function is nil. Therefore it is better to check for an error before using any other argument returned by the function as that can be nil and would result in unwanted issues and also sometimes it might also result in a panic.

# **Conclusion**

This is all about the error in golang. We will discuss advanced topics related to the error in next chapter.

Hope you have liked this article. Please share feedback/improvements/mistakes in comments.

**Next Tutorial** – [Error -Part 2](https://golangbyexample.com/error-in-golang-advanced/)
**Previous Tutorial** – [Select Statement](https://golangbyexample.com/select-statement-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*