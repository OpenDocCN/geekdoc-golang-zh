<!--yml
category: 未分类
date: 2024-10-13 06:12:57
-->

# Replace all instances of a substring with another in Go (Golang)

> 来源：[https://golangbyexample.com/replace-all-instances-substring-go/](https://golangbyexample.com/replace-all-instances-substring-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **ReplaceAll** method that can be used to replace all non-overlapping instances of a given substring with a new substring. It returns a copy of the string

Below is the signature of the function. The function will replace all non-overlapping instances of **old** with **new** in string **s**. If **old** is empty then it inserts **new** between it each valid UTF-8 sequence of bytes in the string **s**.

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
    res := strings.ReplaceAll("abcdabxyabr", "ab", "12")
    fmt.Println(res)

    res = strings.ReplaceAll("abcdabxyabr", "", "12")
    fmt.Println(res)
}
```

**Output**

```
12cd12xy12r
12a12b12c12d12a12b12x12y12a12b12r12
```*