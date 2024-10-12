<!--yml
category: 未分类
date: 2024-10-13 06:46:59
-->

# Sorted Array to Height Balanced BST in Go (Golang)

> 来源：[https://golangbyexample.com/sorted-array-balanced-bst-golang/](https://golangbyexample.com/sorted-array-balanced-bst-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program ")*  *## **Overview**

A sorted array is given. The array is sorted in ascending order. The objective is to convert that sorted array to a Height Balanced BST.

A balanced BST is a BST in which the height of the left subtree and height of the right subtree differs by max 1 for every node.

## **Program**

Here is the program for the same.

```
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func sortedArrayToBST(nums []int) *TreeNode {
	length := len(nums)
	return sortedArrayToBSTUtil(nums, 0, length-1)
}

func sortedArrayToBSTUtil(nums []int, first int, last int) *TreeNode {

	if first > last {
		return nil
	}

	if first == last {
		return &TreeNode{
			Val: nums[first],
		}
	}

	mid := (first + last) / 2

	root := &TreeNode{
		Val: nums[mid],
	}

	root.Left = sortedArrayToBSTUtil(nums, first, mid-1)
	root.Right = sortedArrayToBSTUtil(nums, mid+1, last)
	return root
}

func traverseInorder(root *TreeNode) {
	if root == nil {
		return
	}

	traverseInorder(root.Left)
	fmt.Println(root.Val)
	traverseInorder(root.Right)
}

func main() {
	root := sortedArrayToBST([]int{1, 2, 3, 4, 5})
	traverseInorder(root)

}
```

**Output**

```
1
2
3
4
5
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*