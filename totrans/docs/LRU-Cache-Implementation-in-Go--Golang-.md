<!--yml
category: 未分类
date: 2024-10-13 06:44:58
-->

# LRU Cache Implementation in Go (Golang)

> 来源：[https://golangbyexample.com/lru-cache-implementation-golang/](https://golangbyexample.com/lru-cache-implementation-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
    *   [Implementation Details](#Implementation_Details "Implementation Details")
    *   [Set(key int, value int)](#Setkey_int_value_int "Set(key int, value int)")
    *   [Get(key int)](#Getkey_int "Get(key int) ")
*   [Program](#Program "Program")
*   [Conclusion](#Conclusion "Conclusion")*  *## **Overview**

The objective is to implement a cache that will

*   It should support **Set** and **Get** Operation

*   O(1) Time Complexity for both **Set** and **Get**

*   Assume the maximum capacity of the cache is 3\. Once the cache is full and there is one more key to be inserted then one of the existing entries needs to be deleted from the cache

*   Deletion should be based on eviction algorithm – **LRU**

### **Implementation Details**

*   We will use a **Map** and a **Doubly-linked List** for storing everything. Both map and doubly-linked list are used so that **get** and **set** operations are of  O(1) even with evictions

*   The **Map** will have the key of type string and the value of the type pointer to a node in the **Doubly-linked List**

*   Each node of the **Doubly Linked List** will contain the key as well as value. Each node will also have a pointer to the previous node in the double linked list and a pointer to the next node in the **doubly linked list**

Let’s see how Get and Set are going to work in O(1) time

### **Set(key int, value int)**

For any set operation, it will first create a doubly-linked list node with key and value supplied. Then an entry will be made into the map with key as the input key and value as the address of the node. Once the node is created, then there are two cases

*   The cache is not full –  In this case, it will pass the control to the current Eviction Algorithm which is LRU. The LRU algorithm is going to do insert that node in a double-linked list at the end. The idea is that the front node in the doubly linked list is the least recently used node. Every operation is O(1) here

*   The cache is full – In this case, it will pass the control to the current eviction Algorithm which is LRU. It will evict the least recently used node which is the front node. Once that node is evicted it will insert the new node at the end. Every operation is O(1) here

### **Get(key int)**

For any Get operation, it will first check the map if the given key exists. If it exists then it will fetch the address of the node pointed to by key in the map. It will then fetch the value from the node. Then it will pass the control to the current Eviction Algorithm which is LRU. The LRU algorithm is going to move the current node in the doubly-linked list at the end. Again the idea is that the front node in the doubly linked list is the least recently used node and to maintain that current node is moved to the end. Every operation is O(1) here.

## **Program**

Here is the full working code in go programming language if anyone is interested.

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

**Output**

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

## **Conclusion**

This is all about LRU cache implementation in Golang. Hope you have liked this article. Please share feedback in the comments

*   [lru](https://golangbyexample.com/tag/lru/)*