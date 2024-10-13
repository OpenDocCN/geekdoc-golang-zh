<!--yml

类别：未分类

日期：2024-10-13 06:05:32

-->

# Golang中的链表

> 来源：[https://golangbyexample.com/singly-linked-list-in-golang/](https://golangbyexample.com/singly-linked-list-in-golang/)

单链表是一种简单的链表，只允许向一个方向遍历，即向前。链表中的每个节点包含数据部分和指向下一个节点的指针。

以下实现的链表支持以下操作：

1.  添加前部

1.  添加后部

1.  移除前部

1.  移除后部

1.  遍历

1.  前部

1.  大小

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

**输出：**

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

+   [数据](https://golangbyexample.com/tag/data/)*   [go](https://golangbyexample.com/tag/go/)*   [链接](https://golangbyexample.com/tag/linked/)*   [单链表](https://golangbyexample.com/tag/single-linked-list/)*   [单一](https://golangbyexample.com/tag/singly/)*   [结构](https://golangbyexample.com/tag/structure/)
