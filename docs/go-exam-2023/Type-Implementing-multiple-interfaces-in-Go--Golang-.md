<!--yml

分类：未分类

日期：2024-10-13 06:22:36

-->

# 类型在 Go 中实现多个接口

> 来源：[`golangbyexample.com/type-implementing-multiple-interfaces-go/`](https://golangbyexample.com/type-implementing-multiple-interfaces-go/)

目录

+   概述

+   代码

# 概述

一个类型如果定义了接口的所有方法，则实现该接口。如果该类型定义了另一个接口的所有方法，则它也实现那个接口。本质上，一个类型可以实现多个接口。

让我们看一个例子

假设我们有一个接口**动物**如下

```go
type animal interface {
    breathe()
    walk()
}
```

另外假设我们也有一个**哺乳动物**接口如下

```go
type mammal interface {
    feed()
}
```

我们还有一个**狮子**结构体实现了这个**动物**和**哺乳动物**接口

```go
type lion struct {
    age int
}
```

# **代码**

```go
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

**输出**

```go
Lion breathes
Lion walk
Lion feeds young
```

在上面的程序中，狮子结构体定义了动物接口的所有方法。它也定义了哺乳动物接口的所有方法。这就是为什么这样可行

```go
var a animal
l := lion{}
a = l
a.breathe()
a.walk()
```

这同样也是可行的

```go
var m mammal
m = l
m.feed()
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
