<!--yml
category: 未分类
date: 2024-10-13 06:30:40
-->

# Nested Packages in Go (Golang)

> 来源：[https://golangbyexample.com/nested-packages-golang/](https://golangbyexample.com/nested-packages-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

In GO it is possible to create nested packages.  Nested packages are those packages that lie within another package. Let’s see an example

# **Example**

Create a directory named **learn**. Let’s create a module with import path as **“sample.com/learn”** in the learn directory.

```
go mod init sample.com/learn
```

It will create a go.mod file

**go.mod**

```
module sameple.com/learn

go 1.14
```

Let’s create below files and directories

*   learn/main.go

*   learn/math/math.go

*   learn/math/advanced/advanced.go

You can see that **advanced** is a nested package inside the **math** package.

**learn/math/math.go**

```
package math
func Add(a, b int) int {
    return a + b
}
func Subtract(a, b int) int {
    return a - b
}
```

**learn/math/advanced/advanced.go**

```
package advanced
func Square(a int) int {
    return a * a
}
```

**learn/main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/math"
    "sample.com/learn/math/advanced"
)
func main() {
    fmt.Println(math.Add(2, 1))
    fmt.Println(math.Subtract(2, 1))
    fmt.Println(advanced.Square(2))
}
```

Let’s run this program

```
learn $ go install
learn $ learn
3
1
4
```

Points to note about above program

*   We imported the **advanced** package in main.go with full qualified path i.e,  **import “sample.com/learn/math/advanced”**

*   **Square** function is referred to using **advanced** package i.e, **advanced.Square(2)**

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*