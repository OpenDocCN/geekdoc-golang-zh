<!--yml
category: 未分类
date: 2024-10-13 06:22:36
-->

# Type Implementing multiple interfaces in Go (Golang)

> 来源：[https://golangbyexample.com/type-implementing-multiple-interfaces-go/](https://golangbyexample.com/type-implementing-multiple-interfaces-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# Overview

A type implements an interface if it defines all methods of an interface. If that type defines all methods of another interface then it implements that interface. In essence, a type can implement multiple interfaces.

Let’s see an example

Assume we have an interface **animal** as below

```
type animal interface {
    breathe()
    walk()
}
```

Also let’s say we have a **mammal** interface too as below

```
type mammal interface {
    feed()
}
```

We also have a **lion** struct implementing this **animal** and **mammal** interface

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

type mammal interface {
    feed()
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
func (l lion) feed() {
    fmt.Println("Lion feeds young")
}
func main() {
    var a animal
    l := lion{}
    a = l
    a.breathe()
    a.walk()

    var m mammal
    m = l
    m.feed()
}
```

**Output**

```
Lion breathes
Lion walk
Lion feeds young
```

In the above program, the lion struct defines all methods of animal interface. It also defines all methods of mammal interface. That is why this works

```
var a animal
l := lion{}
a = l
a.breathe()
a.walk()
```

as well as this works

```
var m mammal
m = l
m.feed()
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*