<!--yml
category: 未分类
date: 2024-10-13 06:37:56
-->

# Doubly Linked List in Go (Golang)

> 来源：[https://golangbyexample.com/doubly-linked-list-golang/](https://golangbyexample.com/doubly-linked-list-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

A Doubly Linked List contains three fields in its node. 

*   Data field 
*   One Next pointer points to the next node in the list
*   One Previous pointer which points to the previous node in the list

Here **“Data”** and **“Next”** fields are the same as in the case of a singly linked list. **“Previous”** pointer field is new which makes a linked list a doubly linked list.

Refer to this article to learn about single linked list

[https://golangbyexample.com/singly-linked-list-in-golang/](https://golangbyexample.com/singly-linked-list-in-golang/)

Below is an example of a doubly linked list. Prev pointer of head (start) node points to Null. Similarly, the **Next** pointer of the last node points to Null.

![double linked list](img/e89fb825a0cb856cfc39da333a6770e2.png)

## **Program**

To implement a doubly linked list in Go language, create a node struct with **data**, **prev** pointer, and **next** pointer, methods to add nodes in the doubly linked list (from the front end from the back end both), and methods to traverse forward and reverse.

```
package main

import "fmt"

type node struct {
	data string
	prev *node
	next *node
}

type doublyLinkedList struct {
	len  int
	tail *node
	head *node
}

func initDoublyList() *doublyLinkedList {
	return &doublyLinkedList{}
}

func (d *doublyLinkedList) AddFrontNodeDLL(data string) {
	newNode := &node{
		data: data,
	}
	if d.head == nil {
		d.head = newNode
		d.tail = newNode
	} else {
		newNode.next = d.head
		d.head.prev = newNode
		d.head = newNode
	}
	d.len++
	return
}

func (d *doublyLinkedList) AddEndNodeDLL(data string) {
	newNode := &node{
		data: data,
	}
	if d.head == nil {
		d.head = newNode
		d.tail = newNode
	} else {
		currentNode := d.head
		for currentNode.next != nil {
			currentNode = currentNode.next
		}
		newNode.prev = currentNode
		currentNode.next = newNode
		d.tail = newNode
	}
	d.len++
	return
}
func (d *doublyLinkedList) TraverseForward() error {
	if d.head == nil {
		return fmt.Errorf("TraverseError: List is empty")
	}
	temp := d.head
	for temp != nil {
		fmt.Printf("value = %v, prev = %v, next = %v\n", temp.data, temp.prev, temp.next)
		temp = temp.next
	}
	fmt.Println()
	return nil
}

func (d *doublyLinkedList) TraverseReverse() error {
	if d.head == nil {
		return fmt.Errorf("TraverseError: List is empty")
	}
	temp := d.tail
	for temp != nil {
		fmt.Printf("value = %v, prev = %v, next = %v\n", temp.data, temp.prev, temp.next)
		temp = temp.prev
	}
	fmt.Println()
	return nil
}

func (d *doublyLinkedList) Size() int {
	return d.len
}
func main() {
	doublyList := initDoublyList()
	fmt.Printf("Add Front Node: C\n")
	doublyList.AddFrontNodeDLL("C")
	fmt.Printf("Add Front Node: B\n")
	doublyList.AddFrontNodeDLL("B")
	fmt.Printf("Add Front Node: A\n")
	doublyList.AddFrontNodeDLL("A")
	fmt.Printf("Add End Node: D\n")
	doublyList.AddEndNodeDLL("D")
	fmt.Printf("Add End Node: E\n")
	doublyList.AddEndNodeDLL("E")

	fmt.Printf("Size of doubly linked ist: %d\n", doublyList.Size())

	err := doublyList.TraverseForward()
	if err != nil {
		fmt.Println(err.Error())
	}

	err = doublyList.TraverseReverse()
	if err != nil {
		fmt.Println(err.Error())
	}
}
```

**Output:**

```
Add Front Node: C
Add Front Node: B
Add Front Node: A
Add End Node: D
Add End Node: E
Size of doubly linked ist: 5
value = A, prev = <nil>, next = &{B 0xc000070060 0xc000070020}
value = B, prev = &{A <nil> 0xc000070040}, next = &{C 0xc000070040 0xc000070080}
value = C, prev = &{B 0xc000070060 0xc000070020}, next = &{D 0xc000070020 0xc0000700a0}
value = D, prev = &{C 0xc000070040 0xc000070080}, next = &{E 0xc000070080 <nil>}
value = E, prev = &{D 0xc000070020 0xc0000700a0}, next = <nil>

value = E, prev = &{D 0xc000070020 0xc0000700a0}, next = <nil>
value = D, prev = &{C 0xc000070040 0xc000070080}, next = &{E 0xc000070080 <nil>}
value = C, prev = &{B 0xc000070060 0xc000070020}, next = &{D 0xc000070020 0xc0000700a0}
value = B, prev = &{A <nil> 0xc000070040}, next = &{C 0xc000070040 0xc000070080}
value = A, prev = <nil>, next = &{B 0xc000070060 0xc000070020}</nil></nil></nil></nil></nil></nil></nil></nil>
```*