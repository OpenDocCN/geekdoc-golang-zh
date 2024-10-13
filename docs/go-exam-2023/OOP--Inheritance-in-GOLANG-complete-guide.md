<!--yml

category: 未分类

date: 2024-10-13 06:00:50

-->

# OOP：GOLANG 中继承的完整指南

> 来源：[`golangbyexample.com/oop-inheritance-golang-complete/`](https://golangbyexample.com/oop-inheritance-golang-complete/)

我们将尝试通过与 JAVA 中的继承进行比较来解释 GO 中的继承。我们首先想提到的是 GOLANG 没有像 JAVA 那样的**“Extends”**和**“Implements”**关键字。Go 以不同的方式提供有限的“**Extends**”和“**Implements**”功能，各自有其限制。在继续理解 GO 中的继承之前，有一些值得提及的要点。

+   Go 更倾向于组合而非继承。它允许将结构体嵌入到其他结构体中。

+   Go 不支持类型继承。

我们将从 GO 中继承的最简单示例开始。然后列出一个限制或缺失的功能。在后续的迭代中，我们将修复限制或继续添加缺失的功能，直到我们编写出展示 GO 中继承所有可能/不可能特性的程序。所以让我们开始吧。

继承的基本用例是子类型应该能够访问父类型的公共数据和方法。这在 GO 中通过嵌入实现。基结构体嵌入在子结构体中，子结构体可以直接访问基的数据显示和方法。请参见下面的代码：子结构体能够直接访问数据**“color”**并直接调用函数**“say()”**。

**程序 1**

```go
package main
import "fmt"
type base struct {
    color string
}
func (b *base) say() {
    fmt.Println("Hi from say function")
}
type child struct {
    base  //embedding
    style string
}
func main() {
    base := base{color: "Red"}
    child := &child{
        base:  base,
        style: "somestyle",
    }
    child.say()
    fmt.Println("The color is " + child.color)
}
```

**输出：**

```go
Hi from say function
The color is Red
```

上述程序的一个限制是，你不能将子类型传递给一个期望基类型的函数，因为 GO 不允许类型继承。例如，下面的代码无法编译并给出错误——“**无法将 child（类型*child）用作传递给 check 的参数中的类型 base**”。

**程序 2**

```go
package main
import "fmt"
type base struct {
    color string
}
func (b *base) say() {
    fmt.Println("Hi from say function")
}
type child struct {
    base  //embedding
    style string
}
func check(b base) {
    b.say()
}
func main() {
    base := base{color: "Red"}
    child := &child{
        base:  base,
        style: "somestyle",
    }
    child.say()
    fmt.Println("The color is " + child.color)
    check(child)
}
```

**输出：**

```go
cannot use child (type *child) as type base in argument to check
```

上述错误基本上说明在 GO 中仅通过使用嵌入不可能实现子类型。让我们尝试修复这个错误。这就是 GO 接口派上用场的地方。请参见下面版本的程序，它除了上述功能外，还修复了这个子类型错误。

**程序 3**

```go
package main
import "fmt"
type iBase interface {
    say()
}
type base struct {
    color string
}
func (b *base) say() {
    fmt.Println("Hi from say function")
}
type child struct {
    base  //embedding
    style string
}
func check(b iBase) {
    b.say()
}
func main() {
    base := base{color: "Red"}
    child := &child{
        base:  base,
        style: "somestyle",
    }
    child.say()
    fmt.Println("The color is " + child.color)
    check(child)
}
```

**输出：**

```go
Hi from say function
The color is Red
Hi from say function
```

在上述程序中，我们：(a) 创建了一个接口**“iBase”**，其中有**“say”**方法 (b) 我们将**“check”**方法更改为接受 iBase 类型的参数。

由于基结构体实现了**“say”**方法，反过来，子结构体嵌入了基结构体。因此，子方法间接实现了**“say”**方法，成为**“iBase”**的一种类型，这就是我们现在可以将子结构体传递给 check 函数的原因。很好，我们现在使用结构体和接口的组合修复了一个限制。

但还有一个限制。假设子类和基类都有一个名为 **“clear”** 的函数。现在 **“say”** 方法调用 **“clear”** 方法。当使用子结构调用 **“say”** 方法时，**“say”** 方法将调用基类的 **“clear”** 方法，而不是子类的 **“clear”** 方法。请见下面的示例。

**程序 4**

```go
package main
import "fmt"
type iBase interface {
    say()
}
type base struct {
    color string
}
func (b *base) say() {
    b.clear()
}
func (b *base) clear() {
    fmt.Println("Clear from base's function")
}
type child struct {
    base  //embedding
    style string
}
func (b *child) clear() {
    fmt.Println("Clear from child's function")
}
func check(b iBase) {
    b.say()
}
func main() {
    base := base{color: "Red"}
    child := &child{
        base:  base,
        style: "somestyle",
    }
    child.say()
}
```

**输出：**

```go
Clear from base's function
```

正如你所看到的，上述基类的 **“clear”** 函数被调用，而不是子类的 **“clear”** 方法。这与 Java 不同，在 Java 中会调用 **“child”** 的 **“clear”** 方法。

解决上述问题的一种方法是将 **“clear”** 作为基结构中的函数类型属性。这在 GO 中是可能的，因为函数是 GO 中的一等公民。请见下面的解决方案。

**程序 5**

```go
package main
import "fmt"
type iBase interface {
    say()
}
type base struct {
    color string
    clear func()
}
func (b *base) say() {
    b.clear()
}
type child struct {
    base  //embedding
    style string
}
func check(b iBase) {
    b.say()
}
func main() {
    base := base{color: "Red",
        clear: func() {
            fmt.Println("Clear from child's function")
        }}
    child := &child{
        base:  base,
        style: "somestyle",
    }
    child.say()
}
```

**输出：**

```go
Clear from child's function
```

让我们尝试为上面的程序添加一个新特性——

+   多重继承——子结构应该能够从两个基础结构访问多个属性和方法，并且子类型化也应该是可能的。以下是代码。

**程序 6**

```go
package main
import "fmt"
type iBase1 interface {
    say()
}
type iBase2 interface {
    walk()
}
type base1 struct {
    color string
}
func (b *base1) say() {
    fmt.Println("Hi from say function")
}
type base2 struct {
}
func (b *base1) walk() {
    fmt.Println("Hi from walk function")
}
type child struct {
    base1 //embedding
    base2 //embedding
    style string
}
func (b *child) clear() {
    fmt.Println("Clear from child's function")
}
func check1(b iBase1) {
    b.say()
}
func check2(b iBase2) {
    b.walk()
}
func main() {
    base1 := base1{color: "Red"}
    base2 := base2{}
    child := &child{
        base1: base1,
        base2: base2,
        style: "somestyle",
    }
    child.say()
    child.walk()
    check1(child)
    check2(child)
}
```

**输出：**

```go
Hi from say function
Hi from walk function
Hi from say function
Hi from walk function
```

在上面的程序中，子结构同时嵌入了 base1 和 base2。它也可以作为 iBase1 和 iBase2 接口的实例传递给 check1 和 check2 函数。这就是我们实现多重继承的方式。

现在一个大问题是我们如何在 GO 中实现 **“类型层次结构”**。如前所述，Go 不允许类型继承，因此没有类型层次结构。GO 有意不允许此特性**，因此** 接口行为的任何变化仅传播到其定义了接口所有方法的直接结构。

尽管我们可以像下面这样使用接口和结构体实现类型层次结构。

**程序 7**

```go
package main
import "fmt"
type iAnimal interface {
    breathe()
}
type animal struct {
}
func (a *animal) breathe() {
    fmt.Println("Animal breate")
}
type iAquatic interface {
    iAnimal
    swim()
}
type aquatic struct {
    animal
}
func (a *aquatic) swim() {
    fmt.Println("Aquatic swim")
}
type iNonAquatic interface {
    iAnimal
    walk()
}
type nonAquatic struct {
    animal
}
func (a *nonAquatic) walk() {
    fmt.Println("Non-Aquatic walk")
}
type shark struct {
    aquatic
}
type lion struct {
    nonAquatic
}
func main() {
    shark := &shark{}
    checkAquatic(shark)
    checkAnimal(shark)
    lion := &lion{}
    checkNonAquatic(lion)
    checkAnimal(lion)
}
func checkAquatic(a iAquatic) {}
func checkNonAquatic(a iNonAquatic) {}
func checkAnimal(a iAnimal) {}
```

请查看上面的程序，我们是如何能够创建层次结构的（见下文）。这是 Go 创建类型层次结构的惯用方法，我们通过在结构体层级和接口层级上使用嵌入来实现这一点。需要注意的是，如果你希望在类型层次结构中有所区分，比如 **“鲨鱼”** 不应该同时是 **“iAquatic”** 和 **“iNonAquatic”**，那么在 **“iAquatic”** 和 **“iNonAquatic”** 的方法集中，应该至少有一个方法在另一个中不存在。在我们的例子中，**“游泳”** 和 **“行走”** 就是这些方法。

```go
iAnimal
--iAquatic
----shark
--iNonAquatic
----lion
```

**结论：**

Go 不支持类型继承，但可以通过嵌入实现相同的功能，不过在创建这种类型层次结构时需要谨慎。此外，Go 不提供方法重写。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [继承](https://golangbyexample.com/tag/inheritance/)*   [面向对象编程](https://golangbyexample.com/tag/oop/)*   [面向对象程序设计](https://golangbyexample.com/tag/oops/)
