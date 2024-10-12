<!--yml
category: 未分类
date: 2024-10-13 06:13:45
-->

# Find and delete a character in string in Go (Golang)

> 来源：[https://golangbyexample.com/go-find-delete-character-in-string/](https://golangbyexample.com/go-find-delete-character-in-string/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

**strings** package of GO provides a **ReplaceAll** method that can be used to replace all non-overlapping instances of a given substring with a new substring. We can use this method to delete a character by providing the replacement of the character to be deleted as an empty string

Below is the signature of the function. The function will replace all non-overlapping instances of **old** with **new** in string **s**. To delete a character we can supply that character as **old** and **new** as an empty string

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
    res = strings.ReplaceAll("abcdabxyabr", "a", "")
    fmt.Println(res)
}
```

**Output:**

```
bcdbxybr
```

*   [golang](https://golangbyexample.com/tag/golang/)*