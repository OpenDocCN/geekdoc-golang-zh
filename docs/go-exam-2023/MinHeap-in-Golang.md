<!--yml

类别：未分类

日期：2024-10-13 06:05:37

-->

# Golang中的最小堆

> 来源：[https://golangbyexample.com/minheap-in-golang/](https://golangbyexample.com/minheap-in-golang/)

目录

**   [介绍](#Introduction "Introduction")

+   [最小堆操作](#Operations_on_Minheap "Operations on Minheap")

+   [实现](#Implementation "Implementation")*  *# **介绍**

最小堆是一个完全二叉树，其中父节点的值小于或等于其左子节点和右子节点的值。完全二叉树是指除了最后一层外，所有层都是满的二叉树。

我们使用数组来表示最小堆。根元素是arr[0]。对于索引i，我们有

+   左子节点 – 2*i + 1

+   右子节点 – 2*i + 2

下面是一个最小堆的表示

![](img/3a7852c4ca1f9283ebb010132a829a14.png)

相应的数组为[2, 3, 5, 7, 6, 8]

对于索引0，我们有

+   左子节点 – 2*0 + 1 = 1

+   右子节点 – 2*0 + 2 = 2

因此arr[0]即2的左子节点为arr[1]即3，右子节点为arr[2]即5

由于每个节点值小于或等于其子节点的值，因此根节点的值是最小值。

# **最小堆操作**

+   **插入元素**– 需要O(log n)的时间。如果插入的值小于其父节点，则需要向上遍历以修复。这种遍历会持续到插入的值大于其父节点，或者插入的值成为根节点。第二种情况发生在插入的值是最小的。

+   **移除最小元素** – 需要O(log n)的时间。它保存根值，然后用数组中的最后一个值替换它。然后对根进行最小堆化，这需要O(log n)的时间，因为它向下遍历直到小于其父节点。

+   **获取最小值** – 需要O(1)的时间。返回根值

# **实现**

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
        return fmt.Errorf("Heap is full")
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
        index = m.parent(index)
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

```
2
3
5
6
7
8
```

+   [go](https://golangbyexample.com/tag/go/)*   [在go中的最小堆](https://golangbyexample.com/tag/meanheap-in-go/)*
