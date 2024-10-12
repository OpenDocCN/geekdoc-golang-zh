<!--yml
category: 未分类
date: 2024-10-13 06:33:26
-->

# Convert an array/slice into a JSON in Go (Golang)

> 来源：[https://golangbyexample.com/array-slice-json-golang/](https://golangbyexample.com/array-slice-json-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

**encoding/json** package provides the **Marshal** function which can be used to convert a golang array or slice into a JSON string and vice versa.

Let’s see an example

# **Example**

```
package main
import (
    "encoding/json"
    "fmt"
)
func main() {
    a := make([]string, 2)
    a[0] = "John"
    a[1] = "Sam"
    j, err := json.Marshal(a)
    if err != nil {
        fmt.Printf("Error: %s", err.Error())
    } else {
        fmt.Println(string(j))
    }
}
```

**Output**

```
["John","Sam"]
```*