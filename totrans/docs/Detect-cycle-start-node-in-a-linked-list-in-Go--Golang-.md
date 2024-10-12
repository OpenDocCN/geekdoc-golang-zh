<!--yml
category: 未分类
date: 2024-10-13 06:46:48
-->

# Detect cycle start node in a linked list in Go (Golang)

> 来源：[https://golangbyexample.com/cycle-start-node-linked-list-go/](https://golangbyexample.com/cycle-start-node-linked-list-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

The objective is to find out the cycle start node in a given linked list. A cycle exists in a linked list if  the last node in the linked list points to another node in the front

Example

![](img/37e30f9ef9a9259067409d35628db09d.png)

The linked list above has a cycle. The cycle start node is node 2\. Below is the approach we can follow

*   First, detect whether the given linked list has a cycle or not. Have two pointers. One is the slow pointer and the other is a fast pointer. Both point to the head node initially

*   Now move the slow pointer by 1 node and move the fast pointer by 2 nodes.

```
slow := slow.Next
fast := fast.Next.Next
```

*   If slow and fast pointers are the same at any point in time then the linked list has cyle.

*   The fast pointer and a slow pointer can only meet at a node that is in the cycle. Let’s assume they meet at node 3\. Now get the length of the cycle. The length is 3

*   Then keep one pointer at the head of the node and the other pointer at a distance of cycle length from it. So one pointer will be added at node 1 and another pointer will be at node 4.

*   Move both the pointers until they are the same. They will meet at the cycle start node which is Node 2

## **Program**

Here is the program for the same.

```
package main

import "fmt"

func main() {
	first := initList()
	ele4 := first.AddFront(4)
	first.AddFront(3)
	ele2 := first.AddFront(2)
	first.AddFront(1)

	//Create cycle
	ele4.Next = ele2

	output := cycleStartNode(first.Head)
	fmt.Println(output.Val)

}

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

func initList() *SingleList {
	return &SingleList{}
}
func cycleStartNode(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return nil
	}

	slow := head
	fast := head

	cycleExists := false

	for slow != nil && fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next

		if slow == fast {
			cycleExists = true
			break
		}
	}

	if !cycleExists {
		return nil
	}

	cycleNode := slow

	curr := cycleNode

	lengthCycle := 1

	for curr.Next != cycleNode {
		lengthCycle++
		curr = curr.Next
	}

	curr = head

	for i := 0; i < lengthCycle; i++ {
		curr = curr.Next
	}

	for head != curr {
		head = head.Next
		curr = curr.Next
	}

	return head
}
```

**Output**

```
2
```

**Note:** Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang - [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you -[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*