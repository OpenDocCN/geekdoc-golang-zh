<!--yml
category: 未分类
date: 2024-10-13 06:14:08
-->

# Case insensitive string comparison in Go (Golang)

> 来源：[https://golangbyexample.com/golang-case-insensitive-string-comparison/](https://golangbyexample.com/golang-case-insensitive-string-comparison/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In Golang string are UTF-8 encoded. **strings** package of GO provides an **EqualFold** method that can be used to do case insensitive comparison of two strings in Go.

Below is the signature of the function. The methods return boolean indicating whether the two strings supplied are case insensitive equal or not.

```
func EqualFold(s, t string) bool
```

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.EqualFold("abc", "ABC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "aBC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "AbC")
    fmt.Println(res)

    res = strings.EqualFold("abc", "AbCd")
    fmt.Println(res)
}
```

**Output:**

```
true
true
true
false
```

*   [golang](https://golangbyexample.com/tag/golang/)*