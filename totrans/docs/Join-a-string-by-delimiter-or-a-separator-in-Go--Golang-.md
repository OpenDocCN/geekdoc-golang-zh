<!--yml
category: 未分类
date: 2024-10-13 06:12:08
-->

# Join a string by delimiter or a separator in Go (Golang)

> 来源：[https://golangbyexample.com/go-join-string-delimiter/](https://golangbyexample.com/go-join-string-delimiter/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In GO string are UTF-8 encoded. **strings** package of GO provides a **Join** method that can be used to join a string based upon a delimiter.

Below is the signature of the function

```
func Join(a []string, sep string)
```

As you can notice this function takes a slice of string and a delimiter and it returns a combined string joined by a delimiter. The delimiter or separator is placed between elements of the input string slice. Please note

*   It will return an empty string if the length of the input slice is zero
*   It will output a string combined from the slice of strings if the input delimiter or separator is empty.

Let’s look at the working code

# **Code:**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 s contains sep. Will output slice of length 3
    res := strings.Join([]string{"ab", "cd", "ef"}, "-")
    fmt.Println(res)

    //Case 2 slice is empty. It will output a empty string
    res = strings.Join([]string{}, "-")
    fmt.Println(res)

    //Case 3 sep is empty. It will output a string combined from the slice of strings
    res = strings.Join([]string{"ab", "cd", "ef"}, "")
    fmt.Println(res)
}
```

**Output:**

```
ab-cd-ef

abcdef
```*