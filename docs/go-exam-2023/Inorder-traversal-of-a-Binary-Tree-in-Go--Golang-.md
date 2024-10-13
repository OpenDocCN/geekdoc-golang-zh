<!--yml

类别: 未分类

日期: 2024-10-13 06:46:01

-->

# Go语言中的二叉树中序遍历

> 来源：[https://golangbyexample.com/inorder-binary-tree-golang/](https://golangbyexample.com/inorder-binary-tree-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

在二叉树的中序遍历中，我们遵循以下顺序

+   访问左子树

+   访问根节点

+   访问右子树

例如，假设我们有如下的二叉树

![](img/9a9347838908483552b24df3dc54cd38.png)

那么中序遍历将是

```
[4 2 1 5 3 6]
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

func inorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	left := inorderTraversal(root.Left)
	right := inorderTraversal(root.Right)

	output := make([]int, 0)

	output = append(output, left...)
	output = append(output, root.Val)
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

	output := inorderTraversal(&root)
	fmt.Println(output)

}
```

**输出**

```
[4 2 1 5 3 6]
```

**注意:** 请查看我们的Golang高级教程。本系列教程内容丰富，我们尝试用例子涵盖所有概念。本教程适合那些希望获得Golang专业知识和扎实理解的人 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是的话，这篇文章适合你 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [Go语言](https://golangbyexample.com/tag/go/)*
