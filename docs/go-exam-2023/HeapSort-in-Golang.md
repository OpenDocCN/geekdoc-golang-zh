<!--yml

category: 未分类

date: 2024-10-13 06:06:01

-->

# Golang 中的堆排序

> 来源：[`golangbyexample.com/heapsort-in-golang/`](https://golangbyexample.com/heapsort-in-golang/)

目录

**   介绍

+   堆排序步骤：

+   完整工作代码

+   时间复杂度*  *# **介绍**

堆排序是一种基于比较的排序算法，它使用堆数据结构。有关堆的更多信息，请参考此链接 – [`golangbyexample.com/heap-in-golang/`](https://golangbyexample.com/heap-in-golang/)

本文演示了使用最小堆进行堆排序。也可以使用最大堆实现相同的功能。

+   左子节点 – 2*i + 1

+   右子节点 – 2*i + 2

以下是最小堆的表示

![](img/693dba4ca31661ce2f15e7001b645441.png)

# **堆排序步骤：**

+   构建最小堆。在最小堆构建完成后，第一个元素变为最小值

+   将第一个元素移动到数组的最后。再次调用堆化，大小减一。对数组的大小重复此操作

+   最终数组从大到小排序

# **完整工作代码**

```go
package main

import "fmt"

type minheap struct {
    arr []int
}

func newMinHeap(arr []int) *minheap {
    minheap := &minheap{
        arr: arr,
    }
    return minheap
}

func (m *minheap) leftchildIndex(index int) int {
    return 2*index + 1
}

func (m *minheap) rightchildIndex(index int) int {
    return 2*index + 2
}

func (m *minheap) swap(first, second int) {
    temp := m.arr[first]
    m.arr[first] = m.arr[second]
    m.arr[second] = temp
}

func (m *minheap) leaf(index int, size int) bool {
    if index >= (size/2) && index <= size {
        return true
    }
    return false
}

func (m *minheap) downHeapify(current int, size int) {
    if m.leaf(current, size) {
        return
    }
    smallest := current
    leftChildIndex := m.leftchildIndex(current)
    rightRightIndex := m.rightchildIndex(current)
    if leftChildIndex < size && m.arr[leftChildIndex] < m.arr[smallest] {
        smallest = leftChildIndex
    }
    if rightRightIndex < size && m.arr[rightRightIndex] < m.arr[smallest] {
        smallest = rightRightIndex
    }
    if smallest != current {
        m.swap(current, smallest)
        m.downHeapify(smallest, size)
    }
    return
}

func (m *minheap) buildMinHeap(size int) {
    for index := ((size / 2) - 1); index >= 0; index-- {
        m.downHeapify(index, size)
    }
}

func (m *minheap) sort(size int) {
    m.buildMinHeap(size)
    for i := size - 1; i > 0; i-- {
        // Move current root to end
        m.swap(0, i)
        m.downHeapify(0, i)
    }
}

func (m *minheap) print() {
    for _, val := range m.arr {
        fmt.Println(val)
    }
}

func main() {
    inputArray := []int{6, 5, 3, 7, 2, 8, -1}
    minHeap := newMinHeap(inputArray)
    minHeap.sort(len(inputArray))
    minHeap.print()
    fmt.Scanln()
}
```

**输出：**

```go
8
7
6
5
3
2
-1
```

# **时间复杂度**

堆排序的时间复杂度为 O(nLogn)。

+   [go](https://golangbyexample.com/tag/go/)*   [heapsort](https://golangbyexample.com/tag/heapsort/)*   [golang 中的堆排序](https://golangbyexample.com/tag/heapsort-in-go/)*
