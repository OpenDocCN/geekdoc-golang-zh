<!--yml
category: 未分类
date: 2024-10-13 06:05:48
-->

# Heap in Golang

> 来源：[https://golangbyexample.com/heap-in-golang/](https://golangbyexample.com/heap-in-golang/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Implementation of MinHeap](#Implementation_of_MinHeap "Implementation of MinHeap")*  *# **Introduction**

A Heap is a complete binary tree. A complete binary tree is a binary tree in which all levels are full except the last level. Heap is of two types:

*   **MinHeap**: A MinHeap is a complete binary tree in which the value of the parent node is lesser than or equal to the value of its left and right child.

*   **MaxHeap** : A MaxHeap is a complete binary tree in which the value of the parent node is greater than or equal to the value of its left and right child.

Below is a representation of a minheap. Notice that the parent node is always smaller than or equal to the child nodes

![](img/693dba4ca31661ce2f15e7001b645441.png)

Below is a representation of a maxheap.  Notice that the parent node is always greater than or equal to the child nodes

![](img/32fb0e4c70c6a9b75511ddafc2106833.png)

Let’s look at the implementation of a minheap in GO in this post.

MaxHeap implementation you can find at link – [https://golangbyexample.com/maxheap-in-golang/](https://golangbyexample.com/maxheap-in-golang/)

MinHeap full description can be found at link –[https://golangbyexample.com/minheap-in-golang/](https://golangbyexample.com/minheap-in-golang/)

# **Implementation of MinHeap**

```
package main

import "fmt"

type minheap struct {
    heapArray []int
    size      int
    maxsize   int
}

func newMinHeap(maxsize int) *minheap {
    minheap := &minheap{
        heapArray: []int{},
        size:      0,
        maxsize:   maxsize,
    }
    return minheap
}

func (m *minheap) leaf(index int) bool {
    if index >= (m.size/2) && index <= m.size {
        return true
    }
    return false
}

func (m *minheap) parent(index int) int {
    return (index - 1) / 2
}

func (m *minheap) leftchild(index int) int {
    return 2*index + 1
}

func (m *minheap) rightchild(index int) int {
    return 2*index + 2
}

func (m *minheap) insert(item int) error {
    if m.size >= m.maxsize {
        return fmt.Errorf("Heal is ful")
    }
    m.heapArray = append(m.heapArray, item)
    m.size++
    m.upHeapify(m.size - 1)
    return nil
}

func (m *minheap) swap(first, second int) {
    temp := m.heapArray[first]
    m.heapArray[first] = m.heapArray[second]
    m.heapArray[second] = temp
}

func (m *minheap) upHeapify(index int) {
    for m.heapArray[index] < m.heapArray[m.parent(index)] {
        m.swap(index, m.parent(index))
    }
}

func (m *minheap) downHeapify(current int) {
    if m.leaf(current) {
        return
    }
    smallest := current
    leftChildIndex := m.leftchild(current)
    rightRightIndex := m.rightchild(current)
    //If current is smallest then return
    if leftChildIndex < m.size && m.heapArray[leftChildIndex] < m.heapArray[smallest] {
        smallest = leftChildIndex
    }
    if rightRightIndex < m.size && m.heapArray[rightRightIndex] < m.heapArray[smallest] {
        smallest = rightRightIndex
    }
    if smallest != current {
        m.swap(current, smallest)
        m.downHeapify(smallest)
    }
    return
}
func (m *minheap) buildMinHeap() {
    for index := ((m.size / 2) - 1); index >= 0; index-- {
        m.downHeapify(index)
    }
}

func (m *minheap) remove() int {
    top := m.heapArray[0]
    m.heapArray[0] = m.heapArray[m.size-1]
    m.heapArray = m.heapArray[:(m.size)-1]
    m.size--
    m.downHeapify(0)
    return top
}

func main() {
    inputArray := []int{6, 5, 3, 7, 2, 8}
    minHeap := newMinHeap(len(inputArray))
    for i := 0; i < len(inputArray); i++ {
        minHeap.insert(inputArray[i])
    }
    minHeap.buildMinHeap()
    for i := 0; i < len(inputArray); i++ {
        fmt.Println(minHeap.remove())
    }
    fmt.Scanln()
}
```

**Output:**

```
2
3
5
6
7
8
```

*   [heap](https://golangbyexample.com/tag/heap/)*   [heap in go](https://golangbyexample.com/tag/heap-in-go/)*   [maxheap](https://golangbyexample.com/tag/maxheap/)*   [minheap](https://golangbyexample.com/tag/minheap/)*