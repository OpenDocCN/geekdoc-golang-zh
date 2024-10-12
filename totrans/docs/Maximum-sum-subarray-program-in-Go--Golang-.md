<!--yml
category: 未分类
date: 2024-10-13 06:46:06
-->

# Maximum sum subarray program in Go (Golang)

> 来源：[https://golangbyexample.com/maximum-sum-subarray-golang/](https://golangbyexample.com/maximum-sum-subarray-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the maximum subarray in a given input array. The subarray should be contiguous and should contain at least one element

For example

```
Input: [4, 5 ,-3]
Maximum Subarray is [4, 5]
Output: 9
```

Another example

```
Input: [1, 2, -4, 4, 1]
Maximum Subarray is [4, 1]
Output: 5
```

We use the Kadane algorithm here. In the Kadane algorithm, we keep two variables **max** and **currentMax**

*   **max** is initialized to IntMin and **currentMax** is initialized to zero

*   Then we Loop for each element in the array
    *   Set **currentMax** = **currentMax** + a[i]
    *   If **currentMax** > **max** then **max** is set to **currentMax**
    *   if **currentMax** is less than zero then **currentMax** is reset to zero

## **Program**

Here is the program for the same

```
package main

import "fmt"

const (
	UintSize = 32 << (^uint(0) >> 32 & 1)
	MaxInt   = 1<<(UintSize-1) - 1 // 1<<31 - 1 or 1<<63 - 1
	MinInt   = -MaxInt - 1
)

func main() {
	input := []int{4, 5, -3}
	output := maxSubArray(input)
	fmt.Println(output)

	input = []int{1, 2, -4, 4, 1}
	output = maxSubArray(input)
	fmt.Println(output)
}

func maxSubArray(nums []int) int {
	lenNums := len(nums)

	currentMax := 0
	max := MinInt
	for i := 0; i < lenNums; i++ {
		currentMax = currentMax + nums[i]
		if currentMax > max {
			max = currentMax
		}

		if currentMax < 0 {
			currentMax = 0
		}

	}
	return max
}
```

**Output**

```
9
5
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*