<!--yml

分类：未分类

日期：2024-10-13 06:50:57

-->

# 在 Go (Golang)中打印所有二叉树路径

> 来源：[`golangbyexample.com/print-all-binary-tree-paths-go/`](https://golangbyexample.com/print-all-binary-tree-paths-go/)

目录

+   概述

+   程序

# **概述**

给定一棵树的根节点。目标是打印所有从根节点开始的树路径。

**示例**

![](img/9a9347838908483552b24df3dc54cd38.png)

**输出：**

```go
1->2->4
1->3->5
1->3->6
```

# **程序**

下面是相同程序的代码

```go
package main

import (
	"fmt"
	"strconv"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func binaryTreePaths(root *TreeNode) []string {
	output := make([]string, 0)

	binaryTreePathsUtil(root, "", &output)
	return output
}

func binaryTreePathsUtil(root *TreeNode, curr string, output *[]string) {
	if root == nil {
		return
	}

	valString := strconv.Itoa(root.Val)
	if curr == "" {
		curr = valString
	} else {
		curr = curr + "->" + valString
	}
	if root.Left == nil && root.Right == nil {
		*output = append(*output, curr)
		return
	}

	binaryTreePathsUtil(root.Left, curr, output)
	binaryTreePathsUtil(root.Right, curr, output)

}

func main() {
	root := TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Left.Left = &TreeNode{Val: 4}
	root.Right = &TreeNode{Val: 3}
	root.Right.Left = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}

	output := binaryTreePaths(&root)
	fmt.Println(output)
}
```

**输出：**

```go
[1->2->4 1->3->5 1->3->6]
```

**注意：** 请查看我们的 Golang 高级教程。本系列的教程内容详尽，我们尝试用示例覆盖所有概念。这个教程是为了那些希望获得 Golang 专业知识和扎实理解的人准备的 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，请查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)


