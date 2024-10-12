<!--yml
category: 未分类
date: 2024-10-13 06:41:52
-->

# Add two numbers represented by linked list in Go (Golang)

> 来源：[https://golangbyexample.com/add-numbers-linked-list-golang/](https://golangbyexample.com/add-numbers-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")*  *## **Overview**

Assume that both the linked list is already reversed. Meaning that let’s say the numbers are

*   478

*   339

The output should be **817**

Assume both numbers are represented as a linked list in a reverse manner

```
8->7->4
9->3->3
```

And expected output linked list is also reversed

```
7->1->8
```

Here is the program for the same

```
package main

import "fmt"

func main() {
	first := initList()	
        first.AddFront(4)
	first.AddFront(7)
	first.AddFront(8)

	second := initList()
	second.AddFront(3)
	second.AddFront(3)
	second.AddFront(9)

	result := addTwoNumbers(first.Head, second.Head)
	result.Traverse()
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

func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	var prev *ListNode
	var result *ListNode
	carry := 0

	for l1 != nil && l2 != nil {

		sum := l1.Val + l2.Val + carry

		carry = sum / 10
		if sum >= 10 {
			sum = sum % 10
		}

		tempNode := &ListNode{Val: sum}

		if prev == nil {
			result = tempNode
			prev = tempNode
		} else {
			prev.Next = tempNode
			prev = prev.Next
		}

		l1 = l1.Next

		l2 = l2.Next
	}

	for l1 != nil {
		sum := l1.Val + carry
		carry = sum / 10
		if sum >= 10 {
			sum = sum % 10
		}
		tempNode := &ListNode{Val: sum}
		prev.Next = tempNode
		l1 = l1.Next
	}

	for l2 != nil {
		sum := l2.Val + carry
		carry = sum / 10
		if sum >= 10 {
			sum = sum % 10
		}
		tempNode := &ListNode{Val: sum}
		prev.Next = tempNode
		l2 = l2.Next
	}

	if carry > 0 {
		tempNode := &ListNode{Val: carry}
		prev.Next = tempNode
	}
	return result
}
```

**Output**

```
7
1
8
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*