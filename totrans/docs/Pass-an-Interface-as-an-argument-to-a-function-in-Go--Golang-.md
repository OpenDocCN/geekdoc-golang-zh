<!--yml
category: 未分类
date: 2024-10-13 06:21:40
-->

# Pass an Interface as an argument to a function in Go (Golang)

> 来源：[https://golangbyexample.com/pass-interface-as-argument-function-go/](https://golangbyexample.com/pass-interface-as-argument-function-go/)

A function can accept an argument of an interface type. That interface type can be either

*   Regular Interface

*   Empty Interface

Let’s see example for both of them one by one

Table of Contents

 **   [Regular Interface](#Regular_Interface "Regular Interface")
*   [Empty Interface](#Empty_Interface "Empty Interface")*  *# **Regular Interface**

A function can accept an argument of an interface type. Any type which implements that interface can be passed as an argument to that function.  Let’s understand with an example. Let’s say we have an animal interface as below

```
type animal interface {
    breathe()
    walk()
}
```

We have two struct types implementing that interface

```
type lion struct {
    age int
}
```

and

```
type dog struct {
     age int
}
```

In below code we have **callBreathe** and **callWalk** function which accept an argument of **animal** interface type. Both **lion** and **dog** instance can be passed to this function. We create an instance of both **lion** and **dog** type and pass it to the function. During compilation no type is checked while calling the function,  instead it is enough to check that the type passed to the function does implement **breathe** and **walk** method.

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
	l := lion{age: 10}
	callBreathe(l)
	callWalk(l)

	d := dog{age: 5}
	callBreathe(d)
	callWalk(d)
}

func callBreathe(a animal) {
	a.breathe()
}

func callWalk(a animal) {
	a.breathe()
}
```

**Output**

```
Lion breathes
Lion walk
Dog breathes
Dog walk
```

# **Empty Interface**

An empty interface has no methods , hence by default all types implement the empty interface. If you write a function that accepts an empty interface then you can pass any type to that function. See working code below

```
package main

import "fmt"

func main() {
    test("thisisstring")
    test("10")
    test(true)
}

func test(a interface{}) {
    fmt.Printf("(%v, %T)\n", a, a)
}
```

**Output:**

```
(thisisstring, string)
(10, string)
(true, bool)
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*