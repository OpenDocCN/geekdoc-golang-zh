<!--yml

类别：未分类

日期：2024-10-13 06:00:55

-->

# GO 中的抽象类：完整指南

> 来源：[`golangbyexample.com/go-abstract-class/`](https://golangbyexample.com/go-abstract-class/)

Go 接口没有字段，也不允许在其中定义方法。任何类型都需要实现接口的所有方法才能成为该接口类型。有些用例中，提供方法的默认实现和默认字段在 GO 中是有用的。在理解如何实现之前，首先让我们了解抽象类的要求：

1.  抽象类应该有默认字段

1.  抽象类应该有默认方法

1.  不应该能够直接创建抽象类的实例。

我们将使用**接口（抽象接口）**和**结构（抽象具体类型）**的组合。它们可以提供抽象类的功能。请参见下面的程序：

```go
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

输出：

```go
work called
name is test
common called
```

在上述程序中：

+   我们创建了一个抽象接口**iAlpha**、一个抽象具体结构**alpha**和一个实现者结构**beta**。

+   **alpha**结构嵌入在**beta**结构中。

+   beta 结构能够访问默认字段**“name”**

+   beta 结构能够访问默认方法**“common”**

+   无法创建**iAlpha**的直接实例，因为**alpha**结构仅实现了**iAlpha**的一个方法。

因此它满足所有三个要求，但上述方法也有一个限制。无法从 alpha 的**“common”**方法调用**“work”**方法。基本上，无法从抽象具体类型的默认方法调用抽象接口的未定义方法。不过，有一种方法可以解决这个问题。请参见下面的程序。

```go
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

输出：

```go
common called
work called
name is test
```

在上述程序中：

+   我们在 alpha 中创建了一个类型为 func 的新字段**“work”**

+   我们将 alpha 的**“work”**方法赋值给 beta 的**“work”**方法

上述程序唯一的问题是可以直接实例化**alpha**结构，并通过提供**work**方法的定义，创建类型为**iAlpha**的实例。这违反了上述抽象类要求的第 3 点，因为在未创建我们自己的新类型的情况下，我们能够创建**iAlpha**的类型。让我们尝试解决这个问题。下面的程序还解决了无法从默认方法调用未定义方法的问题。

```go
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

输出：

```go
common called
work called
Name is test
```

在上述程序中：

+   所有默认方法将接受接口类型**iAlpha**的第一个参数。所有**alpha**结构的未定义方法将通过从默认方法传入的这个参数进行调用。

**结论：** 我们可以在上述程序中看到，能够满足抽象类的所有三个要求。这是模拟 GO 中抽象类的一种方法。

+   [抽象](https://golangbyexample.com/tag/abstract/)*   [抽象类](https://golangbyexample.com/tag/abstract-class/)*   [类](https://golangbyexample.com/tag/class/)*   [完整](https://golangbyexample.com/tag/complete/)*   [Go](https://golangbyexample.com/tag/go/)*   [指南](https://golangbyexample.com/tag/guide/)*   [面向对象编程](https://golangbyexample.com/tag/oop/)
