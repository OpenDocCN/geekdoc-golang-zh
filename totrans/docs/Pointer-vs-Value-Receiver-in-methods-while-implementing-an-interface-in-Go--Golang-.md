<!--yml
category: 未分类
date: 2024-10-13 06:22:21
-->

# Pointer vs Value Receiver in methods while implementing an interface in Go (Golang)

> 来源：[https://golangbyexample.com/pointer-vs-value-receiver-method-golang/](https://golangbyexample.com/pointer-vs-value-receiver-method-golang/)

A method of a type can either have a pointer receiver or a value receiver. There is a caveat while pointer vs value receiver for methods of a type when that type implements an interface

*   If a type implements all methods of an interface using value receiver, then both value and pointer of that type can be used while assigning to that interface variable or while passing to a function which accept an argument as that interface.

*   If a type implements all methods of an interface using pointer receiver, then the only pointer of that type can be used while assigning to that interface variable or while passing to a function that accepts an argument as that interface.

Example to demonstrate the first point above

Assume we have an interface **animal** as below

```
type animal interface {
    breathe()
    walk()
}
```

We also have a **lion** struct implementing this **animal** interface

```
type lion struct {
    age int
}
```

**Code**

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

    a = &lion{age: 5}
    a.breathe()
    a.walk()
}
```

**Output**

```
Lion breathes
Lion walk
Lion breathes
Lion walk
```

The **lion** structs implement the **animal** interface using value receiver. Hence it works for both variables of type **lion** as well as a pointer to variable of type **lion**.

This works

```
 a = lion{age: 10}
```

as well as this

```
a = &lion{age: 5}
```

Example to demonstrate the second point above. The **lion** structs implements the **animal** interface using a pointer receiver. Hence it works only for the pointer to a variable of type **lion**.

So this works

```
a = &lion{age: 5}
```

but this raises compilation error

```
a = lion{age: 10}
```

```
cannot use lion literal (type lion) as type animal in assignment:
        lion does not implement animal (breathe method has pointer receiver)
```

See full working code

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

func (l *lion) breathe() {
	fmt.Println("Lion breathes")
}

func (l *lion) walk() {
	fmt.Println("Lion walk")
}

func main() {
	var a animal

	//a = lion{age: 10}
	a.breathe()
	a.walk()

	a = &lion{age: 5}
	a.breathe()
	a.walk()
}
```

Uncomment the line

```
a = lion{age: 10}
```

and it will raise compilation error

```
cannot use lion literal (type lion) as type animal in assignment:
        lion does not implement animal (breathe method has pointer receiver)
```

*   [go](https://golangbyexample.com/tag/go/)