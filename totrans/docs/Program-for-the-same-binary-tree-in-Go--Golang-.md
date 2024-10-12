<!--yml
category: 未分类
date: 2024-10-13 06:48:34
-->

# Program for the same binary tree in Go (Golang)

> 来源：[https://golangbyexample.com/same-binary-tree-golang/](https://golangbyexample.com/same-binary-tree-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find if two given binary trees are the same or notExampleBelow trees are the same

**Tree 1**

![](img/9a9347838908483552b24df3dc54cd38.png)

**Tree 2**

![](img/9a9347838908483552b24df3dc54cd38.png)

## **Program**

Here is the program for the same.

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

**Output**

```
true
false
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*