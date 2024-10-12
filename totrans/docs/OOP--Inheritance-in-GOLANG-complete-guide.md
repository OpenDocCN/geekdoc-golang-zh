<!--yml
category: 未分类
date: 2024-10-13 06:00:50
-->

# OOP: Inheritance in GOLANG complete guide

> 来源：[https://golangbyexample.com/oop-inheritance-golang-complete/](https://golangbyexample.com/oop-inheritance-golang-complete/)

We will try to explain inheritance in GO using comparisons with inheritance in JAVA. The first thing we want to mention here is that GOLANG doesn’t have keywords such as **“Extends”** and **“Implements”** keyword as in JAVA. Go does provide limited functionalities of “**Extends**” and “**Implements**” keyword in a different way which each having its own limitations. Before we proceed to understand inheritance in GO there are some points worth mentioning.

*   Go prefers composition over inheritance. It allows embedding of struct into other struct.
*   Go does not support type inheritance.

We will start with the simplest of an example of inheritance in GO. Then we will list down a limitation or missing feature. In further iterations, we will fix limitation or keep adding missing features until we have written a program which shows all properties of inheritance possible/impossible in Go. So let’s start

The very basic use case of inheritance is child type should be able to access common data and methods of a parent type.  This is done in GO via embedding. The base struct is embedded in child struct and base’s data and methods can directly be accessed by child struct. See below code:  child struct is able to access the data **“color”** directly and also call the function **“say()”** directly. 

**Program 1**

```
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

**Output:**

```
Hi from say function
The color is Red
```

One of the limitations of the above program is that you cannot pass the child type to a function that expects the base type as GO does not allow type inheritance. For example, the below code does not compile and gives an error – “**cannot use child (type *child) as type base in argument to check”**

**Program 2**

```
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

**Output:**

```
cannot use child (type *child) as type base in argument to check
```

The above error tells basically that sub-typing is not possible in GO by just using embedding. Let’s try to fix this error. This is where GO interfaces come into the picture. See below version of the program which in addition to above functionalities also fixes this sub-typing error 

**Program 3**

```
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

**Output:**

```
Hi from say function
The color is Red
Hi from say function
```

In the above program, we:    (a) Created an interface **“iBase”** which has **“say” **method    (b) We changed the **“check” **method to accept the argument of type iBase

Since base struct implements the **“say”** method and in turn, child struct embeds base. So the child method indirectly implements **“say”** method and becomes a type of **“iBase”** and that is why we can pass the child to the check function now. Great that we have fixed one limitation now using a combination of struct and interface. 
But there is one more limitation. Let’s say child and base both have one more function **“clear” . **Now **“say”** method makes a call to **“clear”** method. Then when **“say”** method is called using child struct, in turn, **“say”** method will call **“clear”** method of base and not **“clear”** method of the child. See below example

**Program 4**

```
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

**Output:**

```
Clear from base's function
```

As you can see above **“clear”** function of the base is called instead of**“clear”** method of the child. This is unlike Java where**“clear”** method of **“child”** would have been called.

One way to fix the above problem is to make **“clear”** as a property which is of type function in the base struct. This is possible in GO as functions are first-class variables in GO. See below solution

**Program 5**

```
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

**Output:**

```
Clear from child's function
```

Let’s try to add one more feature to the above program which is –

*   Multiple Inheritance – child struct should be able to access multiple properties and methods from two base struct and also sub-typing should be possible. Here is the code

**Program 6**

```
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

**Output:**

```
Hi from say function
Hi from walk function
Hi from say function
Hi from walk function
```

In the above program, the child embeds both base1 and base2\. It can also be passed as an instance of iBase1 and iBase2 interface to the check1 and check2 function respectively. This is how we achieve multiple inheritance.

Now a big question is how do we implement **“Type Hierarchy”** in GO. As already mentioned type inheritance is not allowed to go and hence it does not have type hierarchy.GO intentionally doesn’t allow this feature** so** any change in the behavior of an interface is only propagated to its immediate structures which defines all methods of the interface. 

Although we can implement type hierarchy using interfaces and struct like below

**Program 7**

```
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

See in above program how we are able to create a hierarchy (see below). This is the idiomatic way of go to create type hierarchy and we are able to achieve this by using embedding both on struct level and on the interface level. The point to be noted here is that if you want distinction in your type hierarchy where lets say a **“shark” ** should not be both **“iAquatic”** and **“iNonAquatic” ** , then there should be at least one method in the method sets of **“iAquatic”** and **“iNonAquatic”** which is not present in the other. In our example **“swim” ** and **“walk” ** are those methods.

```
iAnimal
--iAquatic
----shark
--iNonAquatic
----lion
```

**Conclusion:**

Go doesn’t have support for type inheritance but the same can be achieved using embedding buts one needs to be careful while creating such kind of type hierarchy. Also, go does not provide method overriding.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [inheritance](https://golangbyexample.com/tag/inheritance/)*   [oop](https://golangbyexample.com/tag/oop/)*   [oops](https://golangbyexample.com/tag/oops/)