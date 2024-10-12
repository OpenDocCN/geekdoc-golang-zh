<!--yml
category: 未分类
date: 2024-10-13 06:49:33
-->

# Kth Distinct String in an Array Program in Go (Golang)

> 来源：[https://golangbyexample.com/kth-distinct-string-golang/](https://golangbyexample.com/kth-distinct-string-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

An input string array is given which can have duplicate strings as well. An input number **k** is also provided. Idea is to find the first kth distinct string in the given input string array

Let’s understand it with an example

```
Input: ["a", "c", "b" , "c", "a", "b", "e", "d"]
k=2

Output: "d"
```

In the above array below strings are duplicated

*   “a”

*   “c”

*   “b”

and below strings are not duplicated

*   “e”

*   “d”

Since string **“d”** appears second in order and k is 2 hence the output is **“d”**Another example

```
Input: ["xxx", "xx" "x"]
k=2

Output: "xx"
```

For the same reasoning as above

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