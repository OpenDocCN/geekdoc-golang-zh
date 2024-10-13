<!--yml

类别：未分类

日期：2024-10-13 06:48:34

-->

# Go（Golang）中的相同二叉树程序

> 来源：[`golangbyexample.com/same-binary-tree-golang/`](https://golangbyexample.com/same-binary-tree-golang/)

目录

+   概述

+   程序

## **概述**

目标是检查给定的两棵二叉树是否相同。以下树是相同的

**树 1**

![](img/9a9347838908483552b24df3dc54cd38.png)

**树 2**

![](img/9a9347838908483552b24df3dc54cd38.png)

## **程序**

这里是相同的程序。

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

func isSameTree(p *TreeNode, q *TreeNode) bool {
	if p == nil && q == nil {
		return true
	}

	if p == nil || q == nil {
		return false
	}

	if p.Val != q.Val {
		return false
	}

	return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}

func main() {
	root1 := TreeNode{Val: 1}
	root1.Left = &TreeNode{Val: 2}
	root1.Left.Left = &TreeNode{Val: 4}
	root1.Right = &TreeNode{Val: 3}
	root1.Right.Left = &TreeNode{Val: 5}
	root1.Right.Right = &TreeNode{Val: 6}

	root2 := TreeNode{Val: 1}
	root2.Left = &TreeNode{Val: 2}
	root2.Left.Left = &TreeNode{Val: 4}
	root2.Right = &TreeNode{Val: 3}
	root2.Right.Left = &TreeNode{Val: 5}
	root2.Right.Right = &TreeNode{Val: 6}

	output := isSameTree(&root1, &root2)
	fmt.Println(output)

	root1 = TreeNode{Val: 1}
	root1.Left = &TreeNode{Val: 2}

	root2 = TreeNode{Val: 1}
	root2.Left = &TreeNode{Val: 3}

	output = isSameTree(&root1, &root2)
	fmt.Println(output)
}
```

**输出**

```go
true
false
```

**注意：** 查看我们的 Golang 高级教程。本系列教程详尽，我们尽力覆盖所有概念并提供示例。本教程适合希望获得专业知识和扎实理解 Golang 的读者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


