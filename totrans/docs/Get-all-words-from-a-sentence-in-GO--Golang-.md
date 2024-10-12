<!--yml
category: 未分类
date: 2024-10-13 06:12:04
-->

# Get all words from a sentence in GO (Golang)

> 来源：[https://golangbyexample.com/words-from-sentence-golang/](https://golangbyexample.com/words-from-sentence-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **Fields** method that can be used to split a string around an instance of one or more consecutive white space character.

Below is the signature of the function

```
func Fields(s string) []string 
```

As you can notice it returns a slice of substrings of the input string. Also, note that if the input string is empty it will return an empty slice if

*   The input string is empty

*   Input string contains only whitespaces

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 Input string contains single spaces
    res := strings.Fields("ab cd ef")
    fmt.Println(res)

    //Case 2 Input string doesn't contain white spaces
    res = strings.Fields("abcdef")
    fmt.Println(res)

    //Case 3 Input string contains double white spaces and spaces at end too.
    res = strings.Fields("ab  cd   ef ")
    fmt.Println(res)

    //Case 4: Input string contain white spaces only. Will output a empty slice
    res = strings.Fields("  ")
    fmt.Println(res)

    //Case 5: Input string is empty. Will output a empty slice
    res = strings.Fields("")
    fmt.Println(res)
}
```

**Output:**

```
[ab cd ef]
[abcdef]
[ab cd ef]
[]
[]
```*