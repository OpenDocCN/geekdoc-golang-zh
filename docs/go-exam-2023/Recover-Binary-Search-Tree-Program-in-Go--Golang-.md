<!--yml

分类：未分类

日期：2024-10-13 06:51:50

-->

# 在 Go (Golang)中恢复二叉搜索树程序

> 来源：[`golangbyexample.com/recover-binary-search-tree-golang/`](https://golangbyexample.com/recover-binary-search-tree-golang/)

目录

+   概述

+   程序

# **概述**

给定一个二叉搜索树的根节点。两个二叉搜索树的节点已被交换。我们需要修复二叉树并恢复原始结构。

# **程序**

下面是相应的程序

```go
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func recoverTree(root *TreeNode) {
	var prev *TreeNode
	var first *TreeNode
	var mid *TreeNode
	var last *TreeNode

	recoverTreeUtil(root, &prev, &first, &mid, &last)

	if first != nil && last != nil {
		first.Val, last.Val = last.Val, first.Val
	} else if first != nil && mid != nil {
		first.Val, mid.Val = mid.Val, first.Val
	}
}

func recoverTreeUtil(root *TreeNode, prev, first, mid, last **TreeNode) {
	if root == nil {
		return
	}

	recoverTreeUtil(root.Left, prev, first, mid, last)

	if *prev == nil {
		*prev = root
	} else if *first == nil && (*prev).Val > root.Val {
		*first = *prev
		*mid = root
	} else if (*prev).Val > root.Val {
		*last = root
	}

	*prev = root

	recoverTreeUtil(root.Right, prev, first, mid, last)
}

func main() {
	root := TreeNode{Val: 2}
	root.Left = &TreeNode{Val: 3}
	root.Right = &TreeNode{Val: 1}

	recoverTree(&root)
	fmt.Println(root.Val)
	fmt.Println(root.Left.Val)
	fmt.Println(root.Right.Val)

}
```

**输出**

```go
2
1
3
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽量涵盖所有概念和示例。本教程适合希望获得专业知识和扎实理解 Golang 的人 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解所有设计模式如何在 Golang 中实现感兴趣的话。如果是，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

此外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
