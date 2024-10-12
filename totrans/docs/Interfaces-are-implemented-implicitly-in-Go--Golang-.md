<!--yml
category: 未分类
date: 2024-10-13 06:22:31
-->

# Interfaces are implemented implicitly in Go (Golang)

> 来源：[https://golangbyexample.com/interface-implit-implementation-golanng/](https://golangbyexample.com/interface-implit-implementation-golanng/)

There is no explicit declaration that a type implements an interface. Infact, in Go there doesn’t exist any **“implements”** keyword similar to Java.  A type implements an interface if it implements all the methods of the interface.

Let’s see an example

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

There is no explicit declaration which says that **lion** struct implements the **animal** interface. During compilation, go notices that **lion** struct implements all methods of **animal** interface hence it is allowed.Any other type which implements all methods of the **animal** interface becomes of that interface type.

This holds true even when the interface and the type which defines all methods of an interface are in different packages

Let’s see a more complex example of another type implementing the animal interfaceIf we define a **dog** struct and it implements the **breathe** and **walk** method then it will also be an animal. Let’s see an example

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

**Output**

```
Lion breathes
Lion walk
Dog breathes
Dog walk
```

Both **lion** and **dog** implement the **breathe** and **walk** method hence they are of **animal** type and can correctly be assigned to a variable of interface type.