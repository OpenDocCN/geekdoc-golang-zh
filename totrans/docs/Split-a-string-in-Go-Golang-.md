<!--yml
category: 未分类
date: 2024-10-13 06:11:59
-->

# Split a string in Go(Golang)

> 来源：[https://golangbyexample.com/split-a-string-golang/](https://golangbyexample.com/split-a-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

In Golang string are UTF-8 encoded. **strings** package of GO provides a **Split** method that can be used to split a string by a separator.

Below is the signature of the function:

```
func Split(s, sep string) []string
```

As you can notice the return value of the **Split** function is a slice of string. Let’s notice some points about this method

*   Case 1: The **Split** method slices the **s** into substrings which are separated by **sep**. It will return a slice of string

*   Case 2: If **s** doesn’t contain **sep** then a slice of string of length 1 is returned. The only element of this slice will be **s**.

*   Case 3: If **sep** is empty and then it will return a slice of string after splitting **s** into each UTF-8 sequence

*   Case 4: If both **s** and **sep** are empty it will return an empty slice of string

# **Code**

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    //Case 1 s contains sep. Will output slice of length 3
    res := strings.Split("ab$cd$ef", "$")
    fmt.Println(res)

    //Case 2 s doesn't contain sep. Will output slice of length 1
    res = strings.Split("ab$cd$ef", "-")
    fmt.Println(res)

    //Case 3 sep is empty. Will output slice of length 8
    res = strings.Split("ab$cd$ef", "")
    fmt.Println(res)

    //Case 4 both s and sep are empty. Will output empty slice
    res = strings.Split("", "")
    fmt.Println(res)
}
```

**Output:**

```
[ab cd ef]
[ab$cd$ef]
[a b $ c d $ e f]
[]
```*