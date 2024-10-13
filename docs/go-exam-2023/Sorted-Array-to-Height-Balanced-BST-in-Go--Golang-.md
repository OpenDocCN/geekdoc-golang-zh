<!--yml

category: 未分类

date: 2024-10-13 06:46:59

-->

# 将已排序数组转换为高度平衡的 BST（Golang）。

> 来源：[`golangbyexample.com/sorted-array-balanced-bst-golang/`](https://golangbyexample.com/sorted-array-balanced-bst-golang/)。

目录

+   概述

+   程序

## **概述**

给定一个已排序的数组。数组按升序排序。目标是将该已排序数组转换为高度平衡的 BST。

平衡 BST 是指每个节点的左子树和右子树的高度差最大为 1 的 BST。

## **程序**

这里是相同程序的代码。

```go
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

**输出**

```go
1
2
3
4
5
```

**注意：** 请查看我们的 Golang 高级教程。此系列的教程内容详尽，我们尝试涵盖所有概念并附带示例。此教程适合那些希望获得专业知识和对 golang 有扎实理解的读者——[Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)。

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你——[所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)。


