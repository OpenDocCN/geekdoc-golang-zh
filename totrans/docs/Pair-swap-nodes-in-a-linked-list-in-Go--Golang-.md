<!--yml
category: 未分类
date: 2024-10-13 06:43:19
-->

# Pair swap nodes in a linked list in Go (Golang)

> 来源：[https://golangbyexample.com/pair-swap-nodes-linked-list-go/](https://golangbyexample.com/pair-swap-nodes-linked-list-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Given a linked list and a number k reverse nodes of the linked list in groups of k.

For eg

```
Input: 1->2->3->4->5->6->7
k: 3
Output: 3->2->1->6->5->4->7
```

If the length of the  last group of the linked is less than k, then leave the last group as it is

## **Program**

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

**Output**

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

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*