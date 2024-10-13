<!--yml

分类：未分类

日期：2024-10-13 06:37:07

-->

# 在Go (Golang)中删除单链表的第k个节点

> 来源：[https://golangbyexample.com/delete-kth-node-back-linked-list-golang/](https://golangbyexample.com/delete-kth-node-back-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")

+   [解释](#Explanation "Explanation")*  *## **概述**

在单链表中删除从尾部的第k个节点

**输入链表：**

```
A-> B-> C-> D-> E-> F-> Null
```

要移除的节点是从尾部的第3个节点，然后

**输出链表：**

```
A-> B-> C-> E-> F-> Null
```

## **程序**

```
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
		return fmt.Errorf("TraerseList: List is empty")
	} else {
		current := s.head
		for current != nil {
			fmt.Println(current.data)
			current = current.next
		}
	}
	return nil
}

func (s *singlyLinkedList) RemovekthFromEnd(k int) error {
	if s.head == nil {
		return fmt.Errorf("List is empty")
	}
	if k > s.len {
		return fmt.Errorf("Err: Given number is greater than linked list length")
	}

	if k == s.len {
		s.head = s.head.next
	} else {
		var prev *node
		current := s.head
		for i := 1; i < s.len-k+1; i++ {
			prev = current
			current = current.next
		}
		prev.next = current.next
	}
	s.len--
	return nil
}

func main() {
	singleList := initList()
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

	// Remove 5th node from back
	fmt.Println("\nRemoving 5th node from the end")
	err = singleList.RemovekthFromEnd(5)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println("Traversal after 5th node is removed from the end")
	err = singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	// Remove first node from back
	fmt.Println("\nRemoving 1st node from the end")
	err = singleList.RemovekthFromEnd(1)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println("Traversal after 1st node is removed from the end")
	err = singleList.Traverse()
	if err != nil {
		fmt.Println(err.Error())
	}

	// Trying to delete node from a place greater than list size
	fmt.Println("\nTrying to delete node from a place greater than list size")
	err = singleList.RemovekthFromEnd(7)
	if err != nil {
		fmt.Println(err.Error())
	}
}
```

**输出**

```
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

Removing 5th node from the end
Traversal after 5th node is removed from the end
A
C
D
E
F

Removing 1st node from the end
Traversal after 1st node is removed from the end
A
C
D
E

Trying to delete a node from a place greater than list size
Err: Given number is greater than linked list length
```

## **解释**

我们创建了如下的链表。

```
A-> B-> C-> D-> E-> F-> Null
```

然后我们从尾部移除第5个节点，即**B**。我们遍历链表。从输出中可以看到**B**被移除了。

```
A
C
D
E
F
```

然后我们从尾部移除第1个节点，即**F**。我们遍历链表。从输出中可以看到**F**被移除了。

```
A
C
D
E
```

然后我们尝试从链表中删除一个位置大于链表大小的节点。这会产生以下错误。

```
Err: Given number is greater than linked list length
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
