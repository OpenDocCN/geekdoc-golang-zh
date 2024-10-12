<!--yml
category: 未分类
date: 2024-10-13 06:12:44
-->

# Trim leading and trailing whitespaces from a string in Go (Golang)

> 来源：[https://golangbyexample.com/trim-whitespaces-golang/](https://golangbyexample.com/trim-whitespaces-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **TrimSpace** method that can be used to remove the leading and trailing whitespaces. Also, note that this function returns the copy of the string.

Below is the signature of the function

```
func TrimSpace(s string) string
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
    //This will output removed
    res := strings.TrimSpace(" test  ")
    fmt.Println(res)

    res = strings.TrimSpace(" This is test  ")
    fmt.Println(res)
}
```

**Output:**

```
test
This is test
```*