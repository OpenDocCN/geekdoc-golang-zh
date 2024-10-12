<!--yml
category: 未分类
date: 2024-10-13 06:12:13
-->

# Check if string begins with a prefix in Go (Golang)

> 来源：[https://golangbyexample.com/string-begins-prefix-go/](https://golangbyexample.com/string-begins-prefix-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **HasPrefix** method that can be used to check if a string begins with a certain prefix

Below is the signature of the function

```
func HasPrefix(s, prefix string) bool
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
    //Input string contains the prefix
    res := strings.HasPrefix("abcdef", "ab")
    fmt.Println(res)

    //Input string doesn't contain the prefix
    res = strings.HasPrefix("abcdef", "ac")
    fmt.Println(res)
}
```

**Output:**

```
true
false
```*