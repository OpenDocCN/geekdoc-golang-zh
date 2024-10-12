<!--yml
category: 未分类
date: 2024-10-13 06:00:45
-->

# Inheritance in GO using interface + struct

> 来源：[https://golangbyexample.com/inheritance-go-interface-struct/](https://golangbyexample.com/inheritance-go-interface-struct/)

This post describes inheritance using interface and struct. Do visit our Inheritance in Go Complete Guide post for full reference

> [OOP: Inheritance in GOLANG complete guide](https://golangbyexample.com/oop-inheritance-golang-complete/)

[https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=EpVuUFYEMN#?secret=OtvYyDmPmR](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=EpVuUFYEMN#?secret=OtvYyDmPmR)

Go supports inheritance by embedding struct or using interface. There are different ways of doing it and each having some limitations. The different ways are:

1.  By using embedded struct – The parent struct is embedded in child struct. The limitation is that subtyping is not possible with this approach. You cannot pass the child struct to a function that expects base. Refer this link for more details – [**Inheritance using Struct**](https://golangbyexample.com/inheritance-go-struct/)
2.  By using interfaces – Subtyping is possible but the limitation is that one has no way to refer to common properties. Refer this link for more details – [**Inheritance using Interface**](https://golangbyexample.com/inheritance-go-interface/)
3.  **By using interface + struct** – This fixes the limitations of above two approach but one limitation is that overriding methods is not possible. But there is workaround. Current post describes this approach

**Details:**

In this approach, the base struct is embedded in child struct and base struct implements all methods of the common interface. So child struct can:

1.  Access methods and properties of base struct
2.  Since base struct implements all functions of the common interface, the common interface itself can be used for subtyping.

```
package main

import "fmt"

type iBase interface {
	say()
}

type base struct {
	value string
}

func (b *base) say() {
	fmt.Println(b.value)
}

type child struct {
	base  //embedding
	style string
}

func check(b iBase) {
	b.say()
}

func main() {
	base := base{value: "somevalue"}
	child := &child{
		base:  base,
		style: "somestyle",
	}
	child.say()
	check(child)
} 
```

**Output:**

```
somevalue
somevalue 
```