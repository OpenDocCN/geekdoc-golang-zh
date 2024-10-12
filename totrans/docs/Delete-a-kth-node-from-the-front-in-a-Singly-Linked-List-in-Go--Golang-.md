<!--yml
category: 未分类
date: 2024-10-13 06:37:02
-->

# Delete a kth node from the front in a Singly Linked List in Go (Golang)

> 来源：[https://golangbyexample.com/kth-node-front-linked-list-golang/](https://golangbyexample.com/kth-node-front-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")
*   [Explanation](#Explanation "Explanation")*  *## **Overview**

Delete a kth node from the front in a Singly Linked List

**Input linked list: **

```
A-> B-> C-> D-> E-> F-> Null
```

Let’s say the node to be removed is 3rd from the front, then

**Output linked list: **

```
A-> B-> D-> E-> F-> Null
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

## **Explanation**

We created a linked list as below

```
A-> B-> C-> D-> E-> F-> Null
```

Then we remove the 2nd node from the front which is **B**. We traverse the linked list. As you can see from the output that **B** is removed

```
A
C
D
E
F
```

Then we remove the 5th node from the front which is **F**. We traverse the linked list. As you can see from the output that **F** is removed

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

Also, check out our Golang advance tutorial Series – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)*