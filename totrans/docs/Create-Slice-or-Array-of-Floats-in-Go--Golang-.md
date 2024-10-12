<!--yml
category: 未分类
date: 2024-10-13 06:40:11
-->

# Create Slice or Array of Floats in Go (Golang)

> 来源：[https://golangbyexample.com/slice-array-floats-golang/](https://golangbyexample.com/slice-array-floats-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Slice of Float](#Slice_of_Float "Slice of Float")
*   [Array of Floats](#Array_of_Floats "Array of Floats")*  *## **Overview**

It is possible to create a slice or array of **float** data type in Golang as well. In fact, a slice or array can be created of any data type in Go. This tutorial contains simple examples to create a slice or array of float data type in golang.

Just to add here that in golang array is of fixed size and slice can have variable size. More details here

Array – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

Slice – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **Slice of Float**

```
package main

import "fmt"

func main() {

	//First Way
	var floats_first []float64
	floats_first = append(floats_first, 1.1)
	floats_first = append(floats_first, 2.2)
	floats_first = append(floats_first, 3.3)

	fmt.Println("Output for First slice of floats")
	for _, c := range floats_first {
		fmt.Println(c)
	}

	//Second Way
	floats_second := make([]float64, 3)
	floats_second[0] = 3.3
	floats_second[1] = 2.2
	floats_second[2] = 1.1

	fmt.Println("\nOutput for Second slice of floats")
	for _, c := range floats_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First slice of floats
1.1
2.2
3.3

Output for Second slice of floats
3.3
2.2
1.1
```

We have two ways of creating a slice of float. The first way is

```
var floats_first []float64
floats_first = append(floats_first, 1.1)
floats_first = append(floats_first, 2.2)
floats_first = append(floats_first, 3.3)
```

In the second way, we use make command to create a slice of floats

```
floats_second := make([]float64, 3)
floats_second[0] = 3.3
floats_second[1] = 2.2
floats_second[2] = 1.1
```

## **Array of Floats**

```
package main

import "fmt"

func main() {

	var floats_first [3]float64

	floats_first[0] = 1.1
	floats_first[1] = 2.2
	floats_first[2] = 3.3

	fmt.Println("Output for First Array of floats")
	for _, c := range floats_first {
		fmt.Println(c)
	}

	floats_second := [3]float64{
		3.3,
		2.2,
		1.1,
	}

	fmt.Println("\nOutput for Second Array of floats")
	for _, c := range floats_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First Array of floats
1.1
2.2
3.3

Output for Second Array of floats
3.3
2.2
1.1
```

We have two ways of creating an array. The first way is

```
var floats_first [3]float64
floats_first[0] = 1.1
floats_first[1] = 2.2
floats_first[2] = 3.3
```

In second way we directly initialize the array with created floats

```
floats_second := [3]float64{
	3.3,
	2.2,
	1.1,
}
```

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*