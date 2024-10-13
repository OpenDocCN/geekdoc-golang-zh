<!--yml

分类：未分类

日期：2024-10-13 06:22:31

-->

# 在 Go（Golang）中，接口是隐式实现的。

> 来源：[https://golangbyexample.com/interface-implit-implementation-golanng/](https://golangbyexample.com/interface-implit-implementation-golanng/)

并没有明确声明一个类型实现了一个接口。实际上，在 Go 中并不存在类似 Java 的 **“implements”** 关键字。如果一个类型实现了接口的所有方法，那么它就实现了这个接口。

让我们来看一个示例。

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

**输出**

```
Lion breathes
Lion walk
```

没有明确声明 **狮子** 结构体实现了 **动物** 接口。在编译时，Go 会注意到 **狮子** 结构体实现了 **动物** 接口的所有方法，因此是被允许的。任何其他实现了 **动物** 接口所有方法的类型都成为该接口类型。

这在接口和定义接口所有方法的类型位于不同包时也同样成立。

让我们看看另一个类型实现动物接口的更复杂示例。如果我们定义一个 **狗** 结构体，并且它实现了 **呼吸** 和 **行走** 方法，那么它也将成为一种动物。让我们来看一个示例。

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
    var a animal

    a = lion{age: 10}
    a.breathe()
    a.walk()

    a = dog{age: 5}
    a.breathe()
    a.walk()
}
```

**输出**

```
Lion breathes
Lion walk
Dog breathes
Dog walk
```

**狮子** 和 **狗** 都实现了 **呼吸** 和 **行走** 方法，因此它们属于 **动物** 类型，可以正确地赋值给接口类型的变量。
