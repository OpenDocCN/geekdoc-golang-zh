<!--yml
category: 未分类
date: 2024-10-13 06:44:53
-->

# Check if a given tree is a Binary Search Tree in Go (Golang)

> 来源：[https://golangbyexample.com/tree-is-bst-golang/](https://golangbyexample.com/tree-is-bst-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

We can use the below strategy to know if a given tree is a BST.

*   For a given current node, if the left and right subtree are BST

*   The maximum value in the left subtree is less than the current node value

*   The minimum value in the right subtree is greater than the current node value

Here is the program for the same.

## **Program**

```
package main

import (
	"fmt"
	"math"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isValidBST(root *TreeNode) bool {
	if root == nil {
		return true
	}
	isValid, _, _ := isValidBSTUtil(root)
	return isValid
}

func isValidBSTUtil(node *TreeNode) (bool, int, int) {

	if node.Left == nil && node.Right == nil {
		return true, node.Val, node.Val
	}

	min := node.Val
	max := node.Val

	isValidLeft := true
	var leftMin, leftMax int
	if node.Left != nil {
		isValidLeft, leftMin, leftMax = isValidBSTUtil(node.Left)

		if !isValidLeft {
			return false, 0, 0
		}
		if node.Val <= leftMax {
			return false, 0, 0
		}

		min = leftMin
	}

	isValidRight := true
	var rightMin, rightMax int

	if node.Right != nil {
		isValidRight, rightMin, rightMax = isValidBSTUtil(node.Right)

		if !isValidRight {
			return false, 0, 0
		}

		if node.Val >= rightMin {
			return false, 0, 0
		}
		max = rightMax
	}

	return true, min, max
}

func minOfFour(a, b, c, d int) int {
	return int(math.Min(float64(a), math.Min(float64(b), math.Min(float64(c), float64(d)))))
}

func maxOfFour(a, b, c, d int) int {
	return int(math.Max(float64(a), math.Max(float64(b), math.Max(float64(c), float64(d)))))
}

func main() {
	root := TreeNode{Val: 2}
	root.Left = &TreeNode{Val: 1}
	root.Right = &TreeNode{Val: 3}

	isValidBST := isValidBST(&root)
	fmt.Println(isValidBST)

}
```

**Output**

```
true
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*