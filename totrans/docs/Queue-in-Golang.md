<!--yml
category: 未分类
date: 2024-10-13 06:03:42
-->

# Queue in Golang

> 来源：[https://golangbyexample.com/queue-in-golang/](https://golangbyexample.com/queue-in-golang/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [List Implementation](#List_Implementation "List Implementation")
*   [Slice Implementation](#Slice_Implementation "Slice Implementation")*  *# **Introduction**

A simple queue can be implemented in GO using

1.  container/list package
2.  slice

A queue will have below operations:

1.  Enqueue
2.  Dequeue
3.  Front
4.  Size
5.  Empty

# **List Implementation**

```
package main

import (
    "container/list"
    "fmt"
)

type customQueue struct {
    queue *list.List
}

func (c *customQueue) Enqueue(value string) {
    c.queue.PushBack(value)
}

func (c *customQueue) Dequeue() error {
    if c.queue.Len() > 0 {
        ele := c.queue.Front()
        c.queue.Remove(ele)
    }
    return fmt.Errorf("Pop Error: Queue is empty")
}

func (c *customQueue) Front() (string, error) {
    if c.queue.Len() > 0 {
        if val, ok := c.queue.Front().Value.(string); ok {
            return val, nil
        }
        return "", fmt.Errorf("Peep Error: Queue Datatype is incorrect")
    }
    return "", fmt.Errorf("Peep Error: Queue is empty")
}

func (c *customQueue) Size() int {
    return c.queue.Len()
}

func (c *customQueue) Empty() bool {
    return c.queue.Len() == 0
}

func main() {
    customQueue := &customQueue{
        queue: list.New(),
    }
    fmt.Printf("Enqueue: A\n")
    customQueue.Enqueue("A")
    fmt.Printf("Enqueue: B\n")
    customQueue.Enqueue("B")
    fmt.Printf("Size: %d\n", customQueue.Size())
    for customQueue.Size() > 0 {
        frontVal, _ := customQueue.Front()
        fmt.Printf("Front: %s\n", frontVal)
        fmt.Printf("Dequeue: %s\n", frontVal)
        customQueue.Dequeue()
    }
    fmt.Printf("Size: %d\n", customQueue.Size())
}
```

**Output:**

```
Enqueue: A
Enqueue: B
Size: 2
Front: A
Dequeue: A
Front: B
Dequeue: B
Size: 0
```

# **Slice Implementation**

```
package main

import (
	"fmt"
	"sync"
)

type customQueue struct {
	queue []string
	lock  sync.RWMutex
}

func (c *customQueue) Enqueue(name string) {
	c.lock.Lock()
	defer c.lock.Unlock()
	c.queue = append(c.queue, name)
}

func (c *customQueue) Dequeue() error {
	if len(c.queue) > 0 {
		c.lock.Lock()
		defer c.lock.Unlock()
		c.queue = c.queue[1:]
		return nil
	}
	return fmt.Errorf("Pop Error: Queue is empty")
}

func (c *customQueue) Front() (string, error) {
	if len(c.queue) > 0 {
		c.lock.Lock()
		defer c.lock.Unlock()
		return c.queue[0], nil
	}
	return "", fmt.Errorf("Peep Error: Queue is empty")
}

func (c *customQueue) Size() int {
	return len(c.queue)
}

func (c *customQueue) Empty() bool {
	return len(c.queue) == 0
}

func main() {
	customQueue := &customQueue{
		queue: make([]string, 0),
	}

	fmt.Printf("Enqueue: A\n")
	customQueue.Enqueue("A")
	fmt.Printf("Enqueue: B\n")
	customQueue.Enqueue("B")
	fmt.Printf("Len: %d\n", customQueue.Size())

	for customQueue.Size() > 0 {
		frontVal, _ := customQueue.Front()
		fmt.Printf("Front: %s\n", frontVal)
		fmt.Printf("Dequeue: %s\n", frontVal)
		customQueue.Dequeue()
	}
	fmt.Printf("Len: %d\n", customQueue.Size())
} 
```

**Output:**

```
Enqueue: A
Enqueue: B
Size: 2
Front: A
Dequeue: A
Front: B
Dequeue: B
Size: 0
```*