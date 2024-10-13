<!--yml

类别：未分类

日期：2024-10-13 06:46:39

-->

# 在Go（Golang）中旋转链表

> 来源：[https://golangbyexample.com/rotate-linked-list-golang/](https://golangbyexample.com/rotate-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是将给定列表按指定数量向右旋转。

示例

```
Input: 1->2->3->4->5
k: 2

Output: 4->5->1->2->3
```

## **程序**

这是相应的程序。

```
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

	head := rotateRight(first.Head, 2)
	fmt.Println("")
	head.Traverse()
	fmt.Println("")

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
		fmt.Print(l.Val)
		l = l.Next
	}
}

type SingleList struct {
	Len  int
	Head *ListNode
}

func (s *SingleList) Reverse() {

	curr := s.Head
	var prev *ListNode
	var next *ListNode

	for curr != nil {
		next = curr.Next
		curr.Next = prev
		prev = curr
		curr = next
	}
	s.Head = prev
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

func rotateRight(head *ListNode, k int) *ListNode {

	if head == nil {
		return nil
	}

	if k == 0 {
		return head
	}

	lenList := 0

	curr := head
	var last *ListNode

	for curr != nil {
		lenList++
		last = curr
		curr = curr.Next
	}

	k = k % lenList
	if k == 0 {
		return head
	}

	curr = head

	newHeadIndex := lenList - k

	var prev *ListNode

	for i := 0; i < newHeadIndex; i++ {
		prev = curr
		curr = curr.Next
	}

	newHeadNode := prev.Next

	prev.Next = nil

	last.Next = head
	return newHeadNode
}
```

**输出**

```
12345
45123
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们尝试涵盖所有概念及示例。本教程适合那些希望获得专业知识和对Golang有深入理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是，那么这篇文章就是为你准备的 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [去](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*
