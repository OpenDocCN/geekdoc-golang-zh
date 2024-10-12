<!--yml
category: 未分类
date: 2024-10-13 06:40:06
-->

# Create Slice or Array of Integers in Go (Golang)

> 来源：[https://golangbyexample.com/slice-array-integers-golang/](https://golangbyexample.com/slice-array-integers-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Slice of Int](#Slice_of_Int "Slice of Int")
*   [Array of Integers](#Array_of_Integers "Array of Integers")*  *## **Overview**

It is possible to create a slice or array of **int** data type in Golang as well. In fact, a slice or array can be created of any data type in Go. This tutorial contains simple examples to create a slice or array of int data type in golang.

Just to add here that in golang array is of fixed size and slice can have variable size. More details here

Array – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

Slice – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **Slice of Int**

```
package main

import "fmt"

func main() {

	//First Way
	var integers_first []int
	integers_first = append(integers_first, 1)
	integers_first = append(integers_first, 2)
	integers_first = append(integers_first, 3)

	fmt.Println("Output for First slice of integers")
	for _, c := range integers_first {
		fmt.Println(c)
	}

	//Second Way
	integers_second := make([]int, 3)
	integers_second[0] = 3
	integers_second[1] = 2
	integers_second[2] = 1

	fmt.Println("\nOutput for Second slice of integers")
	for _, c := range integers_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First slice of integers
1
2
3

Output for Second slice of integers
3
2
1
```

We have two ways of creating a slice of integer. The first way is

```
var integers_first []int
integers_first = append(integers_first, 1)
integers_first = append(integers_first, 2)
integers_first = append(integers_first, 3)
```

In the second way, we use make command to create a slice of integers

```
integers_second := make([]int, 3)
integers_second[0] = 3
integers_second[1] = 2
integers_second[2] = 1
```

Either way works. This is how we can create a slice of integers

## **Array of Integers**

```
package main

import "fmt"

func main() {

	var integers_first [3]int

	integers_first[0] = 1
	integers_first[1] = 2
	integers_first[2] = 3

	fmt.Println("Output for First Array of integers")
	for _, c := range integers_first {
		fmt.Println(c)
	}

	integers_second := [3]int{
		3,
		2,
		1,
	}

	fmt.Println("\nOutput for Second Array of integers")
	for _, c := range integers_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First Array of integers
1
2
3

Output for Second Array of integers
3
2
1
```

We have two ways of creating an array. The first way is

```
var integers_first [3]int
integers_first[0] = 1
integers_first[1] = 2
integers_first[2] = 3
```

In second way we directly initialize the array with created integers

```
integers_second := [3]int{
	3,
	2,
	1,
}
```

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*