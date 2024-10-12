<!--yml
category: 未分类
date: 2024-10-13 06:47:59
-->

# Merge two sorted arrays in Go (Golang)

> 来源：[https://golangbyexample.com/merge-two-sorted-arrays-golang/](https://golangbyexample.com/merge-two-sorted-arrays-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Two arrays are given. Both are sorted

*   First array is of length m+n

*   Second array is of n

The objective is to merge these sorted arrays. The first array contains enough length so the first array should be modified only

```
Input1: [2,3,4,0,0]
Input2: [1,5]
Output: [1, 2, 3, 4, 5]

Input1: [4,5,0,0,0,0]
Input2: [1, 2, 3, 7]
Output: [1, 2, 3, 4, 5, 7]
```

Here is the approach we can take

*   Move all elements in the first array to the end in sorted order. The first array will become

```
[0,0,2,3,4]
```

*   Now start from **mth** index element in the first array and **0th** index in the second array.

*   Compare the two and place the smaller at **0th** index in the first array. The first array will become

```
[1, 0, 2, 3, 4]
```

*   Repeat this process. The values at the end of the first array will be placed at the front before getting overridden as we have sufficient space

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func merge(nums1 []int, m int, nums2 []int, n int) []int {

	if m == 0 {
		for k := 0; k < n; k++ {
			nums1[k] = nums2[k]
		}
		return nums1
	}
	nums1 = moveToEnd(nums1, m)
	i := n
	j := 0
	for k := 0; k < m+n; k++ {
		if i < m+n && j < n {
			if nums1[i] < nums2[j] {
				nums1[k] = nums1[i]
				i++
			} else {
				nums1[k] = nums2[j]
				j++
			}
		} else if j < n {
			nums1[k] = nums2[j]
			j++
		}

	}

	return nums1

}

func moveToEnd(nums []int, m int) []int {
	lenNums := len(nums)

	k := lenNums

	for i := m - 1; i >= 0; i-- {
		nums[k-1] = nums[i]
		k--
	}

	return nums
}

func main() {
	output := merge([]int{2, 3, 4, 0, 0}, 3, []int{1, 5}, 2)
	fmt.Println(output)

	output = merge([]int{4, 5, 0, 0, 0, 0}, 2, []int{1, 2, 3, 7}, 4)
	fmt.Println(output)
}
```

**Output**

```
[1 2 3 4 5]
[1 2 3 4 5 7]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*