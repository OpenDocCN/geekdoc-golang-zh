<!--yml
category: 未分类
date: 2024-10-13 06:00:38
-->

# Inheritance in GO using interface

> 来源：[https://golangbyexample.com/inheritance-go-interface/](https://golangbyexample.com/inheritance-go-interface/)

This post describes inheritance using interface. Do visit our Inheritance in Go Complete Guide post for full reference

> [OOP: Inheritance in GOLANG complete guide](https://golangbyexample.com/oop-inheritance-golang-complete/)

[https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=lwzs553n8n#?secret=ZXUMNIsae2](https://golangbyexample.com/oop-inheritance-golang-complete/embed/#?secret=lwzs553n8n#?secret=ZXUMNIsae2)

Go supports inheritance by embedding struct or using interface. There are different ways of doing it and each having some limitations. The different ways are:

1.  By using embedded struct – The parent struct is embedded in child struct. The limitation is that subtyping is not possible with this approach. You cannot pass the child struct to a function that expects base. Refer this link for more details –**[Inheritance using struct](https://golangbyexample.com/inheritance-go-struct/)**
2.  **By using interfaces** – Subtyping is possible but the limitation is that one has no way to refer to common properties. Current post describes this approach
3.  By using interface + struct – This fixes the limitations of above two approach but one limitation is that overriding methods is not possible. But there is a workaround. Refer to this link for more details – **[Inheritance using interface + struct](https://golangbyexample.com/inheritance-go-interface-struct/)**

**Details:**

The child struct implements the methods of a common interface. This approach also solves the problem of subtyping. See below code

```
package main

import "fmt"

type iBase interface {
	say()
}

type child struct {
	style string
}

func (b *child) say() {
	fmt.Println(b.style)
}

func check(b iBase) {
	b.say()
}

func main() {
	child := &child{
		style: "somestyle",
	}
	child.say()
	check(child)
} 
```

**Output:**

```
somestyle
somestyle
```

**Limitation:**

This approach has a limitation that it is not possible to refer to common properties as an interface can not have any properties. This problem is solved by the mixed approach of using Struct + Interface.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [inheritance](https://golangbyexample.com/tag/inheritance/)*   [interface](https://golangbyexample.com/tag/interface/)*   [struct](https://golangbyexample.com/tag/struct/)