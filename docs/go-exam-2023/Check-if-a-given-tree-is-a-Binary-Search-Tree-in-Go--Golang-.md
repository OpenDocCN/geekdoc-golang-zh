<!--yml

类别：未分类

日期：2024-10-13 06:44:53

-->

# 检查给定的树是否为二叉搜索树（BST）在 Go (Golang)中

> 来源：[`golangbyexample.com/tree-is-bst-golang/`](https://golangbyexample.com/tree-is-bst-golang/)

目录

**   概述

+   程序*  *## **概述**

我们可以使用以下策略来判断给定的树是否为 BST。

+   对于给定的当前节点，如果左子树和右子树都是 BST

+   左子树中的最大值小于当前节点值。

+   右子树中的最小值大于当前节点值。

这是相应的程序。

## **程序**

```go
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

**输出**

```go
true
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念并提供示例。本教程适合那些希望深入了解 Golang 并获得专业知识的人—— [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是，那么这篇文章适合你—— [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
