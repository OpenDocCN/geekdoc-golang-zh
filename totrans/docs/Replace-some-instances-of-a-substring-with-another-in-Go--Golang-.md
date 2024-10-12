<!--yml
category: 未分类
date: 2024-10-13 06:13:01
-->

# Replace some instances of a substring with another in Go (Golang)

> 来源：[https://golangbyexample.com/replace-some-instances-substring-go/](https://golangbyexample.com/replace-some-instances-substring-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **Replace** method that can be used to replace some non-overlapping instances of a given substring with a new substring. It returns a copy of the string
Below is the signature of the function.

```
func Replace(s, old, new string, n int)
```

*   The function will replace all non-overlapping instances of **old** with **new** in string **s**.

*   If **old** is empty then it inserts **new** between it each valid UTF-8 sequence of bytes in the string **s**.

*   If **n** is negative then all instances of **old** will be replaced with **new**

Let’s look at the working code

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Replace("abcdabxyabr", "ab", "12", 1)
    fmt.Println(res)

    res = strings.Replace("abcdabxyabr", "ab", "12", -1)
    fmt.Println(res)
}
```

**Output:**

```
12cdabxyabr
12cd12xy12r
```*