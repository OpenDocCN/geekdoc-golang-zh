<!--yml
category: 未分类
date: 2024-10-13 06:35:09
-->

# Write to write or print backslash in a string in Go (Golang)

> 来源：[https://golangbyexample.com/backslash-print-golang/](https://golangbyexample.com/backslash-print-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program for double quotes](#Program_for_double_quotes "Program for double quotes")
*   [Program for back quotes](#Program_for_back_quotes "Program for back quotes")*  *## **Overview**

Backlash is an escaping character. To print a backslash we need to first escape is with another backslash character when using double-quotes. However, a backslash can also be printed using backquotes. It is also used to define a string. A string encoded in backquotes is a raw literal string and doesn’t honor any kind of escaping.

## **Program for double quotes**

```
package main
import "fmt"
func main() {
    fmt.Println("\\test")
}
```

**Output**

\test

A string defined within double quotes will honour escaping characters. That is why we need to escape backlash. Let’s see a program for back quotes

## **Program for back quotes**

```
package main
import "fmt"
func main() {
    fmt.Println(`\test`)
}
```

**Output**

\test

In this case, we don’t need any escaping as with back quotes a string is a raw literal string

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*