<!--yml
category: 未分类
date: 2024-10-13 06:26:22
-->

# Format a message without printing in Go (Golang)

> 来源：[https://golangbyexample.com/format-message-no-print-golang/](https://golangbyexample.com/format-message-no-print-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**Sprintf** function of **fmt** package can be used to format the string without printing it.  It is similar to **Printf** function with the only difference between the two being

*   **Printf** formats and print the message
*   **Sprintf** only formats the message.

Below is the syntax of the **Sprintf** function

```
func Sprintf(format string, a ...interface{}) string
```

The **Sprintf** function formats the string according to format specifier and returns the resultant string

# **Example**

Let’s see a program for this

```
package main

import "fmt"

func main() {
    formattedMessage := fmt.Sprintf("Name is: %s. Age is: %d", "John", 21)
    fmt.Println(formattedMessage)
}
```

**Output**

```
Name is: John. Age is: 21
```

In the above program **fmt.Sprintf** correctly formats the message while **fmt.Println** is used to print the formatted message

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*