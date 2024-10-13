<!--yml

分类：未分类

日期：2024-10-13 06:52:00

-->

# 在 Go (Golang)中删除链表元素的程序

> 来源：[`golangbyexample.com/remove-linked-list-elements-golang/`](https://golangbyexample.com/remove-linked-list-elements-golang/)

目录

+   概述

+   程序

# **概述**

给定一个链表和一个值，删除链表中所有值等于给定值的节点。

**示例 1**

```go
Input: [1, 2, 1, 3, 6], 1
Output: [2, 3, 6]
```

**示例 2**

```go
Input: [2, 2, 3], 2
Output: [3]
```

# **程序**

下面是相应的程序

```go
package main

import "fmt"

type ListNode struct {
	Val  int
	Next *ListNode
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func (s *SingleList) AddFront(num int) *ListNode {
	ele := &ListNode{
		Val: num,
	}
	if s.Head == nil {
		s.Head = ele
	} else {
		ele.Next = s.Head
		s.Head = ele
	}
	s.Len++
	return ele
}
func removeElements(head *ListNode, val int) *ListNode {
	var prev *ListNode

	curr := head

	for curr != nil {
		if curr.Val == val {
			if prev == nil {
				head = curr.Next
			} else {
				prev.Next = curr.Next
			}
		} else {
			prev = curr
		}
		curr = curr.Next

	}

	return head
}

func main() {
	first := initList()
	first.AddFront(6)
	first.AddFront(3)
	first.AddFront(1)
	first.AddFront(2)
	first.AddFront(1)

	result := removeElements(first.Head, 1)
	fmt.Println("Resultant First List")
	result.Traverse()

	first = initList()
	first.AddFront(3)
	first.AddFront(2)
	first.AddFront(2)

	fmt.Println("\nResultant Second List")
	result = removeElements(first.Head, 2)
	result.Traverse()

}

func initList() *SingleList {
	return &SingleList{}
}

func (l *ListNode) Traverse() {
	for l != nil {
		fmt.Println(l.Val)
		l = l.Next
	}
}
```

**输出**

```go
Resultant First List
2
3
6

Resultant Second List
3
```

**注意：** 查看我们的 Golang 高级教程。该系列教程内容详尽，我们努力涵盖所有概念及示例。本教程适合希望获得专业知识和深入理解 Golang 的学习者 – [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解如何在 Golang 中实现所有设计模式感兴趣。如果是的话，这篇文章适合你 – [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

另外，可以在这里查看我们的系统设计教程系列 – [系统设计教程系列](https://techbyexample.com/system-design-questions/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
