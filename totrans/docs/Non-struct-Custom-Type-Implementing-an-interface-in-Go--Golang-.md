<!--yml
category: 未分类
date: 2024-10-13 06:22:52
-->

# Non-struct Custom Type Implementing an interface in Go (Golang)

> 来源：[https://golangbyexample.com/non-struct-type-implementing-interface-go/](https://golangbyexample.com/non-struct-type-implementing-interface-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

It is also perfectly ok for any non-struct custom type to implement an interface. Let’s see an example

Assume we have an interface **animal** as below

```
type animal interface {
    breathe()
    walk()
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

type cat string

func (c cat) breathe() {
	fmt.Println("Cat breathes")
}

func (c cat) walk() {
	fmt.Println("Cat walk")
}

func main() {
	var a animal

	a = cat("smokey")
	a.breathe()
	a.walk()
}
```

**Output**

```
Cat breathes
Cat walk
```

The above program illustrates the concept that any custom type can also implement an interface. The **cat** is of string type and it implements the **breathe** and **walk** method hence it is correct to assign an instance of **cat** type to a variable of **animal** type.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*