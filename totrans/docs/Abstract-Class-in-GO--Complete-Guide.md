<!--yml
category: 未分类
date: 2024-10-13 06:00:55
-->

# Abstract Class in GO: Complete Guide

> 来源：[https://golangbyexample.com/go-abstract-class/](https://golangbyexample.com/go-abstract-class/)

Go Interface doesn’t have fields and also it doesn’t allow the definition of methods inside it. Any type needs to implements all methods of interface to become of that interface type. There are use cases where it is useful to have a default implementation of a method and also default fields in GO. Before understanding how to do it lets first understand the requirements of an abstract class:

1.  Abstract class should have default fields
2.  Abstract class should have the default method
3.  It should not be possible to create a direct instance of the abstract class

We will use a combination of a **interface (abstract interface)** and **struct (abstract concrete type).** Together they can provide the functionalities of an abstract class. See the below program:

```
package main

import "fmt"

//Abstract Interface
type iAlpha interface {
    work()
    common()
}

//Abstract Concrete Type
type alpha struct {
    name string
}

func (a *alpha) common() {
    fmt.Println("common called")
}

//Implementing Type
type beta struct {
    alpha
}

func (b *beta) work() {
    fmt.Println("work called")
    fmt.Printf("name is %s\n", b.name)
    b.common()
}

func main() {
    a := alpha{
        name: "test",
    }
    b := &beta{
        alpha: a,
    }
    b.work()
}
```

Output:

```
work called
name is test
common called
```

In above program:

*   We created an abstract interface **iAlpha**, an abstract concrete struct **alpha,** and an implementor struct **beta**
*   **alpha** struct is embedded in **beta** struct
*   The beta struct is able to access default field **“name”**
*   The beta struct is able to access the default method **“common”**
*   It is not able to create a direct instance of **iAlpha ** as **alpha** struct only implement only one of the method of **iAlpha.**

So it fulfills all three requirements but there is also one limitation of the above method. It is not possible to call the “**work**” method from **“common”** method of alpha. Basically there is no way to call an undefined method of the abstract interface from default methods of an abstract concrete type. There is one way to fix it, though. See below program

```
package main

import "fmt"

//Abstract Interface
type iAlpha interface {
    work()
    common()
}

//Abstract Concrete Type
type alpha struct {
    name string
    work func()
}

func (a *alpha) common() {
    fmt.Println("common called")
    a.work()
}

//Implementing Type
type beta struct {
    alpha
}

func (b *beta) work() {
    fmt.Println("work called")
    fmt.Printf("name is %s\n", b.name)
}

func main() {
    a := alpha{
        name: "test",
    }
    b := &beta{
        alpha: a,
    }
    b.alpha.work = b.work
    b.common()
}
```

Output:

```
common called
work called
name is test
```

In the above program:

*   We created a new field **“work”** of type func in alpha
*   We assigned alpha’s **“work”** method to beta **“work”** method

The only problem with the above program is that it is possible to create a direct instantiation of **alpha** struct and by providing the definition of **work** method, an instance of type **iAlpha** is created. This violates point 3 of the above Abstract class requirement as without creating our own new type we are able to create a type of **iAlpha.** Let’s try to fix this problem. The below program additionally also solves the problem where it was not possible to call the undefined methods from default methods.

```
package main

import "fmt"

//Abstract Interface
type iAlpha interface {
    work()
    common(iAlpha)
}

//Abstract Concrete Type
type alpha struct {
    name string
}

func (a *alpha) common(i iAlpha) {
    fmt.Println("common called")
    i.work()
}

//Implementing Type
type beta struct {
    alpha
}

func (b *beta) work() {
    fmt.Println("work called")
    fmt.Printf("Name is %s\n", b.name)
}

func main() {
    a := alpha{
        name: "test",
    }
    b := &beta{
        alpha: a,
    }
    b.common(b)
}
```

Output:

```
common called
work called
Name is test
```

In the above program:

*   All the default methods will accept the first argument of interface type **iAlpha.** All the undefined methods of **alpha** struct will be called using this argument from default methods.

**Conclusion:** We can see in the above program that we are able to fulfill all three requirements of an abstract class. This is one of the ways to simulate abstract class in GO.

*   [abstract](https://golangbyexample.com/tag/abstract/)*   [abstract class](https://golangbyexample.com/tag/abstract-class/)*   [class](https://golangbyexample.com/tag/class/)*   [complete](https://golangbyexample.com/tag/complete/)*   [go](https://golangbyexample.com/tag/go/)*   [guide](https://golangbyexample.com/tag/guide/)*   [oop](https://golangbyexample.com/tag/oop/)