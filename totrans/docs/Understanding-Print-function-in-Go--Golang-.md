<!--yml
category: 未分类
date: 2024-10-13 06:37:50
-->

# Understanding Print function in Go (Golang)

> 来源：[https://golangbyexample.com/print-function-golang/](https://golangbyexample.com/print-function-golang/)

![](img/a1351ff423b96c7f5cfda45be9f96f65.png)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Print** is defined in the **fmt** package and is used to format a string and write to standard output

```
https://golang.org/pkg/fmt/#Print
```

Below is the function prototype of **Print**

```
func Print(a ...interface{}) (n int, err error)
```

**Print** formats the string using the default format specifier but does not add a new line after the string. **Print** takes a variable number of arguments where each argument is an empty interface. It returns the number of characters printed and any error if happens. Since the argument type is an empty interface we can pass any data type to it. We can pass a string, int, float, struct, or any other data type. Each of the arguments to the **Print** function is formatted according to the default format specifier of that argument type. For example, the struct will be formatted according to the below specifier

```
%v
```

This format specifier only prints the Value part in the struct. There is also one more function provided by **fmt** package which appends a new line – **Println**. The **Print** function is exactly the same as the **Println** function other than two differences

*   It does not append a newline at the end. We need to use the new line identifier to add a new line “\n”.

*   Space is only added between the arguments if neither of the operands is a string

Let’s see an example

## **Program**

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

```
bytesPrinted, err := fmt.Print("Name is: ", name, "\n")
if err != nil {
    log.Fatalln("Error occured", err)
}
fmt.Print(bytesPrinted)
```

will output below

```
Name is: John
14
```

The number of **bytesPrinted** is 14 as 14 characters are outputted

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*