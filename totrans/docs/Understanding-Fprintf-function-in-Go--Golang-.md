<!--yml
category: 未分类
date: 2024-10-13 06:35:59
-->

# Understanding Fprintf function in Go (Golang)

> 来源：[https://golangbyexample.com/fprintf-golang/](https://golangbyexample.com/fprintf-golang/)

![Fprintf Image](img/9dcf336e99223726fb9611b41749a744.png)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [Formatting a string variable](#Formatting_a_string_variable "Formatting a string variable")
    *   [Formatting an integer](#Formatting_an_integer "Formatting an integer")
    *   [Formatting a struct](#Formatting_a_struct "Formatting a struct")
*   [Program](#Program "Program")*  *## **Overview**

**Fprintf** is defined in the **fmt** package and is used to format a string and write that formatted string to **io.Writer** instance passed to it as one of the arguments.

[https://golang.org/pkg/fmt/#Fprint](https://golang.org/pkg/fmt/#Fprint)

Golang provides another similar function **Printf**. The only difference between **Frprintf** and **Printf** is that **Fprintf** writes to the **io.Writer** instance passed to it while **Printf** function writes to the standard output

Below is the function prototype of **Fprintf**

```
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
```

**Fprintf** is also a variadic function meaning that it can have multiple arguments. Here are the details about its arguments

*   The first argument is the **io.Writer** instance to which it writes

*   The second argument is a **format** or **template** string. The string contains the custom specifiers like **%s** for **string**, **%d** for **int,** etc

*   The next is a variable number of arguments. Each of the arguments in this list could be string, int, struct, or anything. That is why it is an empty interface.

**Fprintf** formats the string using custom specifiers. The **format** or **template** string contains the actual string that needs to be formatted plus some formating verbs. These formating verbs tell how the trailing arguments will be formatted in the final string.  So basically the format string argument contains certain symbols which are replaced by trailing arguments.   

Also to note about the **Fprintf** function is that it also doesn’t add a new line. We need to use the new line identifier to add a new line **“\n”**.

Eg

### **Formatting a string variable**

*   **%s** symbol is used

*   Example 

```
name := "John"
fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
```

The output will be as below. It will be written to **os.Stdout** which is standard output. See we pass **os.Stdout** as the first argument.

```
Name is John
```

### **Formatting an integer**

*   **%d** symbol is used

*   Example 

```
age := 21
fmt.Fprintf(os.Stdout, "Age is: %d\n", age)
```

The output will be as below. It will be written to **os.Stdout** which is standard output.

### **Formatting a struct**

For example, there are three format specifiers for printing a struct. 

*   **%v** – It will print only values. The field name will not be printed. This is the default way of printing a struct when using Println
*   **%+v – **It will print both field and value. 
*   **%#v – **It will print the struct, also both field name and value

That is why

```
fmt.Fprintf(os.Stdout, "Employee is %v\n", e)
fmt.Fprintf(os.Stdout, "Employee is %+v\n", e)
fmt.Fprintf(os.Stdout,"Employee is %#v\n", e)
```

writes below respectively to the **os.Stdout** instance

```
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

It is as per the explanation above. Also, note that this function returns the number of characters printed and any error if happens. It does not add a new line. You will have to add **“\n”** explicitly. 

## **Program**

Here is the working program for the same

```
package main

import (
	"fmt"
	"log"
	"os"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"
	age := 21

	fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
	fmt.Fprintf(os.Stdout, "Age is: %d\n", age)

	fmt.Fprintf(os.Stdout, "Name: %s Age: %d\n", name, age)

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Fprintf(os.Stdout, "Employee is %v\n", e)
	fmt.Fprintf(os.Stdout, "Employee is %+v\n", e)
	fmt.Fprintf(os.Stdout, "Employee is %#v\n", e)

	bytesPrinted, err := fmt.Fprintf(os.Stdout, "Name is: %s\n", name)
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

In all the **Fprintf** function above we pass it the instance of **os.Stdout** to it which implements the **io.Writer** interface. Basically with **os.Stdout**, **Fprintf** writes to standard output. This is how **os.Stdout** defined.

```
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

Notice that in below **Fprintf**

```
fmt.Fprintf(os.Stdout, "Name: %s Age: %d\n", name, age)
```

*   **%s** is replaced by name.

*   **%d** is replaced by age.

So basically the symbols or verbs in the format string argument are replaced by trailing arguments in order

**Fprintf** can also be used to write to a file. Since the file instance in golang implements the **io.Writer**, this is not a problem. Below is the program for the same

```
package main
import (
    "fmt"
    "log"
    "os"
)
type employee struct {
    Name string
    Age  int
}
func main() {
    name := "John"
    age := 21
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Fprintf(file, "Name is: %s\n", name)
    fmt.Fprintf(file, "Age is: %d\n", age)
    fmt.Fprintf(file, "Name: %s Age: %d\n", name, age)
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Fprintf(file, "Employee is %v\n", e)
    fmt.Fprintf(file, "Employee is %+v\n", e)
    fmt.Fprintf(file, "Employee is %#v\n", e)
}
```

**Output**

It will create file name **temp.txt** in the current directory with the below contents. In this program we replaced **os.Stdou**t with the file created.

```
Name is: John
Age is: 21
Name: John Age: 21
Employee is {John 21}
Employee is {Name:John Age:21}
Employee is main.employee{Name:"John", Age:21}
```

If the number of format specifiers in the format string does not match the number of next variable arguments then the format specifier will be printed as is. For example, in the below code, we have two format specifier

*   **%d**

*   **%s**

While the next variable number of arguments is only one. Hence when we call it then it will write the second format specifier as is with MISSING as a warning

```
package main

import (
	"fmt"
	"os"
)

type employee struct {
	Name string
	Age  int
}

func main() {
	name := "John"

	fmt.Fprintf(os.Stdout, "Name is: %s %d\n", name)
}
```

**Output**

```
Name is: John %!d(MISSING)
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*