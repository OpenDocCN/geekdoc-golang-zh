<!--yml
category: 未分类
date: 2024-10-13 06:37:39
-->

# Understanding Println function in Go (Golang)

> 来源：[https://golangbyexample.com/println-golang/](https://golangbyexample.com/println-golang/)

![println](img/5327d9a5221455aba6ab8a7076afdfd2.png)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

**Println** is defined in the **fmt** package and is used to format a string and write to standard output

[https://golang.org/pkg/fmt/#Println](https://golang.org/pkg/fmt/#Println)

Below is the function prototype of **Println**

```
func Println(a ...interface{}) (n int, err error)
```

**Println** formats the string using the default format specifier adds a new line after the string. **Println** takes a variable number of arguments where each argument is an empty interface. It returns the number of characters printed and any error if happens. Since the argument type is an empty interface we can pass any data type to it. We can pass a string, int, float, struct, or any other data type. Each of the arguments to the **Println** function is formatted according to the default format specifier of that argument type. For example, the struct will be formatted according to the below specifier

```
%v
```

This format specifier only prints the Value part in the struct. Let’s  see an example

## **Program**

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

*   It appends a newline at the end. That is why each of the output is on a different line

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

The number of **bytesPrinted** is 14 as 14 characters are outputted

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*