<!--yml

类别：未分类。

日期：2024-10-13 06:22:06

-->

# 在 Go (Golang)中嵌入接口。

> 来源：[`golangbyexample.com/embedding-interfaces-go/`](https://golangbyexample.com/embedding-interfaces-go/)

接口可以嵌入另一个接口，也可以嵌入到结构中。让我们逐一查看。

目录

**在另一个接口中嵌入接口**

+   **在结构中嵌入接口** * # **在另一个接口中嵌入接口**

接口可以嵌入任意数量的接口，也可以嵌入到任意接口中。嵌入接口的所有方法成为嵌入接口的一部分。这是一种通过合并一些小接口来创建新接口的方法。让我们通过一个例子来理解。

假设我们有一个接口**动物**如下。

```go
type animal interface {
    breathe()
    walk()
}
```

假设有另一个接口名为**人类**，它嵌入了**动物**接口。

```go
type human interface {
    animal
    speak()
}
```

所以，如果任何类型需要实现**人类**接口，那么它必须定义。

+   **breathe()**和**walk()**方法的动物接口被嵌入在人类中。

+   **speak()**方法的人类接口。

```go
package main

import "fmt"

type animal interface {
	breathe()
	walk()
}

type human interface {
	animal
	speak()
}

type employee struct {
	name string
}

func (e employee) breathe() {
	fmt.Println("Employee breathes")
}

func (e employee) walk() {
	fmt.Println("Employee walk")
}

func (e employee) speak() {
	fmt.Println("Employee speaks")
}

func main() {
	var h human

	h = employee{name: "John"}
	h.breathe()
	h.walk()
	h.speak()
}
```

**输出**

```go
Employee breathes
Employee walk
Employee speaks
```

作为另一个例子，Golang 的**io**包的 ReaderWriter 接口（[`golang.org/pkg/io/#ReadWriter`](https://golang.org/pkg/io/#ReadWriter)）嵌入了另外两个接口。

+   读取器接口 – [`golang.org/pkg/io/#Reader`](https://golang.org/pkg/io/#Reader)

+   写入器接口 – [`golang.org/pkg/io/#Writer`](https://golang.org/pkg/io/#Writer)

```go
type ReadWriter interface {
    Reader
    Writer
}
```

# **在结构中嵌入接口**

接口也可以嵌入到结构中。嵌入接口的所有方法都可以通过该结构调用。如何调用这些方法将取决于嵌入接口是命名字段还是未命名/匿名字段。

+   如果嵌入的接口是命名字段，则必须通过命名接口名称调用接口方法。

+   如果嵌入的接口是未命名/匿名字段，则可以直接引用接口方法或通过接口名称引用。

让我们看一个程序来说明上述要点。

```go
package main

import "fmt"

type animal interface {
    breathe()
    walk()
}

type dog struct {
    age int
}

func (d dog) breathe() {
    fmt.Println("Dog breathes")
}

func (d dog) walk() {
    fmt.Println("Dog walk")
}

type pet1 struct {
    a    animal
    name string
}

type pet2 struct {
    animal
    name string
}

func main() {
    d := dog{age: 5}
    p1 := pet1{name: "Milo", a: d}

    fmt.Println(p1.name)
    // p1.breathe()
    // p1.walk()
    p1.a.breathe()
    p1.a.walk()

    p2 := pet2{name: "Oscar", animal: d}
    fmt.Println(p1.name)
    p2.breathe()
    p2.walk()
    p1.a.breathe()
    p1.a.walk()
}
```

**输出**

```go
Milo
Dog breathes
Dod walk

Oscar
Dog breathes
Dog walk
Dog breathes
Dog walk
```

我们声明了两个结构**pet1**和**pet2**。**pet1**结构中有命名的**动物**接口。

```go
type pet1 struct {
    a    animal
    name string
}
```

**pet2**嵌入了未命名/匿名的**动物**接口。

```go
type pet2 struct {
    animal
    name string
}
```

对于**pet1**结构的一个实例，我们可以这样调用**breathe()**和**walk()**方法。

```go
p1.a.breathe()
p1.a.walk()
```

直接调用这些方法会导致编译错误。

```go
//p1.breathe()
//p1.walk()
```

```go
p1.breathe undefined (type pet1 has no field or method breathe)
p1.walk undefined (type pet1 has no field or method walk)
```

对于**pet2**结构的一个实例，我们可以直接调用**breathe()**和**walk()**方法。

```go
p2.breathe()
p2.walk()
```

如果嵌入接口是匿名或未命名的，我们可以直接访问嵌入接口的方法。

下面也是有效的另一种调用未命名/匿名嵌入接口的方法。

```go
p2.animal.breathe()
p2.animal.walk()
```

请注意，在创建**pet1**或**pet2**结构的实例时，嵌入的接口即**animal**是用实现该接口的类型初始化的，即**dog**。

```go
p1 := pet1{name: "Milo", a: d}
p2 := pet2{name: "Oscar", animal: d}
```

如果我们不初始化嵌入的接口**animal**，那么它将被初始化为接口的零值，即 nil。在这样的 pet1 或 pet2 结构实例上调用**breathe()**和**walk()**方法将导致恐慌。

+   [go](https://golangbyexample.com/tag/go/)*
