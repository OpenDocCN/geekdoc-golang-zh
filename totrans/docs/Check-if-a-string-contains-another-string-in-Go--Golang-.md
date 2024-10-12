<!--yml
category: 未分类
date: 2024-10-13 06:11:55
-->

# Check if a string contains another string in Go (Golang)

> 来源：[https://golangbyexample.com/check-if-substring-golang/](https://golangbyexample.com/check-if-substring-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In Golang string are UTF-8 encoded. **strings** package of GO provides a **Contains** method that can be used to check if a particular string is a substring of another string.

Below is the signature of the function

```
func Contains(s, substr string) bool
```

As you can notice the return value of the Compare function is an bool . This value will be

*   true is **substr** is present in **s**

*   false is **substr** is not present in **s**

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    present := strings.Contains("abc", "ab")
    fmt.Println(present)

    present = strings.Contains("abc", "xyz")
    fmt.Println(present)
}
```

**Output:**

```
true
false
```*