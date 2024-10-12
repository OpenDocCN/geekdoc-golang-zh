<!--yml
category: 未分类
date: 2024-10-13 06:42:39
-->

# Find the sum which is closest to a target number using three numbers in an array or 3Sum closest problem in Go (Golang)

> 来源：[https://golangbyexample.com/three-sum-closest-golang/](https://golangbyexample.com/three-sum-closest-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find the sum using three triplets in the given array such that the sum is closest to a given target sum.

For

```
Input Array: [0,2,3,-1] Target Sum:6
Output: 5 . It can be formed using 0+2+3 
```

## **Program**

Below is the program for the same

```
package main

import (
	"fmt"
	"math"
	"sort"
)

func main() {
	output := threeSumClosest([]int{0, 2, 3, -1}, 6)
	fmt.Println(output)
}

func threeSumClosest(nums []int, target int) int {

	numsLength := len(nums)

	closestSum := 0
	nearest := 10000

	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})

	for k := 0; k < numsLength-2; k++ {
		i := k + 1
		j := numsLength - 1
		for i < j {
			sum := nums[k] + nums[i] + nums[j]
			absSum := int(math.Abs(float64(target - sum)))

			if absSum < nearest {
				nearest = absSum
				closestSum = sum
			}

			if nums[k]+nums[i]+nums[j] > target {
				j--
			} else {
				i++
			}
		}
	}

	return closestSum
}
```

**Output**

```
5
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*