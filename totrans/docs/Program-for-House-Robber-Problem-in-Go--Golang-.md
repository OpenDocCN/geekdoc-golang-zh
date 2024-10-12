<!--yml
category: 未分类
date: 2024-10-13 06:49:29
-->

# Program for House Robber Problem in Go (Golang)

> 来源：[https://golangbyexample.com/house-robber-golang/](https://golangbyexample.com/house-robber-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

There are a couple of houses in the neighborhood. Each house has some money in it. The houses are represented as an array which each entry in the array denotes the amount of money in that house.

For example, if we have below array

```
[2, 3, 4, 2]
```

Then

*   The **First** house has **2** money

*   The **Second** house has **3** money

*   The **Third** house has **4** money

*   The **Fourth** house has **2** money

The robber can rob any number of houses but he cannot rob in two consecutive houses. For example, he can rob in below combinations for the above array

*   1 and 3

*   1 and 4

*   2 and 4

None of the combinations above has houses that are adjacent. The problem is to identify the combination which will yield maximum robbery to the robber.

For example, in the above case, the first combination (1 and 3) will give him maximum robber which is 2+4 =6Hence the robber can rob in first and third house which 2+4=6

Another example

```
Input: [1, 6, 8, 2, 3, 4]
Output: 13
```

The robber can rob in the first, third, and sixth houses which 1+8+4=13

It is a dynamic programming question as it has an optimal substructure. Let’s say the name of the array is **money**

*   dp[0] = money[0]

*   dp[1] = max(money[0], money[1])

*   dp[2] = max(money[0]+ money[1), money[2])

*   dp[i] = dp[i] + max(dp[i-1], dp[i-1]

where **dp[i]** represents the amount which a robber can rob if the ith house is included. In the end, we return the maximum in the **dp** array

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func rob(nums []int) int {
	lenNums := len(nums)

	if lenNums == 0 {
		return 0
	}

	maxMoney := make([]int, lenNums)

	maxMoney[0] = nums[0]

	if lenNums > 1 {
		maxMoney[1] = nums[1]
	}

	if lenNums > 2 {
		maxMoney[2] = nums[2] + nums[0]
	}

	for i := 3; i < lenNums; i++ {
		if maxMoney[i-2] > maxMoney[i-3] {
			maxMoney[i] = nums[i] + maxMoney[i-2]
		} else {
			maxMoney[i] = nums[i] + maxMoney[i-3]
		}

	}

	max := 0
	for i := lenNums; i < lenNums; i++ {
		if maxMoney[i] > max {
			max = maxMoney[i]
		}
	}

	return max
}

func main() {
	output := rob([]int{2, 3, 4, 2})
	fmt.Println(output)

	output = rob([]int{1, 6, 8, 2, 3, 4})
	fmt.Println(output)

}
```

**Output**

```
6
13
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*