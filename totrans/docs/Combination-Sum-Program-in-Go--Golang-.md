<!--yml
category: 未分类
date: 2024-10-13 06:47:27
-->

# Combination Sum Program in Go (Golang)

> 来源：[https://golangbyexample.com/combination-sum-program-go/](https://golangbyexample.com/combination-sum-program-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given an integer array and a target number, the objective is to find the number of combinations from the integer array that sum to the target number.

Each of the elements in the array could be used any number of times in the same combination

Examples

```
Input: [3,4,10,11]
Target: 10
Output: [[3,3,4],[10]]
```

## **Program**

Here is the program for the same.

```
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

**Output**

```
[[10] [3 3 4]]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*