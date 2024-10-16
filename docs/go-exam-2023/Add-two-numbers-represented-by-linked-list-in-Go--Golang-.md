<!--yml

类别：未分类

日期：2024-10-13 06:41:52

-->

# 用 Go（Golang）实现两个由链表表示的数字相加

> 来源：[`golangbyexample.com/add-numbers-linked-list-golang/`](https://golangbyexample.com/add-numbers-linked-list-golang/)

目录

+   概述

## **概述**

假设链表已经被反转。也就是说，假设这两个数字是

+   478

+   339

输出应该是 **817**

假设这两个数字以反向方式表示为链表

```go
8->7->4
9->3->3
```

期望的输出链表也是反向的

```go
7->1->8
```

这是相应的程序

```go
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

**输出**

```go
7
1
8
```


