<!--yml
category: 未分类
date: 2024-10-13 06:22:47
-->

# Inner Working or Internals of an Interface in Go (Golang)

> 来源：[https://golangbyexample.com/inner-working-interface-golang/](https://golangbyexample.com/inner-working-interface-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Like any other variable, an interface variable is represented by a type and value. Interface value, in turn under the hood consists of two tuple

*   Underlying Type
*   Underlying Value

See below diagram which illustrates what we mentioned above

![](img/e09464771d53171817cfeecf9da6a93e.png)

Let’s see an example and then we can create a similar diagram as above for that example.

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
    a.breathe()
    a.walk()
}
```

**Output**

```
Lion breathes
Lion walk
```

For the above case,  lion struct implementing the animal interface would be like below

![](img/c9cc4eb8c2e7148fd7877e127d773ca6.png)

Golang provides format identifiers to print the underlying type and underlying value represented by the interface value.

*   %T can be used to print the concrete type of the interface value

*   %v can be used to print the concrete value of the interface value.

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