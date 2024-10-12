<!--yml
category: 未分类
date: 2024-10-13 06:37:07
-->

# Delete a kth node from back in a Singly Linked List in Go (Golang)

> 来源：[https://golangbyexample.com/delete-kth-node-back-linked-list-golang/](https://golangbyexample.com/delete-kth-node-back-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")
*   [Explanation](#Explanation "Explanation")*  *## **Overview**

Delete a kth node from the back in a Singly Linked List

**Input linked list: **

```
A-> B-> C-> D-> E-> F-> Null
```

Node to be removed is 3rd from the back, then

**Output linked list: **

```
A-> B-> C-> E-> F-> Null
```

## **Program**

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

**Output**

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

## **Explanation**

We created a linked list as below

```
A-> B-> C-> D-> E-> F-> Null
```

Then we remove the 5th node from the back which is **B**. We traverse the linked list. As you can see from the output that **B** is removed

```
A
C
D
E
F
```

Then we remove the 1st node from the back which is **F**. We traverse the linked list. As you can see from the output that **F** is removed

```
A
C
D
E
```

Then we try to delete a node from the list from a place greater than the linked list size. It gives below error

```
Err: Given number is greater than linked list length
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*