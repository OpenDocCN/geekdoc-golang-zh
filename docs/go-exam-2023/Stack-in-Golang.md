<!--yml

分类：未分类

日期：2024-10-13 06:03:48

-->

# Go 中的栈

> 来源：[`golangbyexample.com/stack-in-golang/`](https://golangbyexample.com/stack-in-golang/)

目录

**   介绍

+   列表实现

+   切片实现 

## **介绍**

一个简单的栈可以在 Go 中通过

1.  container/list 包

1.  切片实现

栈将有以下操作：

1.  推入

1.  弹出

1.  前端

1.  大小

1.  空

## **列表实现**

```go
package main

import (
    "container/list"
    "fmt"
)

type customStack struct {
    stack *list.List
}

func (c *customStack) Push(value string) {
    c.stack.PushFront(value)
}

func (c *customStack) Pop() error {
    if c.stack.Len() > 0 {
        ele := c.stack.Front()
        c.stack.Remove(ele)
    }
    return fmt.Errorf("Pop Error: Stack is empty")
}

func (c *customStack) Front() (string, error) {
    if c.stack.Len() > 0 {
        if val, ok := c.stack.Front().Value.(string); ok {
            return val, nil
        }
        return "", fmt.Errorf("Peep Error: Stack Datatype is incorrect")
    }
    return "", fmt.Errorf("Peep Error: Stack is empty")
}

func (c *customStack) Size() int {
    return c.stack.Len()
}

func (c *customStack) Empty() bool {
    return c.stack.Len() == 0
}

func main() {
    customStack := &customStack{
        stack: list.New(),
    }
    fmt.Printf("Push: A\n")
    customStack.Push("A")
    fmt.Printf("Push: B\n")
    customStack.Push("B")
    fmt.Printf("Size: %d\n", customStack.Size())
    for customStack.Size() > 0 {
        frontVal, _ := customStack.Front()
        fmt.Printf("Front: %s\n", frontVal)
        fmt.Printf("Pop: %s\n", frontVal)
        customStack.Pop()
    }
    fmt.Printf("Size: %d\n", customStack.Size())
}
```

**输出：**

```go
Push: A
Push: B
Size: 2
Front: B
Pop: B
Front: A
Pop: A
Size: 0
```

## **切片实现**

```go
package main

import (
    "fmt"
    "sync"
)

type customStack struct {
    stack []string
    lock  sync.RWMutex
}

func (c *customStack) Push(name string) {
    c.lock.Lock()
    defer c.lock.Unlock()
    c.stack = append(c.stack, name)
}

func (c *customStack) Pop() error {
    len := len(c.stack)
    if len > 0 {
        c.lock.Lock()
        defer c.lock.Unlock()
        c.stack = c.stack[:len-1]
        return nil
    }
    return fmt.Errorf("Pop Error: Stack is empty")
}

func (c *customStack) Front() (string, error) {
    len := len(c.stack)
    if len > 0 {
        c.lock.Lock()
        defer c.lock.Unlock()
        return c.stack[len-1], nil
    }
    return "", fmt.Errorf("Peep Error: Stack is empty")
}

func (c *customStack) Size() int {
    return len(c.stack)
}

func (c *customStack) Empty() bool {
    return len(c.stack) == 0
}

func main() {
    customStack := &customStack{
        stack: make([]string, 0),
    }
    fmt.Printf("Push: A\n")
    customStack.Push("A")
    fmt.Printf("Push: B\n")
    customStack.Push("B")
    fmt.Printf("Size: %d\n", customStack.Size())
    for customStack.Size() > 0 {
        frontVal, _ := customStack.Front()
        fmt.Printf("Front: %s\n", frontVal)
        fmt.Printf("Pop: %s\n", frontVal)
        customStack.Pop()
    }
    fmt.Printf("Size: %d\n", customStack.Size())
}
```

**输出：**

```go
Push: A
Push: B
Size: 2
Front: B
Pop: B
Front: A
Pop: A
Size: 0
```

+   [Go 中的数据结构](https://golangbyexample.com/tag/data-structure-in-go/) *   [栈](https://golangbyexample.com/tag/stack/) *   [Go 中的栈](https://golangbyexample.com/tag/stack-in-go/) *
