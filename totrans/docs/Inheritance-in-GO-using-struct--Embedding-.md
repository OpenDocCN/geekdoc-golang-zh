<!--yml
category: 未分类
date: 2024-10-13 06:00:33
-->

# Inheritance in GO using struct (Embedding)

> 来源：[https://golangbyexample.com/inheritance-go-struct/](https://golangbyexample.com/inheritance-go-struct/)

This post desicribes inheritance using struct only. Do visit our Inheritance in Go Complete Guide post for full reference

> [OOP: Inheritance in GOLANG complete guide](https://golangbyexample.com/oop-inheritance-golang-complete/)

[https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=z8WWaJ1OiI#?secret=zH9oZkEbS3](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=z8WWaJ1OiI#?secret=zH9oZkEbS3)

Go supports inheritance by embedding struct or using interface. There are different ways of doing it and each having some limitations. The different ways are:

1.  **By using embedded struct** – The parent struct is embedded in child struct. The limitation is that subtyping is not possible with this approach. You cannot pass the child struct to a function that expects base. The current post describes this approach.
2.  By using interfaces – Subtyping is possible but the limitation is that one has no way to refer to common properties. Refer this link for more details – [**Inheritance using Interface**](https://golangbyexample.com/inheritance-go-interface/)
3.  By using interface + struct – This fixes the limitations of above two approach but one limitation is that overriding methods is not possible. But there is a workaround. Refer to this link for more details – [**Inheritance using interface + struct**](https://golangbyexample.com/inheritance-go-interface-struct/)

**Details:**

In inheritance using a struct, a base struct is embedded in child struct and base properties and methods can directly be called on child struct. See below code:

```
package main

import "fmt"

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

func check(b base) {
	b.say()
}

func main() {
	base := base{value: "somevalue"}
	child := &child{
		base:  base,
		style: "somestyle",
	}
	child.say()
	//check(child)
} 
```

**Output:**

```
somevalue
```

**Limitation:**

Subtyping is not supported. You cannot pass the child struct to a function that expects base.

For example in the above code if you uncomment **//check(child)** it will give compilation error: “cannot use child (type *child) as type base in argument to check”. To fix this we can do inheritance using Interface

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [inheritance](https://golangbyexample.com/tag/inheritance/)*   [interface](https://golangbyexample.com/tag/interface/)*   [struct](https://golangbyexample.com/tag/struct/)