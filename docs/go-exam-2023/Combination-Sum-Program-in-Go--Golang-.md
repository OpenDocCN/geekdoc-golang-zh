<!--yml

category: 未分类

date: 2024-10-13 06:47:27

-->

# Go（Golang）中的组合求和程序

> 来源：[`golangbyexample.com/combination-sum-program-go/`](https://golangbyexample.com/combination-sum-program-go/)

目录

**   概述

+   程序*  *## **概述**

给定一个整数数组和一个目标数字，目标是找到数组中组合的数量，使得它们的和等于目标数字。

数组中的每个元素可以在同一组合中使用任意次数。

示例

```go
Input: [3,4,10,11]
Target: 10
Output: [[3,3,4],[10]]
```

## **程序**

这里是相同程序的代码。

```go
package main

import "fmt"

func combinationSum(candidates []int, target int) [][]int {
	lengthCandidates := len(candidates)
	current_sum_array := make([]int, 0)
	output := make([][]int, 0)
	combinationSumUtil(candidates, lengthCandidates, 0, 0, 0, target, current_sum_array, &output)
	return output
}

func combinationSumUtil(candidates []int, lengthCandidates, index, current_sum_index, current_sum, target int, current_sum_array []int, output *[][]int) {

	if index >= lengthCandidates {
		return
	}

	if current_sum > target {
		return
	}

	if current_sum == target {
		var o []int
		for i := 0; i < current_sum_index; i++ {
			o = append(o, current_sum_array[i])
		}
		*output = append(*output, o)
		return
	}

	//Exclude
	combinationSumUtil(candidates, lengthCandidates, index+1, current_sum_index, current_sum, target, current_sum_array, output)

	//Include
	current_sum_array = append(current_sum_array, candidates[index])

	combinationSumUtil(candidates, lengthCandidates, index, current_sum_index+1, current_sum+candidates[index], target, current_sum_array, output)
}

func main() {
	output := combinationSum([]int{3, 4, 10, 11}, 10)
	fmt.Println(output)
}
```

**输出**

```go
[[10] [3 3 4]]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们努力用示例覆盖所有概念。这个教程适合那些希望获得专业知识和扎实理解 golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
