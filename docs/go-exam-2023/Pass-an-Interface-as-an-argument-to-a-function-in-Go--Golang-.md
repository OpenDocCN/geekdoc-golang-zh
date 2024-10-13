<!--yml

分类：未分类

日期：2024-10-13 06:21:40

-->

# 将接口作为参数传递给 Go（Golang）中的函数。

> 来源：[`golangbyexample.com/pass-interface-as-argument-function-go/`](https://golangbyexample.com/pass-interface-as-argument-function-go/)

函数可以接受接口类型的参数。该接口类型可以是任意类型。

+   常规接口

+   空接口

让我们逐个看一下这两个示例。

目录

**   常规接口

+   空接口*  *# **常规接口**

函数可以接受接口类型的参数。任何实现该接口的类型都可以作为参数传递给该函数。让我们通过一个示例来理解。假设我们有如下动物接口。

```go
type animal interface {
    breathe()
    walk()
}
```

我们有两个结构类型实现该接口。

```go
type lion struct {
    age int
}
```

和

```go
type dog struct {
     age int
}
```

在下面的代码中，我们有**callBreathe**和**callWalk**函数，它们接受**动物**接口类型的参数。**狮子**和**狗**实例都可以传递给该函数。我们创建**狮子**和**狗**类型的实例并将其传递给该函数。在编译期间，调用函数时不会检查类型，而只需检查传递给函数的类型是否实现了**呼吸**和**行走**方法。

```go
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

type dog struct {
     age int
}

func (l dog) breathe() {
	fmt.Println("Dog breathes")
}

func (l dog) walk() {
	fmt.Println("Dog walk")
}

func main() {
	l := lion{age: 10}
	callBreathe(l)
	callWalk(l)

	d := dog{age: 5}
	callBreathe(d)
	callWalk(d)
}

func callBreathe(a animal) {
	a.breathe()
}

func callWalk(a animal) {
	a.breathe()
}
```

**输出**

```go
Lion breathes
Lion walk
Dog breathes
Dog walk
```

# **空接口**

一个空接口没有方法，因此默认情况下所有类型都实现空接口。如果你编写一个接受空接口的函数，那么你可以将任何类型传递给该函数。请参见下面的工作代码。

```go
package main

import "fmt"

func main() {
    test("thisisstring")
    test("10")
    test(true)
}

func test(a interface{}) {
    fmt.Printf("(%v, %T)\n", a, a)
}
```

**输出：**

```go
(thisisstring, string)
(10, string)
(true, bool)
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
