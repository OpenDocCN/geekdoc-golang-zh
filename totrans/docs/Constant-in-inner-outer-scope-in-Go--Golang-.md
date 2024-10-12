<!--yml
category: 未分类
date: 2024-10-13 06:27:49
-->

# Constant in inner/outer scope in Go (Golang)

> 来源：[https://golangbyexample.com/constant-scope-inner-outer-go/](https://golangbyexample.com/constant-scope-inner-outer-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

A constant declared within an inner scope having a same name as constant declared in the outer scope will shadow the constant in outer scope.

# **Example**

```
package main
import "fmt"
const a = 123
func main() {
    const a = 456
    fmt.Println(a)
}
```

**Output**

```
456
```

The above program prints 456 as output because the inner constant declared has that value

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*