<!--yml
category: 未分类
date: 2024-10-13 06:35:23
-->

# Println vs Print vs Printf in Go (Golang)

> 来源：[https://golangbyexample.com/println-printf-print-golang/](https://golangbyexample.com/println-printf-print-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [About Println function](#About_Println_function "About Println function")
*   [About Print function](#About_Print_function "About Print function")
*   [About Printf function](#About_Printf_function "About Printf function")
    *   [Printing a string variable](#Printing_a_string_variable "Printing a string variable")
    *   [Printing an integer](#Printing_an_integer "Printing an integer")
    *   [Printing a struct](#Printing_a_struct "Printing a struct")*  *## **Overview**

**Println**, **Print,** and **Printf** are defined in the **fmt** package and are used to format a string and write to standard output

[https://golang.org/pkg/fmt/](https://golang.org/pkg/fmt/)

The basic difference between them is

*   **Println** formats the string using the default format specifier adds a new line after the string

*   **Print** formats the string using the default format specifier but does not add a new line after the string

*   **Printf** formats the string using a custom specifier. It also doesn’t add a new line

Let’s see each of them in detail with examples

## **About Println function**

Below is the function prototype of **Println**

```
func Println(a ...interface{}) (n int, err error)
```

**Println** takes a variable number of arguments where each argument is an empty interface. It returns the number of characters printed and any error if happens. Since the argument type is an empty interface we can pass any data type to it. We can pass a string, int, float, struct, or any other data type. Each of the arguments to the **Println** function is formatted according to the default format specifier of that argument type. For example, the struct will be formatted according to the below specifier

```
%v
```

This format specifier only prints the Value part in the struct. Let’s see an example

```
package main
import "fmt"
type employee struct {
    Name string
    Age  int
}
func main() {
    name := "John"
    age := 21
    fmt.Println("Name is: ", name)
    fmt.Println("Age is: ", age)
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Println(e)
    fmt.Println("a", 12, "b", 12.0)

    bytesPrinted, err := fmt.Println("Name is: ", name)
    if err != nil {
	log.Fatalln("Error occured", err)
    }
    fmt.Println(bytesPrinted)
}
```

**Output**

```
Name is: John
Age is: 21
{John 21}
a 12 b 12
Name is: John
14
```

Some important points to note about the **Println** function

*   It appends new line at the end. That is why each of the output is on a different line

*   Each of the arguments will be separated by space in the output. That is why

```
fmt.Println("Name is: ", name)
```

prints

```
Name is: John
```

Space is introduced automatically between the two arguments.

*   It returns the number of characters printed or any error if happens

```
bytesPrinted, err := fmt.Println("Name is: ", name)
if err != nil {
    log.Fatalln("Error occured", err)
}
fmt.Println(bytesPrinted)
```

will output below

```
Name is: John
14
```

Number of **bytesPrinted** is 14 as 14 characters are outputted

## **About Print function**

Function prototype of **Print**

```
func Print(a ...interface{}) (n int, err error)
```

The **Print** function is exactly the same as the **Println** function other than two differences

*   It does not append a new line at the end. We need to use the new line identifier to add a new line “\n”.

*   Space is only added between the arguments if neither of the operands is a string

Let’s see an example for the same

```
package main

import "fmt"

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21
	fmt.Print("Name is:", name, "\n")
	fmt.Print("Age is:", age, "\n")

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Print(e, "\n")

	fmt.Print("a", 12, "b", 12.0, "\n")

	fmt.Print(12, 12.0, "\n")

        bytesPrinted, err := fmt.Print("Name is: ", name, "\n")
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Print(bytesPrinted)
}
```

**Output**

```
Name is:John
Age is:21
{John 21}
a12b12
12 12
Name is: John
14
```

Some important points to note about the **Print** function

*   It does not append a new line at the end. That is why need to use **“\n”** for adding a new line.

*   It only adds space between two arguments if each of them is a non-string. That is why

```
fmt.Print(12, 12.0, "\n")
```

prints

```
12 12
```

while

```
fmt.Print("a", 12, "b", 12.0, "\n")
```

prints

```
a12b12
```

*   It also returns the number of characters printed and any error if happens

## **About Printf function**

Function prototype of **Printf**

```
func Printf(format string, a ...interface{}) (n int, err error)
```

**Printf** is also a variadic function meaning that it can have multiple arguments. There are two important points about its argument list

*   Notice that the first argument is a **format** or **template** string.

*   The next is a variable number of arguments. Each of the argument in this list could be string, int, struct, or anything. Due to same reason as above that is why it is an empty interface

The **format** or **template** string contains the actual string that needs to be formatted plus some formating verbs. These formating verbs tell how the trailing arguments will be formatted in the final string.  So basically the format string argument contains certain symbols which are replaced by trailing arguments.  

Eg

### **Printing a string variable**

*   **%s** symbol is used

*   Example 

```
name := "John"
fmt.Printf("Name is: %s\n", name)
```

### **Printing an integer**

*   **%d** symbol is used

*   Example 

```
age := 21
fmt.Printf("Age is: %d\n", age)
```

### **Printing a struct**

For example, there are three format specifiers for printing a struct. 

*   **%v** – It will print only values. The field name will not be printed. This is the default way of printing a struct when using Println

*   **%+v – **It will print both field and value. 

*   **%#v – **It will print the struct, also both field name and value

That is why

```
fmt.Printf("Employee is %v\n", e)
fmt.Printf("Employee is %+v\n", e)
fmt.Printf("Employee is %#v\n", e)
```

prints below respectively

```
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

It is as per the explanation above.

Also, note that this function returns the number of characters printed and any error if happens. Unlike **Println** it does add a new line. You will have to add **“\n”** explicitly. 

Here is the working program for the same

```
package main

import (
	"fmt"
	"log"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21

	fmt.Printf("Name is: %s\n", name)
	fmt.Printf("Age is: %d\n", age)

	fmt.Printf("Name: %s Age: %d\n", name, age)

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Printf("Employee is %v\n", e)
	fmt.Printf("Employee is %+v\n", e)
	fmt.Printf("Employee is %#v\n", e)

	bytesPrinted, err := fmt.Printf("Name is: %s\n", name)
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
}
```

**Output**

```
Name is: John
Age is: 21
Name: John Age: 21
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
Name is: John
14
```

Notice that in below **Printf**

```
fmt.Printf("Name: %s Age: %d\n", name, age)
```

*   **%s** is replaced by name.

*   **%d** is replaced by age.

So basically the symbols or verbs in the format string argument are replaced by trailing arguments in order**If the number of format specifiers in the format string does not match the number of next variable arguments then the format specifier will be printed as is. For example, in the below code, we have two format specifier**

 ***   **%d**

*   **%s**

While the next variable number of arguments is only one. Hence when we print it then it will print the second format specifier as is with MISSING as warning

```
package main
import "fmt"
type employee struct {
    Name string
    Age  int
}
func main() {
    name := "John"
    fmt.Printf("Name is: %s %d\n", name)
}
```

**Output**

```
Name is: John %!d(MISSING)
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)***