<!--yml
category: 未分类
date: 2024-10-13 06:49:42
-->

# Two furthest houses with different colors in Go (Golang)

> 来源：[https://golangbyexample.com/two-furthest-houses-different-color-golang/](https://golangbyexample.com/two-furthest-houses-different-color-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An array is given which represents the color of houses. So array[i] represents the color of the house at index i. The objective is to find two furthest houses with different colors.

If no such indexes exist then return -1

Example 1

```
Input: intervals = [2,2,2,1,2,2]
Output: 3
Explanation: House at index 0 and house at index 3 is of different colors
```

Example 2

```
Input: intervals = [1, 2 ,3, 1, 2]
Output: 2
Explanation: House at index 0 and house at index 4 is of different colors
```

Below is the approach we can take.

*   One of the houses will be either at the 0th index or n-1 index where n is the length of the array
*   We can first assume that the house at the 0th index is in the solution and then compute
*   We can next assume that house at n-1 index is in the solution and then compute

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func maxDistance(colors []int) int {
	lenColors := len(colors)

	if lenColors == 0 || lenColors == 1 {
		return 0
	}
	maxDiff := 0

	leftColor := colors[0]
	rightColor := colors[lenColors-1]

	for i := 1; i < lenColors; i++ {
		if colors[i] != leftColor {
			maxDiff = i
		}
	}

	for i := lenColors - 2; i >= 0; i-- {
		if colors[i] != rightColor {
			diff := lenColors - i - 1
			if diff > maxDiff {
				maxDiff = diff
			}
		}
	}

	return maxDiff
}
func main() {
	output := maxDistance([]int{2, 2, 2, 1, 2, 2})
	fmt.Println(output)

	output = maxDistance([]int{1, 2, 3, 1, 2})
	fmt.Println(output)
}
```

**Output**

```
4
-1
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*