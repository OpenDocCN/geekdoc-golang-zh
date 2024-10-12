<!--yml
category: 未分类
date: 2024-10-13 06:36:31
-->

# Understanding Fprintln function in Go (Golang)

> 来源：[https://golangbyexample.com/fprintln-golang/](https://golangbyexample.com/fprintln-golang/)

![fprintln image](img/3cd9912b7a0733f8200683fac3715644.png)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Fprintln** is defined in the **fmt** package and is used to format a string using the default format specifier and write it to **io.Writer** instance passed to it. It also adds a new line.

[https://golang.org/pkg/fmt/#Fprintln](https://golang.org/pkg/fmt/#Fprintln)

Below is the function prototype of **Fprintln**

```
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

**Fprintln** is also a variadic function meaning that it can have multiple arguments. Here are the details about its arguments

*   The first argument is the **io.Writer** instance to which it writes

*   The next is a variable number of arguments. Each of the arguments in this list could be string, int, struct, or anything. That is why it is an empty interface. Each of these arguments is formatted using the default specifier

**Fprintln** formats the string using the default format specifier adds a new line after the string. **Fprintln** takes a variable number of arguments where each argument is an empty interface. It returns the number of characters printed and any error if happens. Since the argument type is an empty interface we can pass any data type to it. We can pass a string, int, float, struct, or any other data type. Each of the arguments to the **Fprintln** function is formatted according to the default format specifier of that argument type. For example, the struct will be formatted according to the below specifier

There is also another function **Fprint** provided by **fmt** package which is the same as **Fprintln.** The only difference being

*   **Fprintln** appends a new line while **Fprint** does not append a new line.

```
%v
```

This format specifier only prints the Value part in the struct. Let’s see an example.

## **Program**

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
	fmt.Fprintln(os.Stdout, "Name is: ", name)
	fmt.Fprintln(os.Stdout, "Age is: ", age)
	e := employee{
		Name: name,
		Age:  age,
	}
	fmt.Fprintln(os.Stdout, e)
	fmt.Fprintln(os.Stdout, "a", 12, "b", 12.0)

	bytesPrinted, err := fmt.Fprintln(os.Stdout, "Name is: ", name)
	if err != nil {
		log.Fatalln("Error occured", err)
	}
	fmt.Println(bytesPrinted)
}
```

**Output**

```
Name is:  John
Age is:  21
{John 21}
a 12 b 12
Name is:  John
15
```

Some important points to note about the **FPrintln** function

*   In all the **Fprintln** functions above we pass it the instance of **os.Stdout** which implements the **io.Writer** interface. Basically with **os.Stdout**, **Fprintln** writes to standard output. This is how **os. Stdout** is defined.

```
Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
```

*   It appends a new line at the end. That is why each of the output is on a different line

*   Each of the arguments will be separated by space in the output. That is why

```
fmt.Fprintln(os.Stdout,"Name is: ", name)
```

prints

```
Name is: John
```

Space is introduced automatically between the two arguments.

*   It returns the number of characters printed or any error if happens

```
bytesPrinted, err := fmt.Fprintln(os.Stdout, "Name is: ", name)
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

The number of **bytesPrinted** is 14 as 14 characters are outputted

**Fprintln** can also be used to write to a file. Since the file instance in golang implements the **io.Writer**, this is not a problem. Below is the program for the same

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
	fmt.Fprintln(file, "Name is: ", name)
	fmt.Fprintln(file, "Age is: ", age)
	e := employee{
		Name: name,
		Age:  age,
	}
	fmt.Fprintln(file, e)
	fmt.Fprintln(file, "a", 12, "b", 12.0)
}
```

**Output**

It will create file name temp.txt in the current directory with the below contents. In this program we replaced **os.Stdou**t with the file created.

```
Name is:  John
Age is:  21
{John 21}
a 12 b 12
```

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [fprintln](https://golangbyexample.com/tag/fprintln/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*