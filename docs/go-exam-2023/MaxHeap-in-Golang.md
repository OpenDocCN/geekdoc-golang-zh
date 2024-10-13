<!--yml

分类：未分类

日期：2024-10-13 06:05:42

-->

# Golang 中的最大堆

> 来源：[`golangbyexample.com/maxheap-in-golang/`](https://golangbyexample.com/maxheap-in-golang/)

目录

**   介绍

+   对最大堆的操作

+   实现

# **介绍**

最大堆是一个完整的二叉树，其中父节点的值大于或等于其左子节点和右子节点的值。完整的二叉树是一个除了最后一层外，所有层都是满的二叉树。

我们使用数组来表示最大堆。根元素是 arr[0]。对于索引 i，我们有

+   左子节点 – 2*i + 1

+   右子节点 – 2*i + 2

以下是最大堆的表示

![](img/32fb0e4c70c6a9b75511ddafc2106833.png)

相应的数组将是 [8, 7, 6, 5, 3, 2]

对于索引 0，我们有

+   左子节点 – 2*0 + 1 = 1

+   右子节点 – 2*0 + 2 = 2

因此，arr[0] 即 8 的左子节点是 arr[1] 即 7，右子节点是 arr[2] 即 6

由于每个节点的值大于或等于其子节点的值，因此，根节点的值是最大的值。

# **对最大堆的操作**

+   **插入元素** – 需要 O(log n) 的时间。如果插入的值大于其父节点，则需要向上遍历。这个遍历会一直进行，直到插入的值小于其父节点或者插入的值成为根节点。第二种情况发生在插入的值是最大的。

+   **移除最大元素** – 需要 O(log n) 的时间。它保存根值，然后用数组中的最后一个值替换根值。接着，它对根节点进行最大堆化，这需要 O(log n) 的时间，因为它向下遍历，直到值大于其父节点。

+   **获取最大值** – 需要 O(1) 的时间。返回根值

# **实现**

```go
package main

import "fmt"

type maxheap struct {
    heapArray []int
    size      int
    maxsize   int
}

func newMaxHeap(maxsize int) *maxheap {
    maxheap := &maxheap{
        heapArray: []int{},
        size:      0,
        maxsize:   maxsize,
    }
    return maxheap
}

func (m *maxheap) leaf(index int) bool {
    if index >= (m.size/2) && index <= m.size {
        return true
    }
    return false
}

func (m *maxheap) parent(index int) int {
    return (index - 1) / 2
}

func (m *maxheap) leftchild(index int) int {
    return 2*index + 1
}

func (m *maxheap) rightchild(index int) int {
    return 2*index + 2
}

func (m *maxheap) insert(item int) error {
    if m.size >= m.maxsize {
        return fmt.Errorf("Heap is full")
    }
    m.heapArray = append(m.heapArray, item)
    m.size++
    m.upHeapify(m.size - 1)
    return nil
}

func (m *maxheap) swap(first, second int) {
    temp := m.heapArray[first]
    m.heapArray[first] = m.heapArray[second]
    m.heapArray[second] = temp
}

func (m *maxheap) upHeapify(index int) {
    for m.heapArray[index] > m.heapArray[m.parent(index)] {
        m.swap(index, m.parent(index))
        index = m.parent(index)
    }
}

func (m *maxheap) downHeapify(current int) {
    if m.leaf(current) {
        return
    }
    largest := current
    leftChildIndex := m.leftchild(current)
    rightRightIndex := m.rightchild(current)
    //If current is smallest then return
    if leftChildIndex < m.size && m.heapArray[leftChildIndex] > m.heapArray[largest] {
        largest = leftChildIndex
    }
    if rightRightIndex < m.size && m.heapArray[rightRightIndex] > m.heapArray[largest] {
        largest = rightRightIndex
    }
    if largest != current {
        m.swap(current, largest)
        m.downHeapify(largest)
    }
    return
}

func (m *maxheap) buildMaxHeap() {
    for index := ((m.size / 2) - 1); index >= 0; index-- {
        m.downHeapify(index)
    }
}

func (m *maxheap) remove() int {
    top := m.heapArray[0]
    m.heapArray[0] = m.heapArray[m.size-1]
    m.heapArray = m.heapArray[:(m.size)-1]
    m.size--
    m.downHeapify(0)
    return top
}

func main() {
    inputArray := []int{6, 5, 3, 7, 2, 8}
    maxHeap := newMaxHeap(len(inputArray))
    for i := 0; i < len(inputArray); i++ {
        maxHeap.insert(inputArray[i])
    }
    maxHeap.buildMaxHeap()
    fmt.Println("The Max Heap is ")
    for i := 0; i < len(inputArray); i++ {
        fmt.Println(maxHeap.remove())
    }
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
```

+   [go](https://golangbyexample.com/tag/go/)*   [最大堆](https://golangbyexample.com/tag/maxheap/)*   [Golang 中的最大堆](https://golangbyexample.com/tag/maxheap-in-go/)*
