<!--yml

分类：未分类

日期：2024-10-13 06:48:19

-->

# 在 Go (Golang)中对链表进行分区

> 来源：[`golangbyexample.com/partition-linked-list-golang/`](https://golangbyexample.com/partition-linked-list-golang/)

目录

+   概述

+   程序

## **概述**

给定一个链表，同时给定一个目标值。将给定的链表分区，使得所有小于目标值的元素都在所有大于目标值的元素之前。

示例

```go
Input: 4->5->3->1
Output: 2
Target: 1->4->5->3
```

原始顺序也应予以保留。

## **程序**

这里是相应的程序。

```go
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(1)
	first.AddFront(2)
	first.AddFront(3)
	first.AddFront(4)

	first.Head.Traverse()
	newHead := partition(first.Head, 2)
	fmt.Println("")
	newHead.Traverse()

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

func partition(head *ListNode, x int) *ListNode {
	if head == nil {
		return nil
	}

	curr := head

	var prev *ListNode

	for curr != nil {
		if curr.Val >= x {
			break
		}
		prev = curr
		curr = curr.Next
	}

	if curr == nil {
		return head
	}

	firstLargeValueNode := curr

	prev2 := firstLargeValueNode
	for curr != nil {
		if curr.Val < x {
			prev2.Next = curr.Next
			if prev != nil {
				prev.Next = curr
				prev = prev.Next
				prev.Next = firstLargeValueNode
			} else {
				if head == firstLargeValueNode {
					head = curr
				}
				curr.Next = firstLargeValueNode
				prev = curr
			}
		}
		prev2 = curr
		curr = curr.Next
	}

	return head
}
```

**输出**

```go
4
5
3
1

1
4
5
3
```

**注意：** 请查看我们的 Golang 高级教程。本系列教程详细而全面，涵盖了所有概念及示例。本教程适合那些希望掌握 Golang 并获得扎实理解的人 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。 如果是的话，那么这篇文章适合你 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


