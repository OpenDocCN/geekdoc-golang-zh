<!--yml
category: 未分类
date: 2024-10-13 06:41:48
-->

# Find two numbers in an array that adds up to a target number in Go (Golang)

> 来源：[https://golangbyexample.com/target-sum-golang/](https://golangbyexample.com/target-sum-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

For example, let’s say we have a given array

```
[2, 5, 1, 3]
```

The target number is **4**

Then the answer will be index

```
[2, 3]
```

as we have

*   Number 1 at index 2

*   Number 3 at index 3

and 1+3 = 4

Do note that the array is unsorted

Expected TC – O(n)

We can use a hash for the solution. It is based upon the idea that

*   If say one of the numbers is **x**

*   Then the other number will be **target-x**

So if for a number **x** we check that **target-x** is in the hash. If it is then we know we have the solution

Let’s see a program for the same.

## **Program**

```
package main

import "fmt"

func main() {
	output := twoTargetSums([]int{2, 5, 1, 3}, 4)
	fmt.Println(output)
}

func twoTargetSums(nums []int, target int) []int {
	numberMap := make(map[int]int)
	output := make([]int, 2)
	for i := 0; i < len(nums); i++ {
		val, ok := numberMap[target-nums[i]]
		if ok {
			output[0] = val
			output[1] = i
			return output
		} else {
			numberMap[nums[i]] = i
		}
	}
	return output
}
```

**Output**

```
[2 3]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*