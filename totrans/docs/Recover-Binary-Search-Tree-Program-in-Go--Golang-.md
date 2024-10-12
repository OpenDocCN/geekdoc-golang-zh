<!--yml
category: 未分类
date: 2024-10-13 06:51:50
-->

# Recover Binary Search Tree Program in Go (Golang)

> 来源：[https://golangbyexample.com/recover-binary-search-tree-golang/](https://golangbyexample.com/recover-binary-search-tree-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The root of a binary search tree is given. Two nodes of the binary search tree have been swapped. We need to fix the binary tree and recover the original structure

# **Program**

Below is the program for the same

```
package main

import "fmt"

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func recoverTree(root *TreeNode) {
	var prev *TreeNode
	var first *TreeNode
	var mid *TreeNode
	var last *TreeNode

	recoverTreeUtil(root, &prev, &first, &mid, &last)

	if first != nil && last != nil {
		first.Val, last.Val = last.Val, first.Val
	} else if first != nil && mid != nil {
		first.Val, mid.Val = mid.Val, first.Val
	}
}

func recoverTreeUtil(root *TreeNode, prev, first, mid, last **TreeNode) {
	if root == nil {
		return
	}

	recoverTreeUtil(root.Left, prev, first, mid, last)

	if *prev == nil {
		*prev = root
	} else if *first == nil && (*prev).Val > root.Val {
		*first = *prev
		*mid = root
	} else if (*prev).Val > root.Val {
		*last = root
	}

	*prev = root

	recoverTreeUtil(root.Right, prev, first, mid, last)
}

func main() {
	root := TreeNode{Val: 2}
	root.Left = &TreeNode{Val: 3}
	root.Right = &TreeNode{Val: 1}

	recoverTree(&root)
	fmt.Println(root.Val)
	fmt.Println(root.Left.Val)
	fmt.Println(root.Right.Val)

}
```

**Output**

```
2
1
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*