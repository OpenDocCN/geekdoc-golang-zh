<!--yml
category: 未分类
date: 2024-10-13 06:34:44
-->

# How to access the struct from another package in Go (Golang)

> 来源：[https://golangbyexample.com/struct-another-package-golang/](https://golangbyexample.com/struct-another-package-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The struct name in another package must start with a capital letter so that it is public outside its package. If the struct name starts with lowercase then it will not be visible outside its package.

To access a struct outside its package we need to import the package first which contains that struct.

# **Program**

Here is the code for the same

**go.mod**

```
module sample.com/learn
go 1.16
```

**model/person.go**

```
package model
type Person struct {
    Name string
}
```

**main.go**

```
package main
import (
    "fmt"
    "sample.com/learn/person"
)
func main() {
    p := &model.Person{
        Name: "John",
    }
    fmt.Println(p.Name)
}
```

In this program, we first import the **model** package from the **main** package as below.

```
"sample.com/learn/model"
```

Then we access the **Person** struct as below from the **main** package

```
p := &model.Person{
   Name: "John",
}
```

This works because **Person** struct name is in uppercase.

Let’s change the struct name to lower case and run this program. It gives the below compilation error

```
cannot refer to unexported name model.person
```

Also, check out our Golang comprehensive tutorial Series – [Golang Comprehensive Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*