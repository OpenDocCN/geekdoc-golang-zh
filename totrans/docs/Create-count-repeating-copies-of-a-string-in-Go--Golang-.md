<!--yml
category: 未分类
date: 2024-10-13 06:14:01
-->

# Create count/repeating copies of a string in Go (Golang)

> 来源：[https://golangbyexample.com/create-count-repeating-copies-string-golang/](https://golangbyexample.com/create-count-repeating-copies-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**strings** package of GO provides a **Repeat** method that can be used to create repeating copies of a given string. It takes input as the number of counts.

Below is the signature of the function. It returns a copy of the string

```
func Repeat(s string, count int) string
```

Let’s look at the working code

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Repeat("abc", 4)
    fmt.Println(res)
}
```

**Output:**

```
abcabcabcabc
```

*   [go](https://golangbyexample.com/tag/go/)*