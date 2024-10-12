<!--yml
category: 未分类
date: 2024-10-13 06:46:01
-->

# Inorder traversal of a Binary Tree in Go (Golang)

> 来源：[https://golangbyexample.com/inorder-binary-tree-golang/](https://golangbyexample.com/inorder-binary-tree-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

In the inorder traversal of a binary tree, we follow the below order

*   Vist Left Subtree

*   Visit Root

*   Visit Right Subtree

For example, let’s say we have below binary tree

![](img/9a9347838908483552b24df3dc54cd38.png)

Then inorder traversal would be

```
[4 2 1 5 3 6]
```

## **Program**

Here is the program for the same

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

**Output**

```
[4 2 1 5 3 6]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*