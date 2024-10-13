<!--yml

分类：未分类

日期：2024-10-13 06:43:14

-->

# 在Go语言中，针对给定链表按k组反转节点

> 来源：[https://golangbyexample.com/reverse-nodes-k-group-linked-list-golang/](https://golangbyexample.com/reverse-nodes-k-group-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

给定一个链表和一个数字k，按k组反转链表中的节点。

例如

```
Input: 1->2->3->4->5->6->7
k: 3
Output: 3->2->1->6->5->4->7
```

如果链表最后一组的长度小于k，则保留最后一组不变

## **程序**

```
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

```
1
2
3
4

3
2
1
4
```

**注意：** 请查看我们的Golang高级教程。本系列教程内容详尽，我们尝试用例子覆盖所有概念。此教程适合希望获得Golang专业知识和扎实理解的人 - [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解如何在Golang中实现所有设计模式。如果是，那么这篇文章就是为你准备的 - [所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
