<!--yml
category: 未分类
date: 2024-10-13 06:12:40
-->

# Trim suffix of a string in Go (Golang)

> 来源：[https://golangbyexample.com/trim-suffix-string-golang/](https://golangbyexample.com/trim-suffix-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **TrimSuffix** method that can be used to remove a suffix string from the input string. If the input string doesn’t end with the given suffix, then the input string remains unchanged. Also, note that this function returns the copy of the string.

Below is the signature of the function

```
func TrimSuffix(s, suffix string)
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
    //This will output "removed"
    res := strings.TrimSuffix("removedtest", "test")
    fmt.Println(res)

    //The input string will remain unchanged as it doesn't contain the test as suffix
    res2 := strings.TrimSuffix("removedtes", "test")
    fmt.Println(res2)
}
```

**Output:**

```
removed
removedtes
```*