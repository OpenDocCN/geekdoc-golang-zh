<!--yml

分类：未分类

日期：2024-10-13 06:37:02

-->

# 从单链表的前面删除第k个节点，使用Go (Golang)

> 来源：[https://golangbyexample.com/kth-node-front-linked-list-golang/](https://golangbyexample.com/kth-node-front-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")

+   [解释](#Explanation "Explanation") *  *## **概述**

从单链表的前面删除第k个节点

**输入链表：**

```go
A-> B-> C-> D-> E-> F-> Null
```

假设要移除的节点是前面的第3个节点，那么

**输出链表：**

```go
A-> B-> D-> E-> F-> Null
```

## **程序**

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

func (s *singlyLinkedList) Removekth(k int) error {
	if s.head == nil {
		return fmt.Errorf("List is empty")
	}
	if k > s.len {
		return fmt.Errorf("Err: Given number is greater than linked list length")
	}
	if k == 1 {
		fmt.Println("Node to be removed is front node")
		s.head = s.head.next
	} else {
		var prev *node
		current := s.head
		for i := 1; i < k; i++ {
			prev = current
			current = current.next
		}
		prev.next = current.next
	}
	s.len--
	return nil
}

func main() {
	// Initiaise singly linked list
	singleList := initList()
	// Adding nodes in linked list from front
	fmt.Printf("AddFront: F\n")
	singleList.AddFront("F")
	fmt.Printf("AddFront: E\n")
	singleList.AddFront("E")
	fmt.Printf("AddFront: D\n")
	singleList.AddFront("D")
	fmt.Printf("AddFront: C\n")
	singleList.AddFront("C")
	fmt.Printf("AddFront: B\n")
	singleList.AddFront("B")
	fmt.Printf("AddFront: A\n")
	singleList.AddFront("A")

	fmt.Println("Traversal")
	err := singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	fmt.Println("\nRemoving 2nd node from the front which is B")
	err = singleList.Removekth(2)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println("Traversal after 2nd node is removed from the front")
	err = singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	fmt.Println("\nRemoving 5th node from the front which is F now")
	err = singleList.Removekth(5)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println("Traversal after 5th node is removed from the front")
	err = singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	// Trying to delete node from a place greater than list size
	fmt.Println("\nTrying to delete node from a place greater than list size")
	err = singleList.Removekth(5)
	if err != nil {
		fmt.Println(err.Error())
	}

}
```

**输出**

```go
AddFront: F
AddFront: E
AddFront: D
AddFront: C
AddFront: B
AddFront: A
Traversal
A
B
C
D
E
F

Removing 2nd node from the front which is B
Traversal after 2nd node is removed from the front
A
C
D
E
F

Removing 5th node from the front which is F now
Traversal after 5th node is removed from the front
A
C
D
E

Trying to delete node from a place greater than list size
Err: Given number is greater than linked list length
```

## **解释**

我们创建了如下链表

```go
A-> B-> C-> D-> E-> F-> Null
```

然后我们移除前面的第2个节点，即**B**。我们遍历链表。从输出中可以看出**B**被移除。

```go
A
C
D
E
F
```

然后我们移除前面的第5个节点，即**F**。我们遍历链表。从输出中可以看出**F**被移除。

```go
A
C
D
E
```

然后我们尝试从链表中删除一个节点，位置超过链表的大小。这会产生以下错误。

```go
Err: Given number is greater than linked list length
```

另外，查看我们的Golang高级教程系列 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/) *
