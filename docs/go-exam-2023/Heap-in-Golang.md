<!--yml

类别：未分类

日期：2024-10-13 06:05:48

-->

# Golang 中的堆

> 来源：[`golangbyexample.com/heap-in-golang/`](https://golangbyexample.com/heap-in-golang/)

目录

**   介绍

+   MinHeap 的实现

# **介绍**

堆是一种完整的二叉树。完整的二叉树是指除最后一层外，所有层都是满的。堆有两种类型：

+   **MinHeap**：MinHeap 是一种完整的二叉树，其中父节点的值小于或等于其左右子节点的值。

+   **MaxHeap**：MaxHeap 是一种完整的二叉树，其中父节点的值大于或等于其左右子节点的值。

以下是一个 minheap 的表示。注意父节点总是小于或等于子节点

![](img/693dba4ca31661ce2f15e7001b645441.png)

以下是一个 maxheap 的表示。注意父节点总是大于或等于子节点

![](img/32fb0e4c70c6a9b75511ddafc2106833.png)

在这篇文章中，让我们来看一下 Go 中 minheap 的实现。

MaxHeap 的实现可以在链接中找到 – [`golangbyexample.com/maxheap-in-golang/`](https://golangbyexample.com/maxheap-in-golang/)

MinHeap 的完整描述可以在链接中找到 – [`golangbyexample.com/minheap-in-golang/`](https://golangbyexample.com/minheap-in-golang/)

# **MinHeap 的实现**

```go
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

**输出：**

```go
2
3
5
6
7
8
```

+   [heap](https://golangbyexample.com/tag/heap/)*   [heap in go](https://golangbyexample.com/tag/heap-in-go/)*   [maxheap](https://golangbyexample.com/tag/maxheap/)*   [minheap](https://golangbyexample.com/tag/minheap/)*
