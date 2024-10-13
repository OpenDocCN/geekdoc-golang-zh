<!--yml

类别：未分类

日期：2024-10-13 06:47:32

-->

# 在 Go（Golang）中删除链表的中间节点

> 来源：[`golangbyexample.com/delete-middle-node-linked-list-golang/`](https://golangbyexample.com/delete-middle-node-linked-list-golang/)

目录

+   概述

+   程序

## **概述**

目标是删除链表的中间节点。如果 x 是链表的大小，则中间节点是

```go
mid = x/2
```

示例

```go
Input: 1->2->3->4->5
Output: 1->2->4->5
```

## **程序**

下面是相应的程序。

```go
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(5)
	first.AddFront(4)
	first.AddFront(3)
	first.AddFront(2)
	first.AddFront(1)

	first.Head.Traverse()
	deleteMiddle(first.Head)
	fmt.Println("")
	first.Head.Traverse()

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

func deleteMiddle(head *ListNode) *ListNode {

	if head == nil {
		return nil
	}

	size := sizeOfList(head)

	mid := size / 2

	if mid == 0 {
		return head.Next
	}

	curr := head
	for i := 0; i < mid-1; i++ {
		curr = curr.Next
	}

	prev := curr

	midNode := prev.Next

	if midNode == nil {
		return head
	}

	midNext := midNode.Next

	prev.Next = midNext

	return head

}

func sizeOfList(head *ListNode) int {
	l := 0
	for head != nil {
		l = l + 1
		head = head.Next
	}
	return l
}
```

**输出**

```go
1
2
3
4
5

1
2
4
5
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程内容详尽，我们尽力涵盖所有概念及其示例。本教程适合那些希望获得专业知识和深入理解 Golang 的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对理解如何在 Golang 中实现所有设计模式感兴趣。如果是的话，这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
