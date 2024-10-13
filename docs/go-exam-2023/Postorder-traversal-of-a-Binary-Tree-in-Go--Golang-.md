<!--yml

分类：未分类

日期：2024-10-13 06:45:51

-->

# 在 Go (Golang) 中的二叉树后序遍历

> 来源：[https://golangbyexample.com/postorder-binary-tree-golang/](https://golangbyexample.com/postorder-binary-tree-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

在二叉树的后序遍历中，我们遵循以下顺序

+   访问左子树

+   访问右子树

+   访问根节点

例如，假设我们有如下二叉树

![](img/9a9347838908483552b24df3dc54cd38.png)

然后后序遍历结果为

```
[4 2 5 6 3 1]
```

## **程序**

下面是相应的程序

```
package main

import (
	"fmt"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	left := postorderTraversal(root.Left)
	right := postorderTraversal(root.Right)

	output := make([]int, 0)

	output = append(output, left...)

	output = append(output, right...)

	output = append(output, root.Val)
	return output
}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := postorderTraversal(&root)
	fmt.Println(output)

}
```

**输出**

```
[4 2 5 6 3 1]
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们努力覆盖所有概念及示例。本教程适合希望掌握 Golang 知识的用户 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对了解所有设计模式在 Golang 中的实现感兴趣，那这篇文章正适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
