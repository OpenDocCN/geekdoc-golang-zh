<!--yml
category: 未分类
date: 2024-10-13 06:45:19
-->

# Construct a binary tree from postorder and inorder in Go (Golang)

> 来源：[https://golangbyexample.com/binary-tree-postorder-inorder-golang/](https://golangbyexample.com/binary-tree-postorder-inorder-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Two arrays are given which represent the postorder and inorder traversal of a binary tree. The objective is to construct a binary tree from them

Example:

Consider below tree

![](img/9a9347838908483552b24df3dc54cd38.png)

Postorder traversal of the tree will be

```
[1,2,4,3,5,6]
```

Inorder traversal of the tree will be

```
[4,2,1,5,3,6]
```

Postorder and Inorder array will be given and we have to construct the tree again from the inorder and postorder. Below will be the strategy

*   We will use three indexes which is the start of the array, end of the array, and current index

*   The last index in the postorder will be the root.

*   We will find the index in an inorder array whose values match the value at the last index in the postorder array. Let’s this index be called rootIndex

*   All values in the left side of rootIndex in the inorder array will be in the left subtree

*   All values on the right side of rootIndex in the inorder array will be in the right subtree

*   We can then recurse with the same strategy for the right subtree and then the left subtree.

For eg

*   The last index in the postorder traversal is the root which is value **1**

*   The value **1** is at the **2nd** index in the inorder traversal. Hence rootIndex is **2**

*   The left part of rootIndex in the inorder traversal is **[4,2]**  which is part of the left subtree

*   The right side of rootIndex in the inorder traversal  is **[5,3,6]** which is part of the right subtree

*   We can recurse for the right subtree and then the left subtree

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

func buildTree(inorder []int, postorder []int) *TreeNode {
	lenTree := len(inorder)

	index := lenTree - 1
	return buildTreeUtil(inorder, postorder, &index, 0, lenTree-1)
}

func buildTreeUtil(inorder []int, postorder []int, index *int, low, high int) *TreeNode {
	if low > high {
		return nil
	}
	if low == high {
		currentIndexValue := postorder[*index]
		(*index)--
		return &TreeNode{Val: currentIndexValue}
	}

	currentIndexValue := postorder[*index]
	(*index)--

	root := &TreeNode{Val: currentIndexValue}

	mid := 0
	for i := low; i <= high; i++ {
		if inorder[i] == currentIndexValue {
			mid = i
		}
	}

	root.Right = buildTreeUtil(inorder, postorder, index, mid+1, high)
	root.Left = buildTreeUtil(inorder, postorder, index, low, mid-1)
	return root

}

func main() {
	inorder := []int{4, 2, 1, 5, 3, 6}
	postorder := []int{4, 2, 5, 6, 3, 1}

	root := buildTree(inorder, postorder)
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

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)*