<!--yml
category: 未分类
date: 2024-10-13 06:48:19
-->

# Partition a linked list in Go (Golang)

> 来源：[https://golangbyexample.com/partition-linked-list-golang/](https://golangbyexample.com/partition-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

A linked list is given. Also, a target value is given. Partition the given linked list in such a way all values less than target values comes before all the values that are greater than the target value

Example

```
Input: 4->5->3->1
Output: 2
Target: 1->4->5->3
```

The original order should be preserved as well

## **Program**

Here is the program for the same.

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

**Output**

```
4
5
3
1

1
4
5
3
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*