<!--yml

类别：未分类

日期：2024-10-13 06:42:34

-->

# 找到数组中所有和为目标数字的三元组，使用 Go (Golang)

> 来源：[`golangbyexample.com/array-triplets-target-sum-golang/`](https://golangbyexample.com/array-triplets-target-sum-golang/)

目录

**   概述

+   程序*  *## **概述**

假设输入是

```go
[1, 2, 3, 4, 1]
```

那么答案就是

```go
[[1 2 3] [1 4 1] [2 3 1]]
```

我们可以使用哈希来解决这个问题。它基于以下思想

+   如果两个数字是 **x** 和 **y**，而目标和是 **target**

+   那么另一个数字将是 **target**–**(x+y)**

## **程序**

```go
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

**输出**

```go
[[1 2 3] [1 4 1] [2 3 1]]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
