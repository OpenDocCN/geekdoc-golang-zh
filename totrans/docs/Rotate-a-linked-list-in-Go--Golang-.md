<!--yml
category: 未分类
date: 2024-10-13 06:46:39
-->

# Rotate a linked list in Go (Golang)

> 来源：[https://golangbyexample.com/rotate-linked-list-golang/](https://golangbyexample.com/rotate-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to rotate a given list in the right direction by a given number of places.

Example

```
Input: 1->2->3->4->5
k: 2

Output: 4->5->1->2->3
```

## **Program**

Here is the program for the same.

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

**Output**

```
12345
45123
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*