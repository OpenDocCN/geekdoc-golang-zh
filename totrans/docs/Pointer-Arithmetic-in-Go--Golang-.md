<!--yml
category: 未分类
date: 2024-10-13 06:29:00
-->

# Pointer Arithmetic in Go (Golang)

> 来源：[https://golangbyexample.com/pointer-arithmetic-golang/](https://golangbyexample.com/pointer-arithmetic-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Pointer arithmetic is not possible in golang unlike C language. It raises compilation error.

# **Program**

```
package main
func main() {
    a := 1
    b := &a
    b = b + 1
}
```

**Output**

Above program raises compilation error

```
invalid operation: b + 1 (mismatched types *int and int)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*