<!--yml

类别：未分类

日期：2024-10-13 06:44:58

-->

# Go (Golang)中的LRU缓存实现。

> 来源：[https://golangbyexample.com/lru-cache-implementation-golang/](https://golangbyexample.com/lru-cache-implementation-golang/)

目录。

**[概述](#Overview "Overview")**

    +   [实现细节](#Implementation_Details "Implementation Details")

    +   [Set(key int, value int)](#Setkey_int_value_int "Set(key int, value int)")

    +   [Get(key int)](#Getkey_int "Get(key int) ")

+   [程序](#Program "Program")

+   [结论](#Conclusion "Conclusion")*  *## **概述**

目标是实现一个缓存。

+   它应该支持**Set**和**Get**操作。

+   **Set**和**Get**的时间复杂度为O(1)。

+   假设缓存的最大容量为3。当缓存已满并且需要插入一个新键时，则需要从缓存中删除现有条目之一。

+   删除应基于驱逐算法——**LRU**。

### **实现细节**

+   我们将使用**映射**和**双向链表**来存储所有内容。使用映射和双向链表的目的是确保即使在驱逐情况下，**get**和**set**操作也是O(1)。

+   **映射**的键为字符串类型，值为指向**双向链表**中节点的指针类型。

+   **双向链表**的每个节点将包含键和值。每个节点还将有一个指向双向链表中前一个节点的指针和一个指向下一个节点的指针。

让我们看看**Get**和**Set**如何在O(1)时间内工作。

### **Set(key int, value int)**

对于任何set操作，它将首先创建一个包含给定键和值的双向链表节点。然后在映射中以输入键作为键，节点地址作为值进行条目插入。一旦节点创建，就有两种情况。

+   缓存未满——在这种情况下，它将控制权传递给当前的驱逐算法LRU。LRU算法将在双向链表的末尾插入该节点。前面的节点是最近最少使用的节点。这里每个操作都是O(1)。

+   缓存已满——在这种情况下，它将控制权传递给当前的驱逐算法LRU。它将驱逐前面的最近最少使用的节点。一旦该节点被驱逐，就会在末尾插入新节点。这里每个操作都是O(1)。

### **Get(key int)**

对于任何Get操作，它首先检查映射中给定的键是否存在。如果存在，则获取映射中该键指向的节点的地址。然后从节点中获取值。接下来，它将控制权传递给当前的驱逐算法LRU。LRU算法将在双向链表的末尾移动当前节点。前面的节点是最近最少使用的节点，保持该节点在末尾。这里每个操作都是O(1)。

## **程序**

这是用 Go 编程语言编写的完整工作代码，供有兴趣的人参考。

**doublylinklist.go**

```
package main

import "fmt"

type node struct {
	key   string
	value string
	prev  *node
	next  *node
}

type doublyLinkedList struct {
	len  int
	tail *node
	head *node
}

func initDoublyList() *doublyLinkedList {
	return &doublyLinkedList{}
}

func (d *doublyLinkedList) AddToFront(key, value string) {
	newNode := &node{
		key:   key,
		value: value,
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

func (d *doublyLinkedList) RemoveFromFront() {
	if d.head == nil {
		return
	} else if d.head == d.tail {
		d.head = nil
		d.tail = nil
	} else {
		d.head = d.head.next
	}
	d.len--
}

func (d *doublyLinkedList) AddToEnd(node *node) {
	newNode := node
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
func (d *doublyLinkedList) Front() *node {
	return d.head
}

func (d *doublyLinkedList) MoveNodeToEnd(node *node) {
	prev := node.prev
	next := node.next

	if prev != nil {
		prev.next = next
	}

	if next != nil {
		next.prev = prev
	}
	if d.tail == node {
		d.tail = prev
	}
	if d.head == node {
		d.head = next
	}
	node.next = nil
	node.prev = nil
	d.len--
	d.AddToEnd(node)
}

func (d *doublyLinkedList) TraverseForward() error {
	if d.head == nil {
		return fmt.Errorf("TraverseError: List is empty")
	}
	temp := d.head
	for temp != nil {
		fmt.Printf("key = %v, value = %v, prev = %v, next = %v\n", temp.key, temp.value, temp.prev, temp.next)
		temp = temp.next
	}
	fmt.Println()
	return nil
}

func (d *doublyLinkedList) Size() int {
	return d.len
}
```

**evictionAlgorithm.go**

```
package main

type evictionAlgo interface {
	evict(c *Cache) string
	get(node *node, c *Cache)
	set(node *node, c *Cache)
	set_overwrite(node *node, value string, c *Cache)
}

func createEvictioAlgo(algoType string) evictionAlgo {
	if algoType == "fifo" {
		return &fifo{}
	} else if algoType == "lru" {
		return &lru{}
	}

	return nil
}
```

**lru.go**

```
package main

import "fmt"

type lru struct {
}

func (l *lru) evict(c *Cache) string {
	key := c.doublyLinkedList.Front().key
	fmt.Printf("Evicting by lru strtegy. Evicted Node Key: %s: ", key)
	c.doublyLinkedList.RemoveFromFront()
	return key
}

func (l *lru) get(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to get operation")
	c.doublyLinkedList.MoveNodeToEnd(node)
}

func (l *lru) set(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set operation")
	c.doublyLinkedList.AddToEnd(node)
}

func (l *lru) set_overwrite(node *node, value string, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set_overwrite operation")
	node.value = value
	c.doublyLinkedList.MoveNodeToEnd(node)
}
```

**fifo.go**

```
package main

import "fmt"

type fifo struct {
}

func (l *fifo) evict(c *Cache) string {
	fmt.Println("Evicting by fifo strtegy")
	key := c.doublyLinkedList.Front().key
	c.doublyLinkedList.RemoveFromFront()
	return key
}

func (l *fifo) get(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to get operation")
}

func (l *fifo) set(node *node, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set operation")
	c.doublyLinkedList.AddToEnd(node)
}

func (l *fifo) set_overwrite(node *node, value string, c *Cache) {
	fmt.Println("Shuffling doubly linked list due to set_overwrite operation")
}
```

**cache.go**

```
package main

import "fmt"

type Cache struct {
	doublyLinkedList *doublyLinkedList
	storage          map[string]*node
	evictionAlgo     evictionAlgo
	capacity         int
	maxCapacity      int
}

func initCache(evictionAlgo evictionAlgo, maxCapacity int) Cache {
	storage := make(map[string]*node)
	return Cache{
		doublyLinkedList: &doublyLinkedList{},
		storage:          storage,
		evictionAlgo:     evictionAlgo,
		capacity:         0,
		maxCapacity:      maxCapacity,
	}
}

func (this *Cache) setEvictionAlgo(e evictionAlgo) {
	this.evictionAlgo = e
}

func (this *Cache) set(key, value string) {
	node_ptr, ok := this.storage[key]
	if ok {
		this.evictionAlgo.set_overwrite(node_ptr, value, this)
		return
	}
	if this.capacity == this.maxCapacity {
		evictedKey := this.evict()
		delete(this.storage, evictedKey)
	}
	node := &node{key: key, value: value}
	this.storage[key] = node
	this.evictionAlgo.set(node, this)
	this.capacity++
}

func (this *Cache) get(key string) string {
	node_ptr, ok := this.storage[key]
	if ok {
		this.evictionAlgo.get(node_ptr, this)
		return (*node_ptr).value
	}
	return ""
}

func (this *Cache) evict() string {
	key := this.evictionAlgo.evict(this)
	this.capacity--
	return key
}

func (this *Cache) print() {
	for k, v := range this.storage {
		fmt.Printf("key :%s value: %s\n", k, (*v).value)
	}
	this.doublyLinkedList.TraverseForward()
}
```

**main.go**

```
package main

import "fmt"

func main() {
	lru := createEvictioAlgo("lru")
	cache := initCache(lru, 3)
	cache.set("a", "1")
	cache.print()

	cache.set("b", "2")
	cache.print()

	cache.set("c", "3")
	cache.print()

	value := cache.get("a")
	fmt.Printf("key: a, value: %s\n", value)
	cache.print()

	cache.set("d", "4")
	cache.print()

	cache.set("e", "5")
	cache.print()
}
```

**输出**

```
Shuffling doubly linked list due to set operation
key :a value: 1
key = a, value = 1, prev = <nil>, next = <nil>Shuffling doubly linked list due to set operation
key :a value: 1
key :b value: 2
key = a, value = 1, prev = <nil>, next = &{b 2 0xc00007e1e0 <nil>}
key = b, value = 2, prev = &{a 1 <nil>0xc00007e210}, next = <nil>Shuffling doubly linked list due to set operation
key :a value: 1
key :b value: 2
key :c value: 3
key = a, value = 1, prev = <nil>, next = &{b 2 0xc00007e1e0 0xc00007e2a0}
key = b, value = 2, prev = &{a 1 <nil>0xc00007e210}, next = &{c 3 0xc00007e210 <nil>}
key = c, value = 3, prev = &{b 2 0xc00007e1e0 0xc00007e2a0}, next = <nil>Shuffling doubly linked list due to get operation
key: a, value: 1
key :a value: 1
key :b value: 2
key :c value: 3
key = b, value = 2, prev = <nil>, next = &{c 3 0xc00007e210 0xc00007e1e0}
key = c, value = 3, prev = &{b 2 <nil>0xc00007e2a0}, next = &{a 1 0xc00007e2a0 <nil>}
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = <nil>Evicting by lru strtegy. Evicted Node Key: %s:  b
Shuffling doubly linked list due to set operation
key :d value: 4
key :c value: 3
key :a value: 1
key = c, value = 3, prev = &{b 2 <nil>0xc00007e2a0}, next = &{a 1 0xc00007e2a0 0xc00007e450}
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = &{d 4 0xc00007e1e0 <nil>}
key = d, value = 4, prev = &{a 1 0xc00007e2a0 0xc00007e450}, next = <nil>Evicting by lru strtegy. Evicted Node Key: %s:  c
Shuffling doubly linked list due to set operation
key :a value: 1
key :d value: 4
key :e value: 5
key = a, value = 1, prev = &{c 3 0xc00007e210 0xc00007e1e0}, next = &{d 4 0xc00007e1e0 0xc00007e570}
key = d, value = 4, prev = &{a 1 0xc00007e2a0 0xc00007e450}, next = &{e 5 0xc00007e450 <nil>}
key = e, value = 5, prev = &{d 4 0xc00007e1e0 0xc00007e570}, next =</nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil></nil> 
```

## **结论**

以上就是在 Golang 中实现 LRU 缓存的所有内容。希望你喜欢这篇文章。请在评论中分享反馈。

+   [lru](https://golangbyexample.com/tag/lru/)*
