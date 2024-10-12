<!--yml
category: 未分类
date: 2024-10-13 06:34:22
-->

# How to initialize a struct having an array or slice field in Go (Golang)

> 来源：[https://golangbyexample.com/struct-slice-field-go/](https://golangbyexample.com/struct-slice-field-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

A struct can have a field which is a slice or array of another type. To initialize such a struct we can initialize the slice/array of another type first. After that, we can initialize the parent struct. In the below example **class** struct type has a slice of the **student** struct type.

```
type student struct {
	name   string 
	rollNo int    
	city   string 
}

type class struct {
	className string
	students  []student
}
```

To initialize such kind of struct we need to initialize the slice of nested struct first. That is first we will initialize the slice of **student** struct. For eg as below

```
goerge := student{"Goerge", 35, "Newyork"}
john := student{"Goerge", 25, "London"}

students := []student{goerge, john}
```

Then we can initialize the **class** struct as below

```
class := class{"firstA", students}class := class{"firstA", students}
```

Another way is to directly initialize the slice of **student** struct during the initialization of **class** struct itself. Like as below

```
class := class{"firstA", []student{goerge, john}}
```

# **Program**

Let’s see a complete program for that.

```
package main

import "fmt"

type class struct {
	className string
	students  []student
}

type student struct {
	name   string
	rollNo int
	city   string
}

func main() {
	goerge := student{"Goerge", 35, "Newyork"}
	john := student{"Goerge", 25, "London"}

	students := []student{goerge, john}
	class := class{"firstA", students}

	fmt.Printf("class is %v\n", class)
}
```

**Output**

```
class is {firstA [{Goerge 35 Newyork} {Goerge 25 London}]}
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*