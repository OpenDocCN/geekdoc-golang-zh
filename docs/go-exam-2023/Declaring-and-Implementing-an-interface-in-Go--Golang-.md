<!--yml

分类: 未分类

日期：2024-10-13 06:22:26

-->

# 在 Go (Golang) 中声明和实现接口

> 来源：[https://golangbyexample.com/declaring-implementing-interface-golang/](https://golangbyexample.com/declaring-implementing-interface-golang/)

目录

**   [概述](#Overview "Overview")

+   [声明接口](#Declaring_an_Interface "Declaring an Interface")

+   [实现接口](#Implementing_an_interface "Implementing an interface")*  *# **概述**

接口是 Go 中的一种类型，它是方法签名的集合。这些方法签名的集合旨在表示一种行为。接口仅声明方法集，任何实现接口所有方法的类型都是该接口类型。

# **声明接口**

让我们定义一个名为**动物**的接口。**动物**接口有两个方法**呼吸**和**行走**。它仅定义方法签名，别无其他。

```
type animal interface {
    breathe()
    walk()
}
```

方法签名包括

+   方法名称

+   参数的数量及每个参数的类型

+   返回值的数量及每个返回值的类型

通过上述声明，我们创建了一个新的接口类型，即**动物**。

定义一个**动物**类型的变量是可以的。让我们创建一个**动物**接口类型的变量。

```
package main

import "fmt"

type animal interface {
	breathe()
	walk()
}

func main() {
	var a animal

	fmt.Println(a)
	fmt.Printf("Underlying Type: %T\n", a)
	fmt.Printf("Underlying Value: %v\n", a)
}
```

**输出**

```
 <nil>Underlying Type: <nil>Underlying Value:</nil></nil> 
```

如上程序所示，创建一个接口类型的变量是可以的。它打印出 nil，因为接口的默认零值是 nil。

# **实现接口**

任何实现了呼吸和行走方法的类型都可以被称为实现了**动物**接口。因此，如果我们定义一个**狮子**结构体并实现呼吸和行走方法，那么它将实现动物接口。

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

我们声明一个动物接口类型的变量

```
var a animal
```

然后我们将一个狮子结构体的实例赋值给它。

```
a = lion{}
```

将**狮子**结构体的实例赋值给**动物**接口类型的变量是可行的，因为**狮子**结构体实现了**动物**的呼吸和行走方法。赋值时并不会检查类型，而只需检查赋值的类型是否实现了**呼吸**和**行走**方法。这个概念类似于鸭子类型，一个**狮子**可以像**动物**一样**呼吸**和**行走**，因此它就是一种**动物**。

没有明确声明一个类型实现了一个接口。实际上，在 Go 中并不存在类似 Java 的**“implements”**关键字。如果一个类型实现了接口的所有方法，则它实现了该接口。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
