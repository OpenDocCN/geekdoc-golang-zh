<!--yml
category: 未分类
date: 2024-10-13 06:12:48
-->

# Count instances of substring in a string in Go (Golang)

> 来源：[https://golangbyexample.com/instances-substring-string-go/](https://golangbyexample.com/instances-substring-string-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code " Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **Count** method that can be used to get the number of non-overlapping instances of a substring in a particular string.
Below is the signature of the function

```
func Count(s, substr string) int
```

Also note that if the **substr** supplied to the function is an empty string, then return value will be 1+ count of Unicode Code points in the given string.

Let’s look at the working code

#  **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {

    //Case 1 Input string contains the substring
    res := strings.Count("abcdabcd", "ab")
    fmt.Println(res)

    //Case 1 Input string doesn't contains the substring
    res = strings.Count("abcdabcd", "xy")
    fmt.Println(res)

    //Case 1 Substring supplied is an empty string
    res = strings.Count("abcdabcd", "")
    fmt.Println(res)
}
```

**Output:**

```
2
0
9
```*