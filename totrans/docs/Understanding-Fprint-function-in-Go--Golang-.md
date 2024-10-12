<!--yml
category: 未分类
date: 2024-10-13 06:36:05
-->

# Understanding Fprint function in Go (Golang)

> 来源：[https://golangbyexample.com/fprint-golang/](https://golangbyexample.com/fprint-golang/)

![](img/73f3f2483c5496743de2017b97b4b213.png)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Fprint** is defined in the **fmt** package and is used to format a string using the default format specifier and write it to **io.Writer** instance passed to it.

[https://golang.org/pkg/fmt/#Fprint](https://golang.org/pkg/fmt/#Fprint)

Below is the function prototype of **Fprint**

```
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
```

**Fprint** is also a variadic function meaning that it can have multiple arguments. Here are the details about its arguments

*   The first argument is the **io.Writer** instance to which it writes

*   The next is a variable number of arguments. Each of the arguments in this list could be string, int, struct, or anything. That is why it is an empty interface. Each of these arguments is formatted using the default specifier

**Fprint** formats the string using the default format specifier but does not add a new line after the string. **Fprint** takes a variable number of arguments after the first argument where each argument is an empty interface. Since the argument type is an empty interface we can pass any data type to it. We can pass a string, int, float, struct, or any other data type. Each of the arguments to the **Fprint** function is formatted according to the default format specifier of that argument type. For example, the struct will be formatted according to the below specifier

```
%v
```

This format specifier only prints the Value part in the struct. There is also one more function provided by **fmt** package which appends a new line – **Fprintln**.

## **Program**

Let’s see an example for the same

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

	fmt.Fprint(os.Stdout, "Name is:", name, "\n")
	fmt.Fprint(os.Stdout, "Age is:", age, "\n")

	e := employee{
		Name: name,
		Age:  age,
	}

	fmt.Fprint(os.Stdout, e, "\n")

	fmt.Fprint(os.Stdout, "a", 12, "b", 12.0, "\n")

	fmt.Fprint(os.Stdout, 12, 12.0, "\n")

	bytesPrinted, err := fmt.Fprint(os.Stdout, "Name is: ", name, "\n")
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
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

Some important points to note about the **Fprint** function

*   In all the **Fprint** function above we pass it the instance of **os.Stdout** to it which implements the **io.Writer** interface. Basically with **os.Stdout**, **Fprint** writes to standard output. This is how **os.Stdout** is defined.

```
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

*   It does not append a new line at the end. That is why need to use **“\n”** for adding a new line.

*   It only adds space between two arguments if each of them is a non-string. That is why

```
fmt.Fprint(os.Stdout, 12, 12.0, "\n")
```

prints

```
12 12
```

while

```
fmt.Fprint(os.Stdout, "a", 12, "b", 12.0, "\n")
```

prints

```
a12b12
```

*   It also returns the number of characters printed and any error if happens

```
bytesPrinted, err := fmt.Fprint(os.Stdout, "Name is: ", name, "\n")
if err != nil {
    log.Fatalln("Error occured", err)
}
fmt.Fprint(bytesPrinted)
```

will output below

```
Name is: John
14
```

The number of **bytesPrinted** is 14 as 14 characters are outputted

**Fprint** can also be used to write to a file. Since the file instance in golang implements the **io.Writer**, this is not a problem. Below is the program for the same

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
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    name := "John"
    age := 21
    fmt.Fprint(file, "Name is:", name, "\n")
    fmt.Fprint(file, "Age is:", age, "\n")
    e := employee{
        Name: name,
        Age:  age,
    }
    fmt.Fprint(file, e, "\n")
    fmt.Fprint(file, "a", 12, "b", 12.0, "\n")
    fmt.Fprint(file, 12, 12.0, "\n")
}
```

**Output**

It will create file name temp.txt in the current directory with the below contents. In this program we replaced **os.Stdou**t with the file created.

```
Name is:John
Age is:21
{John 21}
a12b12
12 12
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*