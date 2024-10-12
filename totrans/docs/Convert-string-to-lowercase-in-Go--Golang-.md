<!--yml
category: 未分类
date: 2024-10-13 06:12:21
-->

# Convert string to lowercase in Go (Golang)

> 来源：[https://golangbyexample.com/string-lowercase-golang/](https://golangbyexample.com/string-lowercase-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **ToLower** method that can be used to convert all Unicode letters to their lower case. This method will return the copy of the string as in GO string are immutable.

Below is the signature of the function

```
func ToLower(s string) string
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
    res := strings.ToLower("ABC")
    fmt.Println(res)

    res = strings.ToLower("ABC12$a")
    fmt.Println(res)
}
```

**Output:**

```
abc
abc12$a
```*