<!--yml

分类：未分类

日期：2024-10-13 06:38:03

-->

# 在Go中反转双向链表 (Golang)

> 来源：[https://golangbyexample.com/reverse-doubly-linked-list-golang/](https://golangbyexample.com/reverse-doubly-linked-list-golang/)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

双向链表可以通过以下两种方法反转：

+   通过交换节点的前后指针。

+   通过使用栈

在本教程中，我们将介绍第一种方法，即通过交换前后指针。

假设我们有以下双向链表

![dll_reverse1](img/370305dc6f9fcbe0b14eb0aec8026c81.png)

反转后，双向链表将如下所示：

![](img/fdb6f3f034a9dadb562eea9beb6a5886.png)

## **程序**

在这种方法中，我们需要注意以下几点：

+   交换双向链表的头和尾

+   交换所有节点的前后指针

```go
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
func (d *doublyLinkedList) Size() int {
    return d.len
}
func (d *doublyLinkedList) ReverseDLL() {
    currentNode := d.head
    var nextInList *node
    d.head, d.tail = d.tail, d.head
    for currentNode != nil {
        nextInList = currentNode.next
        currentNode.next, currentNode.prev = currentNode.prev, currentNode.next
        currentNode = nextInList
    }
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
    fmt.Println("Reversing Doubly Linked List")
    doublyList.ReverseDLL()
    fmt.Printf("Size of doubly linked ist: %d\n", doublyList.Size())
    err = doublyList.TraverseForward()
    if err != nil {
        fmt.Println(err.Error())
    }
}
```

**输出**

```go
Add Front Node: C
Add Front Node: B
Add Front Node: A
Add End Node: D
Add End Node: E
Size of doubly linked ist: 5
value = A, prev = <nil>, next = &{B 0xc000070060 0xc000070020}
value = B, prev = &{A <nil>0xc000070040}, next = &{C 0xc000070040 0xc000070080}
value = C, prev = &{B 0xc000070060 0xc000070020}, next = &{D 0xc000070020 0xc0000700a0}
value = D, prev = &{C 0xc000070040 0xc000070080}, next = &{E 0xc000070080 <nil>}
value = E, prev = &{D 0xc000070020 0xc0000700a0}, next = <nil>Reversing Doubly Linked List
Size of doubly linked ist: 5
value = E, prev = <nil>, next = &{D 0xc0000700a0 0xc000070020}
value = D, prev = &{E <nil>0xc000070080}, next = &{C 0xc000070080 0xc000070040}
value = C, prev = &{D 0xc0000700a0 0xc000070020}, next = &{B 0xc000070020 0xc000070060}
value = B, prev = &{C 0xc000070080 0xc000070040}, next = &{A 0xc000070040 <nil>}
value = A, prev = &{B 0xc000070020 0xc000070060}, next =</nil></nil></nil></nil></nil></nil></nil> 
```

此外，请查看我们的Golang高级教程系列 – [Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
