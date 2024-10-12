<!--yml
category: 未分类
date: 2024-10-13 06:05:32
-->

# Linked List in Golang

> 来源：[https://golangbyexample.com/singly-linked-list-in-golang/](https://golangbyexample.com/singly-linked-list-in-golang/)

Singly-linked list is a simple kind of linked list that allows traversal in one direction i.e forward. Each node in the linked list contains the data part and the pointer to the next node in the linked list.

The linked list implemented below supports the following operations

1.  AddFront
2.  AddBack
3.  RemoveFront
4.  RemoveBack
5.  Traverse
6.  Front
7.  Size

```
package main

import "fmt"

type ele struct {
    name string
    next *ele
}

type singleList struct {
    len  int
    head *ele
}

func initList() *singleList {
    return &singleList{}
}

func (s *singleList) AddFront(name string) {
    ele := &ele{
        name: name,
    }
    if s.head == nil {
        s.head = ele
    } else {
        ele.next = s.head
        s.head = ele
    }
    s.len++
    return
}

func (s *singleList) AddBack(name string) {
    ele := &ele{
        name: name,
    }
    if s.head == nil {
        s.head = ele
    } else {
        current := s.head
        for current.next != nil {
            current = current.next
        }
        current.next = ele
    }
    s.len++
    return
}

func (s *singleList) RemoveFront() error {
    if s.head == nil {
        return fmt.Errorf("List is empty")
    }
    s.head = s.head.next
    s.len--
    return nil
}

func (s *singleList) RemoveBack() error {
    if s.head == nil {
        return fmt.Errorf("removeBack: List is empty")
    }
    var prev *ele
    current := s.head
    for current.next != nil {
        prev = current
        current = current.next
    }
    if prev != nil {
        prev.next = nil
    } else {
        s.head = nil
    }
    s.len--
    return nil
}

func (s *singleList) Front() (string, error) {
    if s.head == nil {
        return "", fmt.Errorf("Single List is empty")
    }
    return s.head.name, nil
}

func (s *singleList) Size() int {
    return s.len
}

func (s *singleList) Traverse() error {
    if s.head == nil {
        return fmt.Errorf("TranverseError: List is empty")
    }
    current := s.head
    for current != nil {
        fmt.Println(current.name)
        current = current.next
    }
    return nil
}

func main() {
     singleList := initList()
    fmt.Printf("AddFront: A\n")
    singleList.AddFront("A")
    fmt.Printf("AddFront: B\n")
    singleList.AddFront("B")
    fmt.Printf("AddBack: C\n")
    singleList.AddBack("C")

    fmt.Printf("Size: %d\n", singleList.Size())

    err := singleList.Traverse()
    if err != nil {
        fmt.Println(err.Error())
    }

    fmt.Printf("RemoveFront\n")
    err = singleList.RemoveFront()
    if err != nil {
        fmt.Printf("RemoveFront Error: %s\n", err.Error())
    }

    fmt.Printf("RemoveBack\n")
    err = singleList.RemoveBack()
    if err != nil {
        fmt.Printf("RemoveBack Error: %s\n", err.Error())
    }

    fmt.Printf("RemoveBack\n")
    err = singleList.RemoveBack()
    if err != nil {
        fmt.Printf("RemoveBack Error: %s\n", err.Error())
    }

    fmt.Printf("RemoveBack\n")
    err = singleList.RemoveBack()
    if err != nil {
        fmt.Printf("RemoveBack Error: %s\n", err.Error())
    }

    err = singleList.Traverse()
    if err != nil {
        fmt.Println(err.Error())
    }

   fmt.Printf("Size: %d\n", singleList.Size())
}
```

**Output:**

```
AddFront: A
AddFront: B
AddBack: C
Size: 3
B
A
C
RemoveFront
RemoveBack
RemoveBack
RemoveBack
RemoveBack Error: removeBack: List is empty
TranverseError: List is empty
Size: 0
```

*   [data](https://golangbyexample.com/tag/data/)*   [go](https://golangbyexample.com/tag/go/)*   [linked](https://golangbyexample.com/tag/linked/)*   [single linked list](https://golangbyexample.com/tag/single-linked-list/)*   [singly](https://golangbyexample.com/tag/singly/)*   [structure](https://golangbyexample.com/tag/structure/)