<!--yml

类别：未分类

日期：2024-10-13 06:45:14

-->

# 从先序和中序在 Go (Golang)中构造二叉树

> 来源：[`golangbyexample.com/tree-preorder-inorder-golang/`](https://golangbyexample.com/tree-preorder-inorder-golang/)

目录

+   概述

+   程序

## **概述**

给定两个数组，它们表示一个二叉树的先序遍历和中序遍历。目标是从中构造一个二叉树。

示例：

考虑下面的树

![](img/9a9347838908483552b24df3dc54cd38.png)

树的先序遍历将是

```go
[1,2,4,3,5,6]
```

树的中序遍历将是

```go
[4,2,1,5,3,6]
```

将给定先序和中序数组，我们必须再次从中序和先序构造树。以下是策略

+   我们将使用三个索引，分别是数组的起始、结束和当前索引

+   先序中的起始索引将是根。

+   我们将在中序数组中找到一个索引，其值与先序数组的起始索引处的值匹配。我们将这个索引称为 rootIndex。

+   中序数组中 rootIndex 左侧的所有值将位于左子树中

+   中序数组中 rootIndex 右侧的所有值将位于右子树中

+   然后我们可以用相同的策略递归左子树，再递归右子树。

例如

+   先序遍历的第一个索引是根，其值为**1**

+   值**1**在中序遍历的**第 2**个索引上。因此 rootIndex 是**2**

+   中序遍历中 rootIndex 左侧的部分是**[4,2]**，这部分属于左子树

+   中序遍历中 rootIndex 右侧的部分是**[5,3,6]**，这部分属于右子树

+   我们可以先递归左子树，然后递归右子树

## **程序**

下面是相同程序的代码

```go
package main

import (
	"fmt"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func buildTree(preorder []int, inorder []int) *TreeNode {

	lenOfTree := len(preorder)

	current := 0
	newRoot := buildTreeUtil(preorder, inorder, &current, 0, lenOfTree-1)

	return newRoot
}

func buildTreeUtil(preorder []int, inorder []int, current *int, low, high int) *TreeNode {
	if low > high {
		return nil
	}

	if low == high {
		rootNode := &TreeNode{Val: preorder[*current]}
		(*current)++
		return rootNode
	}

	rootNode := &TreeNode{Val: preorder[*current]}
	rootValue := preorder[*current]
	(*current)++

	var rootIndex int
	for i := low; i <= high; i++ {
		if inorder[i] == rootValue {
			rootIndex = i
		}
	}

	rootNode.Left = buildTreeUtil(preorder, inorder, current, low, rootIndex-1)
	rootNode.Right = buildTreeUtil(preorder, inorder, current, rootIndex+1, high)

	return rootNode
}

func main() {
	inorder := []int{4, 2, 1, 5, 3, 6}
	preorder := []int{1, 2, 4, 3, 5, 6}

	root := buildTree(preorder, inorder)
	fmt.Printf("root: %d\n", root.Val)
	fmt.Printf("root.Left: %d\n", root.Left.Val)
	fmt.Printf("root.Left.Left: %d\n", root.Left.Left.Val)
	fmt.Printf("root.Right: %d\n", root.Right.Val)
	fmt.Printf("root.Right.Left: %d\n", root.Right.Left.Val)
	fmt.Printf("root.Right.Right: %d\n", root.Right.Right.Val)
}
```

**输出**

```go
root: 1
root.Left: 2
root.Left.Left: 4
root.Right: 3
root.Right.Left: 5
root.Right.Right: 6
```

**注意：** 查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尽力涵盖所有概念和示例。本教程适合那些希望获得专业知识和深入理解 Golang 的读者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
