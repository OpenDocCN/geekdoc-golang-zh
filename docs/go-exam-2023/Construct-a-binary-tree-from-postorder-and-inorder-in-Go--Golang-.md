<!--yml

分类: 未分类

日期: 2024-10-13 06:45:19

-->

# 在 Go (Golang)中从后序和中序构建二叉树

> 来源：[`golangbyexample.com/binary-tree-postorder-inorder-golang/`](https://golangbyexample.com/binary-tree-postorder-inorder-golang/)

目录

+   概述

+   程序

## **概述**

给定两个数组，表示二叉树的后序和中序遍历。目标是从中构建二叉树

示例:

考虑下面的树

![](img/9a9347838908483552b24df3dc54cd38.png)

树的后序遍历将是

```go
[1,2,4,3,5,6]
```

树的中序遍历将是

```go
[4,2,1,5,3,6]
```

将提供后序和中序数组，我们必须再次从中序和后序构建树。以下将是策略

+   我们将使用三个索引，即数组的开始、结束和当前索引

+   后序中的最后一个索引将是根节点。

+   我们将在中序数组中查找与后序数组最后一个索引的值匹配的索引。我们将此索引称为 rootIndex

+   在中序数组中，rootIndex 左侧的所有值将位于左子树中

+   在中序数组中，rootIndex 右侧的所有值将位于右子树中

+   我们可以使用相同的策略递归处理右子树，然后处理左子树。

例如

+   后序遍历中的最后一个索引是根节点，其值为 **1**

+   值 **1** 在中序遍历中的 **第 2** 个索引。因此，rootIndex 是 **2**

+   在中序遍历中，rootIndex 左侧的部分是 **[4,2]**，它是左子树的一部分

+   在中序遍历中，rootIndex 右侧是 **[5,3,6]**，它是右子树的一部分

+   我们可以递归处理右子树，然后处理左子树

## **程序**

以下是相应的程序

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

func buildTree(inorder []int, postorder []int) *TreeNode {
	lenTree := len(inorder)

	index := lenTree - 1
	return buildTreeUtil(inorder, postorder, &index, 0, lenTree-1)
}

func buildTreeUtil(inorder []int, postorder []int, index *int, low, high int) *TreeNode {
	if low > high {
		return nil
	}
	if low == high {
		currentIndexValue := postorder[*index]
		(*index)--
		return &TreeNode{Val: currentIndexValue}
	}

	currentIndexValue := postorder[*index]
	(*index)--

	root := &TreeNode{Val: currentIndexValue}

	mid := 0
	for i := low; i <= high; i++ {
		if inorder[i] == currentIndexValue {
			mid = i
		}
	}

	root.Right = buildTreeUtil(inorder, postorder, index, mid+1, high)
	root.Left = buildTreeUtil(inorder, postorder, index, low, mid-1)
	return root

}

func main() {
	inorder := []int{4, 2, 1, 5, 3, 6}
	postorder := []int{4, 2, 5, 6, 3, 1}

	root := buildTree(inorder, postorder)
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

**注意：** 查看我们的 Golang 高级教程。本系列的教程详细且我们已尝试用示例涵盖所有概念。这个教程适合那些希望获得专业知识和对 golang 有扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)*
