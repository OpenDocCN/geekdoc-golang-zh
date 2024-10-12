<!--yml
category: 未分类
date: 2024-10-13 06:47:32
-->

# Delete middle node of a linked list in Go (Golang)

> 来源：[https://golangbyexample.com/delete-middle-node-linked-list-golang/](https://golangbyexample.com/delete-middle-node-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to remove the middle node of the linked list. If x is the size of the linked list then the middle node is

```
mid = x/2
```

Examples

```
Input: 1->2->3->4->5
Output: 1->2->4->5
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

**Output**

```
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

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*