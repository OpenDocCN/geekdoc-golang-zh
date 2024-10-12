<!--yml
category: 未分类
date: 2024-10-13 06:13:10
-->

# Find the index of the last instance of a substring in Go (Golang)

> 来源：[https://golangbyexample.com/index-last-occurence-substring-go/](https://golangbyexample.com/index-last-occurence-substring-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **LastIndex** method that can be used to get the index of the last occurrence of a substring in a particular string. It returns -1 is the substring is not present in the given string.

Below is the signature of the function

```
func LastIndex(s, substr string) int
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
    //Output will be 1 as "bc" is present in "abcdef" at index 1
    res := strings.LastIndex("abcdef", "bc")
    fmt.Println(res)

    //Output will be 8 as "cd" is present in "abcdefabcdef" at index 8
    res = strings.LastIndex("abcdefabcdef", "cd")
    fmt.Println(res)

    //Output will be -1 as "ba" is not present in "abcdef"
    res = strings.LastIndex("abcdef", "ba")
    fmt.Println(res)
}
```

**Output:**

```
1
8
-1
```*