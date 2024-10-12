<!--yml
category: 未分类
date: 2024-10-13 06:51:12
-->

# Check If N and Its Double Exist in Go (Golang)

> 来源：[https://golangbyexample.com/number-double-golang/](https://golangbyexample.com/number-double-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

An array is given. The objective is to find if there exists any number for which it’s double also exists.

**Example 1**

```
Input: [8,5,4,3]
Output: true
Explanation: 4 and 8
```

**Example 2**

```
Input: [1,3,7,9]
Output: false
Explanation: There exists no number for which its double exist
```

The idea is to use a map here. For every element, we will check  and return true if

*   If its double exists in the map

*   If the number is even then if its half exists in the map

# **Program**

Below is the program for the same

```
package main

import "fmt"

func checkIfExist(arr []int) bool {
	numMap := make(map[int]bool)

	for i := 0; i < len(arr); i++ {
		if numMap[arr[i]*2] {
			return true
		}
		if arr[i]%2 == 0 && numMap[arr[i]/2] {
			return true
		}
		numMap[arr[i]] = true
	}
	return false
}

func main() {
	output := checkIfExist([]int{8, 5, 4, 3})
	fmt.Println(output)

	output = checkIfExist([]int{1, 3, 7, 9})
	fmt.Println(output)

}
```

**Output:**

```
true
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you - [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here - [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*