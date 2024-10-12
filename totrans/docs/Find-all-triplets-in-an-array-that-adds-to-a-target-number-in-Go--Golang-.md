<!--yml
category: 未分类
date: 2024-10-13 06:42:34
-->

# Find all triplets in an array that adds to a target number in Go (Golang)

> 来源：[https://golangbyexample.com/array-triplets-target-sum-golang/](https://golangbyexample.com/array-triplets-target-sum-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Let’s say the input is

```
[1, 2, 3, 4, 1]
```

Then the answer would be

```
[[1 2 3] [1 4 1] [2 3 1]]
```

We can use a hash for the solution. It is based upon the idea that

*   If two of the numbers are **x** and **y** and target sum is **target**

*   Then the other number will be **target**–**(x+y)**

## **Program**

```
package main

import (
	"fmt"
)

func main() {
	output := threeSumTarget([]int{1, 2, 3, 4, 1}, 6)
	fmt.Println(output)
}

func threeSumTarget(nums []int, target int) [][]int {

	numsLength := len(nums)
	var results [][]int
	for k := 0; k < numsLength-2; k++ {
		numsMap := make(map[int]int)
		for i := k + 1; i < numsLength; i++ {
			if numsMap[target-(nums[i]+nums[k])] > 0 {
				result := []int{nums[k], target - (nums[i] + nums[k]), nums[i]}
				results = append(results, result)
			}
			numsMap[nums[i]] = i
		}
	}

	return results
}
```

**Output**

```
[[1 2 3] [1 4 1] [2 3 1]]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*