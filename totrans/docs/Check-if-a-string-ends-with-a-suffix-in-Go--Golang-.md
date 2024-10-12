<!--yml
category: 未分类
date: 2024-10-13 06:12:17
-->

# Check if a string ends with a suffix in Go (Golang)

> 来源：[https://golangbyexample.com/go-strings-ends-suffix/](https://golangbyexample.com/go-strings-ends-suffix/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **HasSuffix** method that can be used to check if a string ends with a certain suffix

Below is the signature of the function

```
func HasSuffix(s, prefix string) bool
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
    //Input string contains the suffix
    res := strings.HasSuffix("abcdef", "ef")
    fmt.Println(res)

    //Input string doesn't contain the suffix
    res = strings.HasPrefix("abcdef", "fg")
    fmt.Println(res)
}
```

**Output:**

```
true
false
```*