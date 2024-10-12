<!--yml
category: 未分类
date: 2024-10-13 06:52:20
-->

# Sort a slice of Int in Ascending and Descending order in Go (Golang)

> 来源：[https://golangbyexample.com/sort-slice-asc-desc-golang/](https://golangbyexample.com/sort-slice-asc-desc-golang/)

Table of Contents

 **   [Sort a slice in Ascending order](#Sort_a_slice_in_Ascending_order "Sort a slice in Ascending order")
*   [Sort a slice in Descending order](#Sort_a_slice_in_Descending_order "Sort a slice in Descending order")*  *# **Sort a slice in Ascending order**

**sort.Ints** package of going can be used to sort a full slice or a part of the slice. It is going to sort the string into Ascending order.

Below is the signature of the method

[https://pkg.go.dev/sort#Ints](https://pkg.go.dev/sort#Ints)

```
func Ints(x []int)
```

It takes a slice ‘x’ as an argument and sorts the slice ‘x’ in place.

Below is the program for the same

```
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Ints(nums)
	fmt.Printf("\nAfter: %v", nums)
}
```

**Output**

```
Before: [3 4 2 1]
After: [1 2 3 4]
```

# **Sort a slice in Descending order**

To sort a slice in descending order we will use **Slice** method of **sort** pacakge

[https://pkg.go.dev/sort#Slice](https://pkg.go.dev/sort#Slice)

Below is the signature of the method

```
func Slice(x any, less func(i, j int) bool)
```

So this function takes two arguments

*   **x any** – any is nothing here but an empty interface [https://pkg.go.dev/builtin#any](https://pkg.go.dev/builtin#any)

*   **less func(i, j int)** – This function is nothing but a comparator function

We can use this comparator function to decide the descending order of the elements in the slice. Below is an example

```
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] > nums[j]
	})
	fmt.Printf("\nAfter: %v", nums)
}
```

**Output**

```
Before: [3 4 2 1]
After: [4 3 2 1]
```

As a matter of fact, you can use **sort.Slice** method to also sort a slice in descending order. Below is an example

```
package main

import (
	"fmt"
	"sort"
)

func main() {

	nums := []int{3, 4, 2, 1}
	fmt.Printf("Before: %v", nums)
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})
	fmt.Printf("\nAfter: %v", nums)
}
```

**Output**

```
Before: [3 4 2 1]
After: [1 2 3 4]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -

[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

**Note:** Check out our system design tutorial series [System Design Questions](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*