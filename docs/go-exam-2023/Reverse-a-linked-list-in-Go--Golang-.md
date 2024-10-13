<!--yml

类别: 未分类

日期: 2024-10-13 06:42:59

-->

# 在Go (Golang)中反转链表

> 来源：[https://golangbyexample.com/reverse-linked-list-golang/](https://golangbyexample.com/reverse-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

目标是反转给定的链表。

示例

```
Input:  3->2->1
Output: 1->2->3
```

## **程序**

以下是相应的程序

```
package main

import "fmt"

func main() {
	first := initList()
	first.AddFront(1)
	first.AddFront(2)
	first.AddFront(3)
	first.AddFront(4)

	first.Head.Traverse()
	first.Reverse()
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
```

**输出**

```
4
3
2
1

1
2
3
4
```

**注意：** 请查看我们的Golang高级教程。本系列的教程内容详尽，力求涵盖所有概念和示例。此教程适合希望获得专业知识和扎实理解的Golang学习者 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在Golang中实现。如果是的话，那么这篇文章就是为你准备的 – [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
