<!--yml

分类：未分类

日期：2024-10-13 06:43:19

-->

# 在 Go (Golang) 中交换链表中的节点

> 来源：[`golangbyexample.com/pair-swap-nodes-linked-list-go/`](https://golangbyexample.com/pair-swap-nodes-linked-list-go/)

目录

+   概述

+   程序

## **概述**

给定一个链表和一个数字 k，将链表中的节点按 k 的分组进行反转。

例如

```go
Input: 1->2->3->4->5->6->7
k: 3
Output: 3->2->1->6->5->4->7
```

如果链表最后一组的长度小于 k，则保持最后一组不变

## **程序**

```go
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(4)
	first.AddFront(3)
	first.AddFront(2)
	first.AddFront(1)

	first.Head.Traverse()
	temp := ReverseKGroup(first.Head, 3)
        fmt.Println()
	temp.Traverse()

}

func initList() *SingleList {
	return &SingleList{}
}

type ListNode struct {
	Val  int
	Next *ListNode
}

func (l *ListNode) Traverse() {
	for l != nil {
		fmt.Println(l.Val)
		l = l.Next
	}
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func ReverseKGroup(head *ListNode, k int) *ListNode {

	curr := head
	var prev *ListNode
	var next *ListNode

	i := 0

	for curr != nil && i < k {
		i++
		curr = curr.Next
	}
	if i == k {
		curr = head
	} else {
		return head
	}

	i = 0
	for curr != nil && i < k {
		next = curr.Next
		curr.Next = prev
		prev = curr
		curr = next
		i++
	}

	head.Next = ReverseKGroup(curr, k)
	return prev
}

func (s *SingleList) AddFront(num int) {
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
}
```

**输出**

```go
1
2
3
4

3
2
1
4
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们努力涵盖所有概念和示例。这个教程适合那些希望获得 Golang 专业知识和扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在 Golang 中实现所有设计模式。如果是的话，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
