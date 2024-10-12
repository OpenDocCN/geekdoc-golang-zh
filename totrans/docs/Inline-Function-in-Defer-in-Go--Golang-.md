<!--yml
category: 未分类
date: 2024-10-13 06:26:47
-->

# Inline Function in Defer in Go (Golang)

> 来源：[https://golangbyexample.com/inline-function-defer-go/](https://golangbyexample.com/inline-function-defer-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

It is also possible to have an inline function with defer.

# **Example**

Let’s see an example of that.

```
package main

import "fmt"

func main() {
    defer func() { fmt.Println("In inline defer") }()
    fmt.Println("Executed")
}
```

**Output**

```
Executed
In inline defer
```

In the above code we have defer with a inline function

```
defer func() { fmt.Println("In inline defer") }()
```

This is allowed in go. Also note that it is mandatory to add **“()”** after the function otherwise compiler will raise error

```
expression in defer must be function call
```

As seen from the output, that the inline function is called after everything in the main is executed and before main returns. That is why

```
Executed in main
```

is printed before

```
In inline Defer
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*