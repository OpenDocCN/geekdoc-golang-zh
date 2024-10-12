<!--yml
category: 未分类
date: 2024-10-13 06:27:45
-->

# Can constant be reassigned after its declaration in Go (Golang)

> 来源：[https://golangbyexample.com/constant-reassign-go/](https://golangbyexample.com/constant-reassign-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Example](#Example "Example")*  *# **Overview**

Constant Variable cannot be reassigned after its declaration as it is constant and its value cannot change. If you try to reassign to a constant variable then it will raise a compilation error.

# **Example**

For example below code will raise a compilation error

```
package main
func main() {
    const a int = 8
    a = 9
}
```

**Output**

```
main.go:5:4: cannot assign to a
```

In the above program we created a constant first

```
const a int = 8
```

Then we try to assign a new value 9 to constant **a,** hence it raises a compilation error as constant once declared cannot be reassigned.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*