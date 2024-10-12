<!--yml
category: 未分类
date: 2024-10-13 06:11:51
-->

# string compare in Go (Golang)

> 来源：[https://golangbyexample.com/compare-two-strings-golang/](https://golangbyexample.com/compare-two-strings-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In Golang string are UTF-8 encoded. **strings** package of GO provides a **Compare** method that can be used to compare two strings in Go. Note that this method compares strings lexicographically.

Below is the signature of the function

```
func Compare(a, b string) int
```

As you can notice the return value of the Compare function is an int. This value will be

*   0 if a==b

*   -1 if a < b

*   +1 if a > b

So if the return value is 0 then the two strings are equal. Let’s look at a working program

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    res := strings.Compare("abc", "abc")
    fmt.Println(res)

    res = strings.Compare("abc", "xyz")
    fmt.Println(res)

    res = strings.Compare("xyz", "abc")
    fmt.Println(res)
}
```

**Output:**

```
0
-1
1
```

*   [compare](https://golangbyexample.com/tag/compare/)*   [two strings](https://golangbyexample.com/tag/two-strings/)*