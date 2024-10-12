<!--yml
category: 未分类
date: 2024-10-13 06:12:26
-->

# Convert string to uppercase in Go (Golang)

> 来源：[https://golangbyexample.com/golang-string-uppercase/](https://golangbyexample.com/golang-string-uppercase/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In Glang string are UTF-8 encoded. **strings** package of go language provides a **ToUpper** method that can be used to convert all Unicode letters to their upper case. This method will return the copy of the string as in GO string are immutable.

Below is the signature of the function

Let’s look at the working code

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.ToUpper("abc")
    fmt.Println(res)

    res = strings.ToUpper("abc12$")
    fmt.Println(res)
}
```

**Output:**

```
ABC
ABC12$
```*