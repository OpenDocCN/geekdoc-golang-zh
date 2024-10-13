<!--yml

分类：未分类

日期：2024-10-13 06:45:09

-->

# Go语言中的二叉树高度或最大深度

> 来源：[https://golangbyexample.com/height-binary-tree-golang/](https://golangbyexample.com/height-binary-tree-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是获取二叉树的高度。例如，假设我们有下面的二叉树

![](img/9a9347838908483552b24df3dc54cd38.png)

那么这里二叉树的高度是3。

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

func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	if root.Left == nil && root.Right == nil {
		return 1
	}

	lHeight := maxDepth(root.Left)
	rHeight := maxDepth(root.Right)

	if lHeight >= rHeight {
		return lHeight + 1
	} else {
		return rHeight + 1
	}
}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	height := maxDepth(&root)
	fmt.Println(height)

}
```

**输出**

```go
3
```

**注意：** 请查看我们的Golang高级教程。本系列教程详尽，我们尝试涵盖所有概念及示例。本教程适合希望获得专业知识和对golang有扎实理解的读者 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解如何在Golang中实现所有设计模式感兴趣。如果是的话，这篇文章适合你 –[所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
