<!--yml
category: 未分类
date: 2024-10-13 06:28:10
-->

# Constant array or slice in Go (Golang)

> 来源：[https://golangbyexample.com/constant-array-golang/](https://golangbyexample.com/constant-array-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Go only supports four types of constant

*   Numeric ( int, int64, float, float64, complex128 etc)
*   String
*   Character or rune
*   Boolean

Go doesn’t support const array or slice. It is because in go constant value is computed at compile time. Arrays or slices are always evaluated at run time. So below program would raise a compilation error

# **Example**

```
package main
func main() {
	const e = [1]int{1}
}
```

**Output**

```
const initializer [1]int literal is not a constant
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*