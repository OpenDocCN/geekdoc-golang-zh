<!--yml
category: 未分类
date: 2024-10-13 06:12:35
-->

# Trim prefix of a string in Go (Golang)

> 来源：[https://golangbyexample.com/trim-prefix-string-go/](https://golangbyexample.com/trim-prefix-string-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **TrimPrefix** method that can be used to remove a prefix string from the input string. If the input string doesn’t start with the given prefix, then the input string remains unchanged. Also, note that this function returns the copy of the string.

Below is the signature of the function

```
func TrimPrefix(s, prefix string) string
```

Let’s look at the working code

# **Code**

```
package main

import (
    "fmt"
    "strings"
)

func main() {

    //This will output removed
    res := strings.TrimPrefix("testremoved", "test")
    fmt.Println(res)

    //The input string will remain unchanged as it doesn't contain the test as prefix
    res2 := strings.TrimPrefix("tesremoved", "test")
    fmt.Println(res2)
}
```

**Output**:

```
removed
tesremoved
```*