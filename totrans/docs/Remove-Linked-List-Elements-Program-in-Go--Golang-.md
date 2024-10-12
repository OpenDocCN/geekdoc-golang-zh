<!--yml
category: 未分类
date: 2024-10-13 06:52:00
-->

# Remove Linked List Elements Program in Go (Golang)

> 来源：[https://golangbyexample.com/remove-linked-list-elements-golang/](https://golangbyexample.com/remove-linked-list-elements-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *# **Overview**

Given a linked list and a value, delete all nodes from the linked list whose value is equal to the given value.

**Example 1**

```
Input: [1, 2, 1, 3, 6], 1
Output: [2, 3, 6]
```

**Example 2**

```
Input: [2, 2, 3], 2
Output: [3]
```

# **Program**

Below is the program for the same

```
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

**Output**

```
Resultant First List
2
3
6

Resultant Second List
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you – [All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

Also, check out our system design tutorial series here – [System Design Tutorial Series](https://techbyexample.com/system-design-questions/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*