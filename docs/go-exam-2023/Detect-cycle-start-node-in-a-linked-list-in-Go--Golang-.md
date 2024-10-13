<!--yml

类别：未分类

日期：2024-10-13 06:46:48

-->

# 在 Golang 中检测链表的循环起始节点。

> 来源：[`golangbyexample.com/cycle-start-node-linked-list-go/`](https://golangbyexample.com/cycle-start-node-linked-list-go/)

目录

+   概述

+   程序 

## **概述**

目标是找出给定链表中的循环起始节点。如果链表中的最后一个节点指向前面的某个节点，则链表中存在循环。

示例

![](img/37e30f9ef9a9259067409d35628db09d.png)

上述链表存在循环。循环起始节点是节点 2。下面是我们可以遵循的方法。

+   首先，检测给定的链表是否有循环。设置两个指针，一个是慢指针，另一个是快指针。两者最初都指向头节点。

+   现在将慢指针移动 1 个节点，快指针移动 2 个节点。

```go
slow := slow.Next
fast := fast.Next.Next
```

+   如果在任何时刻慢指针和快指针相同，则链表存在循环。

+   快指针和慢指针只能在循环内的节点相遇。假设它们在节点 3 相遇。现在计算循环的长度。长度为 3。

+   现在在节点头部保持一个指针，另一个指针与它保持一个循环长度的距离。因此，一个指针将指向节点 1，另一个指针将指向节点 4。

+   移动两个指针，直到它们相同。它们将在循环起始节点相遇，即节点 2。

## **程序**

这是相应的程序。

```go
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

**输出**

```go
2
```

**注意：** 查看我们的 Golang 高级教程。本系列教程内容详尽，我们尝试涵盖所有概念及示例。本教程适合希望获得专业知识和深入理解 Golang 的读者 - [Golang 高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你有兴趣了解所有设计模式如何在 Golang 中实现。如果是的话，那么这篇文章就是为你准备的 - [所有设计模式 Golang](https://golangbyexample.com/all-design-patterns-golang/)


