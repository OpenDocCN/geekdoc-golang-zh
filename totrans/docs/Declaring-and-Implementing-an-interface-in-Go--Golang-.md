<!--yml
category: 未分类
date: 2024-10-13 06:22:26
-->

# Declaring and Implementing an interface in Go (Golang)

> 来源：[https://golangbyexample.com/declaring-implementing-interface-golang/](https://golangbyexample.com/declaring-implementing-interface-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Declaring an Interface](#Declaring_an_Interface "Declaring an Interface")
*   [Implementing an interface](#Implementing_an_interface "Implementing an interface")*  *# **Overview**

Interface is a type in Go which is a collection of method signatures. These collection of method signatures are meant to represent a behaviour. The interface declares only the method set and any type which implements all methods of the interface is of that interface type.

# **Declaring an Interface**

Let’s define an interface named **animal.** The **animal** interface has two methods **breathe** and **walk**. It defines only the method signatures and nothing else.

```
type animal interface {
    breathe()
    walk()
}
```

A method signature would include

*   Name of the method

*   Number of arguments and type of each argument

*   Number of return values and type of each return value

With above declaration we created a new interface type i.e **animal .**

It is ok to define a variable of **animal** type. Let’s create a variable of **animal** interface type.

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

**Output**

```
 <nil>Underlying Type: <nil>Underlying Value:</nil></nil> 
```

As seen in above program it is ok to create a variable of interface type. It prints nil as default zero value of an interface is nil.

# **Implementing an interface**

Any type which implements the breathe and walk method then it is said to implement the **animal** interface. . So if we define a **lion** struct and implements the breathe and walk method then it will implement the  animal interface.

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

We declare a variable of animal interface type

```
var a animal
```

Then we assign an instance of lion struct to it.

```
a = lion{}
```

Assigning an instance of **lion** struct to a variable of **animal** interface type works because **lion** struct implements both **breathe** and **walk** method of **animal**. The type is not checked during this assignment, instead it is enough to check that the type assigned does implement **breathe** and **walk** method.  The concept is similar to duck typing,  a **lion** can **breathe** and **walk** like an **animal** and hence it is an **animal**.

There is no explicit declaration that a type implements an interface. In fact, in Go there doesn’t exist any **“implements”** keyword similar to Java.  A type implements an interface if it implements all the methods of the interface.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*