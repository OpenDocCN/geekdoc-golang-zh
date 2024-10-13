<!--yml

分类：未分类

日期：2024-10-13 06:45:56

-->

# 在 Go 语言中，二叉树的先序遍历

> 来源：[`golangbyexample.com/preorder-binary-tree-golang/`](https://golangbyexample.com/preorder-binary-tree-golang/)

目录

+   概述

+   程序

## **概述**

在二叉树的先序遍历中，我们遵循以下顺序

+   访问根节点

+   访问左子树

+   访问右子树

例如，我们有以下二叉树

![](img/9a9347838908483552b24df3dc54cd38.png)

然后先序遍历为

```go
[1 2 4 3 5 6]
```

## **程序**

这是相应的程序

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

func preorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	left := preorderTraversal(root.Left)
	right := preorderTraversal(root.Right)

	output := make([]int, 0)

	output = append(output, root.Val)
	output = append(output, left...)
	output = append(output, right...)
	return output
}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := preorderTraversal(&root)
	fmt.Println(output)

}
```

**输出**

```go
[1 2 4 3 5 6]
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力覆盖所有概念并附有示例。本教程适合希望深入掌握 Golang 的用户 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
