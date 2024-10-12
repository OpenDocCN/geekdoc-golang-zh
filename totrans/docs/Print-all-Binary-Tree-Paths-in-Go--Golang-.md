<!--yml
category: 未分类
date: 2024-10-13 06:50:57
-->

# Print all Binary Tree Paths in Go (Golang)

> 来源：[https://golangbyexample.com/print-all-binary-tree-paths-go/](https://golangbyexample.com/print-all-binary-tree-paths-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

The root of a tree is given. The objective is to print all the tree paths starting with the root.

**Example**

![](img/9a9347838908483552b24df3dc54cd38.png)

**Output:**

```
1->2->4
1->3->5
1->3->6
```

# **Program**

Below is the program for the same

```
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

**Output:**

```
[1->2->4 1->3->5 1->3->6]
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*