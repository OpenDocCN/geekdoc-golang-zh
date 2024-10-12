<!--yml
category: 未分类
date: 2024-10-13 06:39:45
-->

# Slice of Struct in Go (Golang)

> 来源：[https://golangbyexample.com/slice-struct-golang/](https://golangbyexample.com/slice-struct-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

It is possible to create a slice of struct in Golang as well. In fact, a slice can be created of any data type in Go. Below is a simple example to create a slice of structs

## **Program**

```
package main

import "fmt"

type employee struct {
	name string
	age  int
}

func main() {
	employees := make([]employee, 3)

	employees[0] = employee{name: "John", age: 21}
	employees[1] = employee{name: "Simon", age: 25}
	employees[2] = employee{name: "David", age: 18}

	for _, e := range employees {
		fmt.Println(e)
	}
}
```

**Output**

```
{John 21}
{Simon 25}
{David 18}
```

In the above program, we created a struct named **employee**

```
type employee struct {
	name string
	age  int
}
```

Then we created a slice of struct like this

```
employees := make([]employee, 3)
```

This is how we can create a slice of struct

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*