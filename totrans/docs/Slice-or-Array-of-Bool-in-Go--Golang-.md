<!--yml
category: 未分类
date: 2024-10-13 06:40:01
-->

# Slice or Array of Bool in Go (Golang)

> 来源：[https://golangbyexample.com/slice-array-bool-golang/](https://golangbyexample.com/slice-array-bool-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Slice of Bool](#Slice_of_Bool "Slice of Bool")
*   [Array of Bool](#Array_of_Bool "Array of Bool")*  *## **Overview**

It is possible to create a slice or array of **bool** data type in Golang as well. In fact, a slice or array can be created of any data type in Go. This tutorial contains simple examples to create a slice or array of bool data type in golang.

Just to add here that in golang array is of fixed size and slice can have variable size. More details here

Array – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

Slice – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **Slice of Bool**

```
package main

import "fmt"

func main() {

	//First Way
	var booleans_first []bool
	booleans_first = append(booleans_first, true)
	booleans_first = append(booleans_first, false)
	booleans_first = append(booleans_first, true)

	fmt.Println("Output for First slice of booleans")
	for _, c := range booleans_first {
		fmt.Println(c)
	}

	//Second Way
	booleans_second := make([]bool, 3)
	booleans_second[0] = false
	booleans_second[1] = true
	booleans_second[2] = false

	fmt.Println("\nOutput for Second slice of booleans")
	for _, c := range booleans_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First slice of booleans
true
false
true

Output for Second slice of booleans
false
true
false
```

We have two ways of creating a slice of bool. The first way is

```
var booleans_first []bool
booleans_first = append(booleans_first, true)
booleans_first = append(booleans_first, false)
booleans_first = append(booleans_first, true)
```

In the second way, we use make command to create a slice of booleans

```
booleans_second := make([]bool, 3)
booleans_second[0] = false
booleans_second[1] = true
booleans_second[2] = false
```

Either way works. This is how we can create a slice of booleans

## **Array of Bool**

```
package main

import "fmt"

func main() {

	var booleans_first [3]bool

	booleans_first[0] = true
	booleans_first[1] = false
	booleans_first[2] = true

	fmt.Println("Output for First Array of booleans")
	for _, c := range booleans_first {
		fmt.Println(c)
	}

	booleans_second := [3]bool{
		false,
		true,
		false,
	}

	fmt.Println("\nOutput for Second Array of booleans")
	for _, c := range booleans_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First Array of booleans
true
false
true

Output for Second Array of booleans
false
true
false
```

We have two ways of creating an array. The first way is

```
var booleans_first [3]bool
booleans_first[0] = true
booleans_first[1] = false
booleans_first[2] = true
```

In second way we directly initialize the array with created booleans

```
booleans_second := [3]bool{
	false,
	true,
	false,
}
```

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*