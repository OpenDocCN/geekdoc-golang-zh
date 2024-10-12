<!--yml
category: 未分类
date: 2024-10-13 06:22:06
-->

# Embedding Interfaces in Go (Golang)

> 来源：[https://golangbyexample.com/embedding-interfaces-go/](https://golangbyexample.com/embedding-interfaces-go/)

An interface can be embedded in another interface as well as it can be embedded in a struct. Let’s look at each one by one

Table of Contents

 **   [Embedding interface in another interface](#Embedding_interface_in_another_interface "Embedding interface in another interface")
*   [Embedding interface in a struct](#Embedding_interface_in_a_struct "Embedding interface in a struct")*  *# **Embedding interface in another interfa**ce

An interface can embed any number of interfaces in it as well as it can be embedded in any interface. All the methods of the embedded interface become part of the embedding interface. It is a way of creating a new interface by merging some small interfaces. Let’s understand it with an example

Assume we have an interface **animal** as below

```
type animal interface {
    breathe()
    walk()
}
```

Let’s say there is another interface named **human** which embeds the **animal** interface

```
type human interface {
    animal
    speak()
}
```

So if any type needs to implement the **human** interface, then it has to define

*   **breathe()** and **walk()** method of animal interfaces animal is  embedded in human

*   **speak()** method of human interface

```
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

**Output**

```
Employee breathes
Employee walk
Employee speaks
```

As another example, the ReaderWriter interface of the **io** package of golang ([https://golang.org/pkg/io/#ReadWriter](https://golang.org/pkg/io/#ReadWriter)) embeds two other interfaces

*   The reader interface  – [https://golang.org/pkg/io/#Reader](https://golang.org/pkg/io/#Reader)

*   The writer interface   – [https://golang.org/pkg/io/#Writer](https://golang.org/pkg/io/#Writer)

```
type ReadWriter interface {
    Reader
    Writer
}
```

# **Embedding interface in a struct**

An interface can be embedded in a struct as well.  All the methods of the embedded interface can be called via that struct. How these methods will be called will depend upon whether the embedded interface is a named field or an unnamed/anonymous field. 

*   If the embedded interface is a named field, then interface methods have to be called via the named interface name

*   If the embedded interface is unnamed/anonymous field then interface methods can be referred directly or via the interface name

Let’s see a program illustrating above points

```
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

**Output**

```
Milo
Dog breathes
Dod walk

Oscar
Dog breathes
Dog walk
Dog breathes
Dog walk
```

We declared two struct **pet1** and **pet2**.  **pet1** struct has named **animal** interface in it

```
type pet1 struct {
    a    animal
    name string
}
```

**pet2** has unnamed/anonymous **animal** interface embedded

```
type pet2 struct {
    animal
    name string
}
```

For an instance of **pet1** struct we call the **breathe()** and **walk()** method like this

```
p1.a.breathe()
p1.a.walk()
```

Directly calling these methods with raise compilation error

```
//p1.breathe()
//p1.walk()
```

```
p1.breathe undefined (type pet1 has no field or method breathe)
p1.walk undefined (type pet1 has no field or method walk)
```

For an instance of **pet2** struct we can call the **breathe()** and **walk()** method like directly

```
p2.breathe()
p2.walk()
```

We can directly access the methods of embedded interface if the embedded interface is anonymous or unnamed.

Below is also valid and another way fo called methods of unnamed/anonymous embedded interface

```
p2.animal.breathe()
p2.animal.walk()
```

Also note that while creating the instance of either the **pet1** or **pet2** struct, the embedded interface i.e **animal** is initialised with a type implementing  it i.e **dog** .

```
p1 := pet1{name: "Milo", a: d}
p2 := pet2{name: "Oscar", animal: d}
```

If we don’t initialise the embedded interface **animal**, then it will be intialised with the zero value of the interface which is nil. Calling **breathe()** and **walk()** method  on such an instance of pet1 or pet2 struct will create a panic.

*   [go](https://golangbyexample.com/tag/go/)*