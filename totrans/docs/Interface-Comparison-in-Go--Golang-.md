<!--yml
category: 未分类
date: 2024-10-13 06:22:11
-->

# Interface Comparison in Go (Golang)

> 来源：[https://golangbyexample.com/interface-comparison-golang/](https://golangbyexample.com/interface-comparison-golang/)

For understanding whether two interface variables are equal or not, we first need to understand the internal representation of an interface. Like any other variable, an interface variable is represented by a type and value. Interface variable value, in turn under the hood, consists of two tuple

*   Underlying Type
*   Underlying Value

See below diagram which illustrates what we mentioned above

![](img/e09464771d53171817cfeecf9da6a93e.png)

Two interface are comparable if either

*   Interface value is nil or
*   The underlying type is the same and comparable. Underlying Value is also the same

Some of the comparable types as defined by go specification are

*   boolean
*   numeric
*   string,
*   pointer
*   channel
*   interface types
*   structs – if all it’s field type is comparable
*   array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification

*   Slice
*   Map
*   Function

Two interface variable can be compared using **==** or **!=** operators

Let’s see a program

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
	var b animal
	var c animal
	var d animal
	var e animal

	a = lion{age: 10}
	b = lion{age: 10}
	c = lion{age: 5}

	if a == b {
		fmt.Println("a and b are equal")
	} else {
		fmt.Println("a and b are not equal")
	}

	if a == c {
		fmt.Println("a and c are equal")
	} else {
		fmt.Println("a and c are not equal")
	}

	if d == e {
		fmt.Println("d and e are equal")
	} else {
		fmt.Println("d and e are not equal")
	}
}
```

**Output**

```
a and b are equal
a and c are not equal
d and e are equal
```

In the above program, we have animal interface and we have a lion struct that implements the animal interface by defining two of its methods.

Interface variable **a** and **b** are equal because

*   Underlying type is same i.e  **lion** struct
*   Underlying type is comparable i.e **lion** struct has only one field of **int** type and **int** type is comparable
*   Underlying value is same i.e **lion.age** is 10 for both

First two points also apply for comparing a and c, but they are not equal because

*   Underlying value is not same i.e **lion.age** is 10 for **a** while it is 5 for **c**

**Interface variable d and e are equal because**

 ***   Interface value is nil for both

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)**