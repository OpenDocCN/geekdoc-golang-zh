<!--yml
category: 未分类
date: 2024-10-13 06:35:52
-->

# Understanding Errorf function in Go (Golang)

> 来源：[https://golangbyexample.com/errorf-function-golang/](https://golangbyexample.com/errorf-function-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [Formatting a string variable](#Formatting_a_string_variable "Formatting a string variable")
    *   [Formatting an integer](#Formatting_an_integer "Formatting an integer")
    *   [Formatting a struct](#Formatting_a_struct "Formatting a struct")
*   [Wrapping of error](#Wrapping_of_error "Wrapping of error")*  *## **Overview**

**Errorf** function is defined in the **fmt** package and is used to create a custom error with a formatted message as per the format specifier provided.

[https://golang.org/pkg/fmt/#Errorf](https://golang.org/pkg/fmt/#Errorf)

*   Its main use is to create a custom error in golang with a formatted message.

*   It is also used to create a custom error which can wrap another error provided to it. In golang, an error can wrap another error as well.  What does the wrapping of error mean? It means to create a hierarchy of errors in which a  particular instance of error wraps another error and that particular instance itself can be wrapped inside another error. We will see about this in detail later in this tutorial.

Below is the function prototype of **Errorf**

```
func Errorf(format string, a ...interface{}) error
```

As you can see it returns an instance of the error.

**Errorf** is also a variadic function meaning that it can have multiple arguments. There are two important points about its argument list

*   Notice that the first argument is a **format** or **template** string.

*   The next is a variable number of arguments. Each of the arguments in this list could be string, int, struct, or anything. That is why it is an empty interface

**Errrof** formats the string using custom specifiers. The first argument that is the **format** or **template** string contains the actual string that needs to be formatted plus some formating verbs. These formating verbs tell how the trailing arguments will be formatted in the final string.  So basically the format string argument contains certain symbols which are replaced by trailing arguments. 

Eg 

### **Formatting a string variable**

*   **%s** symbol is used

*   Example 

```
name := "J"
fmt.Errorf("Name has less then 3 character. Name: %s\n", name)
```

### **Formatting an integer**

*   **%d** symbol is used

*   Example 

```
age := 0
fmt.Errorf("Age is 0: Age:%d\n", age)
```

### **Formatting a struct**

For example, there are three format specifiers for printing a struct. 

*   **%v** – It will print only values. The field name will not be printed. This is the default way of printing a struct when using Println

*   **%+v – **It will print both field and value. 

*   **%#v – **It will print the struct, also both field name and value

That is why

```
fmt.Errorf("Employee not found. Details: %v\n", e)
fmt.Errorf("Employee not found. Details: %+v\n", e)
fmt.Errorf("Employee not found. Details: %#v\n", e)
```

returns an error with below formatted message respectively

Here is the working program for the same

```
package main

import (
	"fmt"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	sampleErr := "database connection issue"

	err := fmt.Errorf("Err is: %s", sampleErr)
	fmt.Println(err)

	port := 8080

	err = fmt.Errorf("Err is: %s to port %d", sampleErr, port)
	fmt.Println(err)

	e := employee{
		Name: "John",
	}

	err = fmt.Errorf("Employee not found. Details %v", e)
	fmt.Println(err)
	err = fmt.Errorf("Employee not found. Details %+v", e)
	fmt.Println(err)
	err = fmt.Errorf("Employee not found. Details %#v", e)
	fmt.Println(err)
}
```

**Output**

```
Err is: database connection issue
Err is: database connection issue to port 8080
Employee not found. Details {John 0}
Employee not found. Details {Name:John Age:0}
Employee not found. Details main.employee{Name:"John", Age:0}
```

Notice that in below **Errorf**

```
err = fmt.Errorf("Err is: %s to port %d", sampleErr, port)
```

*   **%s** is replaced by **sampleErr**.

*   **%d** is replaced by **port**.

So basically the symbols or verbs in the format string argument are replaced by trailing arguments in order

If the number of format specifiers in the format string does not match the number of next variable arguments then the format specifier will be printed as is. For example, in the below code, we have two format specifier

*   %s

*   %d

While the next variable number of arguments is only one. Hence when we format it then it returns the formatted error with second format specifier as is with MISSING as a warning

```
package main

import "fmt"

func main() {
	name := "John"

	err := fmt.Errorf("Employee not found with name: %s and age %d", name)
	fmt.Println(err)
}
```

**Output**

```
Employee not found with name: John and age %!d(MISSING)
```

## **Wrapping of error**

Below is the syntax for wrapping an error

```
e := fmt.Errorf("... %w ...", ..., err, ...)
```

**%w** directive Is used for wrapping the error.  The **fmt.Errorf **should be called with only one %w directive. Let’s see an example.

```
package main

import (
	"fmt"
)

type errorOne struct{}

func (e errorOne) Error() string {
	return "Error One happened"
}

func main() {

	e1 := errorOne{}

	e2 := fmt.Errorf("E2: %w", e1)

	e3 := fmt.Errorf("E3: %w", e2)

	fmt.Println(e2)

	fmt.Println(e3)

}
```

**Output**

```
E2: Error One happened
E3: E2: Error One happened
```

In the above program, we created a struct **errorOne** that has an **Error** method hence it implements the **error** interface. Then we created an instance of the **errorOne** struct named **e1**. Then we wrapped that instance **e1** into another error **e2** like this

```
e2 := fmt.Errorf("E2: %w", e1)
```

Then we wrapped **e2** into **e3** like below. 

```
e3 := fmt.Errorf("E3: %w", e2)
```

So so we created a hierarchy of errors in which **e3** wraps **e2** and **e2** wraps **e1**.  Thus **e3** also wraps **e1** transitively. When we print **e2 ** it also prints the error from **e1** and gives the output.

```
E2: Error One happened
```

When we print **e3** it prints the error from **e2** as well as **e1** and gives the output.

```
E3: E2: Error One happened
```

You can read about wrapping and unwrapping of error in detail here

[https://golangbyexample.com/wrapping-and-unwrapping-error-golang/](https://golangbyexample.com/wrapping-and-unwrapping-error-golang/)

This is all about the Errorf function. Hope you have liked this article. Please share feedback in the comments. Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [errorf](https://golangbyexample.com/tag/errorf/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*