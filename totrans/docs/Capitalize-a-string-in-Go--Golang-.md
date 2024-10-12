<!--yml
category: 未分类
date: 2024-10-13 06:12:30
-->

# Capitalize a string in Go (Golang)

> 来源：[https://golangbyexample.com/capitalize-string-golang/](https://golangbyexample.com/capitalize-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In Golang string are UTF-8 encoded. **strings** package of go provides a **Title** method that can be used to convert all first letters of all words in a sentence as capital. It returns a copy of the string with all first letters of the words capitalized.

Below is the signature of the function

```
func Title(s string) string
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
    res := strings.Title("this is a test sentence")
    fmt.Println(res)
}
```

**Output:**

```
This Is A Test Sentence
```*