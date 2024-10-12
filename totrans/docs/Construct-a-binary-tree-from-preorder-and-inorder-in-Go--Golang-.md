<!--yml
category: 未分类
date: 2024-10-13 06:45:14
-->

# Construct a binary tree from preorder and inorder in Go (Golang)

> 来源：[https://golangbyexample.com/tree-preorder-inorder-golang/](https://golangbyexample.com/tree-preorder-inorder-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Two arrays are given which represent the preorder and inorder traversal of a binary tree. The objective is to construct a binary tree from them

Example:

Consider below tree

![](img/9a9347838908483552b24df3dc54cd38.png)

Preorder traversal of the tree will be

```
[1,2,4,3,5,6]
```

Inorder traversal of the tree will be

```
[4,2,1,5,3,6]
```

Preorder and Inorder array will be given and we have to construct the tree again from the inorder and preorder. Below will be the strategy

*   We will use three indexes which is the start of the array, end of the array, and current index

*   The start index in the preorder will be the root.

*   We will find the index in an inorder array whose values match the value at the start index in the preorder array. Let’s this index be called rootIndex

*   All values in the left side of rootIndex in the inorder array will be in the left subtree

*   All values on the right side of rootIndex in the inorder array will be in the right subtree

*   We can then recurse with the same strategy for the left subtree and then the right subtree.

For eg

*   The first index in the preorder traversal is the root which is value **1**

*   The value **1** is at the **2nd** index in the inorder traversal. Hence rootIndex is **2**

*   The left part of rootIndex in the inorder traversal is **[4,2]**  which is part of the left subtree

*   The right side of rootIndex in the inorder traversal  is **[5,3,6]** which is part of the right subtree

*   We can recurse for the left subtree and then the right subtree

## **Program**

Below is the program for the same

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

func buildTree(preorder []int, inorder []int) *TreeNode {

	lenOfTree := len(preorder)

	current := 0
	newRoot := buildTreeUtil(preorder, inorder, &current, 0, lenOfTree-1)

	return newRoot
}

func buildTreeUtil(preorder []int, inorder []int, current *int, low, high int) *TreeNode {
	if low > high {
		return nil
	}

	if low == high {
		rootNode := &TreeNode{Val: preorder[*current]}
		(*current)++
		return rootNode
	}

	rootNode := &TreeNode{Val: preorder[*current]}
	rootValue := preorder[*current]
	(*current)++

	var rootIndex int
	for i := low; i <= high; i++ {
		if inorder[i] == rootValue {
			rootIndex = i
		}
	}

	rootNode.Left = buildTreeUtil(preorder, inorder, current, low, rootIndex-1)
	rootNode.Right = buildTreeUtil(preorder, inorder, current, rootIndex+1, high)

	return rootNode
}

func main() {
	inorder := []int{4, 2, 1, 5, 3, 6}
	preorder := []int{1, 2, 4, 3, 5, 6}

	root := buildTree(preorder, inorder)
	fmt.Printf("root: %d\n", root.Val)
	fmt.Printf("root.Left: %d\n", root.Left.Val)
	fmt.Printf("root.Left.Left: %d\n", root.Left.Left.Val)
	fmt.Printf("root.Right: %d\n", root.Right.Val)
	fmt.Printf("root.Right.Left: %d\n", root.Right.Left.Val)
	fmt.Printf("root.Right.Right: %d\n", root.Right.Right.Val)
}
```

**Output**

```
root: 1
root.Left: 2
root.Left.Left: 4
root.Right: 3
root.Right.Left: 5
root.Right.Right: 6
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*