<!--yml

分类：未分类

日期：2024-10-13 06:36:17

-->

# 使用 Go (Golang) 将单链表转换为循环链表

> 来源：[https://golangbyexample.com/single-linked-list-circular-golang/](https://golangbyexample.com/single-linked-list-circular-golang/)

![](img/2bc3aa339263d40466019d041011a04e.png)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

使用 Golang 将单链表转换为循环链表

**输入单链表：**

```go
"A" -> "B" -> "C" -> "D"
```

**输出应该是一个循环链表：**

```go
"A" -> "B" -> "C" -> "D"
 ^                               |
|____________________|
```

## **程序**

本程序中有两个重要方法

+   **ToCircular** – 将单链表转换为循环链表

+   **IsCircular** – 检查一个链表是否为循环链表

```go
package main

import "fmt"

type node struct {
	data string
	next *node
}

type singlyLinkedList struct {
	len  int
	head *node
}

func initList() *singlyLinkedList {
	return &singlyLinkedList{}
}

func (s *singlyLinkedList) AddFront(data string) {
	node := &node{
		data: data,
	}

	if s.head == nil {
		s.head = node
	} else {
		node.next = s.head
		s.head = node
	}
	s.len++
	return
}

func (s *singlyLinkedList) Traverse() error {
	if s.head == nil {
		return fmt.Errorf("TraverseError: List is empty")
	}
	current := s.head
	for current != nil {
		fmt.Println(current.data)
		current = current.next
	}
	return nil
}

//Function to convert singly linked list to circular linked list
func (s *singlyLinkedList) ToCircular() {
	current := s.head
	for current.next != nil {
		current = current.next
	}
	current.next = s.head
}

func (s *singlyLinkedList) IsCircular() bool {
	if s.head == nil {
		return true
	}
	current := s.head.next
	for current.next != nil && current != s.head {
		current = current.next
	}
	return current == s.head
}

func main() {
	singleList := initList()
	fmt.Printf("AddFront: D\n")
	singleList.AddFront("D")
	fmt.Printf("AddFront: C\n")
	singleList.AddFront("C")
	fmt.Printf("AddFront: B\n")
	singleList.AddFront("B")
	fmt.Printf("AddFront: A\n")
	singleList.AddFront("A")

	err := singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	fmt.Printf("Size: %d\n", singleList.len)
	singleList.ToCircular()

	isCircular := singleList.IsCircular()
	fmt.Printf("Is Circular: %t\n", isCircular)
}
```

**输出**

```go
AddFront: D
AddFront: C
AddFront: B
AddFront: A
A
B
C
D
Size: 4
Is Circular: true
```

+   [循环链表](https://golangbyexample.com/tag/circular-linked-list/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [单链表](https://golangbyexample.com/tag/single-linked-list/)*
