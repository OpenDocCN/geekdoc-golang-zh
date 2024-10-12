<!--yml
category: 未分类
date: 2024-10-13 06:13:06
-->

# Replace a character with another in a string in Go (Golang)

> 来源：[https://golangbyexample.com/replace-character-string-go/](https://golangbyexample.com/replace-character-string-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **Replace** method that can be used to replace a character with another in a given string. It returns a copy of the string.

Below is the signature of the function.

*   Supplies the character that is to be replaced as **old**

*   Supply the character you want to be replaced with as **new**

*   **n** represents the number of replacements. If **n** is -1 then all instances of **old** are replaced with **new**

```
func Replace(s, old, new string, n int)
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
    //It will replaces 1 instance of a with 1
    res := strings.Replace("abcdabxyabr", "a", "1", 1)
    fmt.Println(res)

    //It will replace all instances of a with 1
    res = strings.Replace("abcdabxyabr", "a", "1", -1)
    fmt.Println(res)
}
```

**Output:**

```
1bcdabxyabr
1bcd1bxy1br
```*