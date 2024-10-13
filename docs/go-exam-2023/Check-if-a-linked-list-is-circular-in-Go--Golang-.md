<!--yml

类别：未分类

日期：2024-10-13 06:36:23

-->

# 在Go（Golang）中检查链表是否是循环链表

> 来源：[https://golangbyexample.com/linked-list-is-circular-go/](https://golangbyexample.com/linked-list-is-circular-go/)

![链表 - 是否是循环链表图像](img/fd1244a69df50e7bcb299f396b6f796a.png)

目录

**   [概述](#Overview "Overview")

+   [程序](#Program "Program")*  *## **概述**

检查链表是否是循环链表。如果所有节点以循环的形式连接，则链表是循环的。

## **程序**

在下面的程序中，我们首先创建一个链表。然后检查它是否是循环链表。它会先打印false。之后，我们将链表转换为循环链表。然后再次检查它是否是循环链表。现在打印true。

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
    isCircular := singleList.IsCircular()
    fmt.Printf("Before: Is Circular: %t\n", isCircular)

    fmt.Printf("Size: %d\n", singleList.len)
    singleList.ToCircular()

    isCircular = singleList.IsCircular()
    fmt.Printf("After: Is Circular: %t\n", isCircular)
}
```

**输出**

```
AddFront: D
AddFront: C
AddFront: B
AddFront: A
A
B
C
D
Before: Is Circular: false
Size: 4
After: Is Circular: true
```

+   [循环](https://golangbyexample.com/tag/circular/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [单链表](https://golangbyexample.com/tag/single/)*
