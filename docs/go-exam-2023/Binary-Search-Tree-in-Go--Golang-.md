<!--yml

category: 未分类

date: 2024-10-13 06:06:05

-->

# Go 语言中的二叉搜索树

> 来源：[`golangbyexample.com/binary-search-tree-in-go/`](https://golangbyexample.com/binary-search-tree-in-go/)

目录

**   介绍

+   完整工作代码*  *# **介绍**

二叉搜索树（BST）是一个二叉树。每个节点在二叉搜索树中

+   左子树中每个节点的值都小于当前节点的值

+   右子树中每个节点的值都大于当前节点的值

+   左右子树本身都是二叉搜索树

# **完整工作代码**

```go
package main

import "fmt"

type bstnode struct {
    value int
    left  *bstnode
    right *bstnode
}

type bst struct {
    root *bstnode
}

func (b *bst) reset() {
    b.root = nil
}

func (b *bst) insert(value int) {
    b.insertRec(b.root, value)
}

func (b *bst) insertRec(node *bstnode, value int) *bstnode {
    if b.root == nil {
        b.root = &bstnode{
            value: value,
        }
        return b.root
    }
    if node == nil {
        return &bstnode{value: value}
    }
    if value <= node.value {
        node.left = b.insertRec(node.left, value)
    }
    if value > node.value {
        node.right = b.insertRec(node.right, value)
    }
    return node
}

func (b *bst) find(value int) error {
    node := b.findRec(b.root, value)
    if node == nil {
        return fmt.Errorf("Value: %d not found in tree", value)
    }
    return nil
}

func (b *bst) findRec(node *bstnode, value int) *bstnode {
    if node == nil {
        return nil
    }
    if node.value == value {
        return b.root
    }
    if value < node.value {
        return b.findRec(node.left, value)
    }
    return b.findRec(node.right, value)
}

func (b *bst) inorder() {
    b.inorderRec(b.root)
}

func (b *bst) inorderRec(node *bstnode) {
    if node != nil {
        b.inorderRec(node.left)
        fmt.Println(node.value)
        b.inorderRec(node.right)
    }
}

func main() {
    bst := &bst{}
    eg := []int{2, 5, 7, -1, -1, 5, 5}
    for _, val := range eg {
        bst.insert(val)
    }
    fmt.Printf("Printing Inorder:\n")
    bst.inorder()
    bst.reset()
    eg = []int{4, 5, 7, 6, -1, 99, 5}
    for _, val := range eg {
        bst.insert(val)
    }
    fmt.Printf("\nPrinting Inorder:\n")
    bst.inorder()
    fmt.Printf("\nFinding Values:\n")
    err := bst.find(2)
    if err != nil {
        fmt.Printf("Value %d Not Found\n", 2)
    } else {
        fmt.Printf("Value %d Found\n", 2)
    }
    err = bst.find(6)
    if err != nil {
        fmt.Printf("Value %d Not Found\n", 6)
    } else {
        fmt.Printf("Value %d Found\n", 6)
    }
    err = bst.find(5)
    if err != nil {
        fmt.Printf("Value %d Not Found\n", 5)
    } else {
        fmt.Printf("Value %d Found\n", 5)
    }
    err = bst.find(1)
    if err != nil {
        fmt.Printf("Value %d Not Found\n", 1)
    } else {
        fmt.Printf("Value %d Found\n", 1)
    }
}
```

**输出：**

```go
Printing Inorder:
-1
-1
2
5
5
5
7

Printing Inorder:
-1
4
5
5
6
7
99

Finding Values:
Value 2 Not Found
Value 6 Found
Value 5 Found
Value 1 Not Found
```

+   [Go 语言中的二叉搜索树](https://golangbyexample.com/tag/binary-search-tree-in-golang/)*   [bst](https://golangbyexample.com/tag/bst/)*
