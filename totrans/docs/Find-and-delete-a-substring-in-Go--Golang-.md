<!--yml
category: 未分类
date: 2024-10-13 06:13:50
-->

# Find and delete a substring in Go (Golang)

> 来源：[https://golangbyexample.com/go-find-delete-substring/](https://golangbyexample.com/go-find-delete-substring/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**strings** package of GO provides a **ReplaceAll** method that can be used to replace all non-overlapping instances of a given substring with a new substring. We can use this method to delete a substring by providing the replacement of the substring to be deleted as an empty string

Below is the signature of the function. The function will replace all non-overlapping instances of **old** with **new** in string **s**. To delete a substring we can supply that substring as **old** and **new** as an empty string

```
func ReplaceAll(s, old, new string) string
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
    res = strings.ReplaceAll("abcabcabcdef", "ab", "")
    fmt.Println(res)
}
```

**Output:**

```
cccdef
```*