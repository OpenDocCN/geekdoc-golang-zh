<!--yml
category: 未分类
date: 2024-10-13 06:22:41
-->

# Print underlying type and value of an interface in Go (Golang)

> 来源：[https://golangbyexample.com/print-type-value-interface-golang/](https://golangbyexample.com/print-type-value-interface-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Golang provides format identifiers to print the underlying type and underlying value represented by the interface value.

*   %T can be used to print the concrete type of the interface value
*   %v can be used to print the concrete value of the interface value.

Assume we have an interface **animal** as below

```
type animal interface {
    breathe()
    walk()
}
```

We also have a **lion** struct implementing this **animal** interface

```
type lion struct {
    age int
}
```

# **Code**

```
package main

import "fmt"

type animal interface {
    breathe()
    walk()
}

type lion struct {
    age int
}

func (l lion) breathe() {
    fmt.Println("Lion breathes")
}

func (l lion) walk() {
    fmt.Println("Lion walk")
}

func main() {
    var a animal
    a = lion{age: 10}
    fmt.Printf("Underlying Type: %T\n", a)
    fmt.Printf("Underlying Value: %v\n", a)
}
```

**Output**

```
Concrete Type: main.lion
Concrete Value: {10}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*