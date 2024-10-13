<!--yml

类别：未分类

日期：2024-10-13 06:06:23

-->

# Go 语言中的冒泡排序

> 来源：[https://golangbyexample.com/go-bubble-sort/](https://golangbyexample.com/go-bubble-sort/)

目录

**[介绍](#Introduction "Introduction")**

+   **[时间复杂度](#Time_Complexity "Time Complexity")**

+   **[空间复杂度](#Space_Complexity "Space Complexity")**

+   [实现：](#Implementation "Implementation:")*  *# **介绍**

在冒泡排序中：

+   在每次迭代 i（从 0 开始）中，我们从第一个元素开始，如果相邻元素顺序错误，就不断交换，直到长度为 **(len-i)**，其中 **len** 是数组的长度。

+   在迭代结束时，最大的或最小的元素（取决于顺序是升序还是降序）位于位置 **(len-i)**

# **时间复杂度**

+   O(n*n)

# **空间复杂度**

+   冒泡排序的空间复杂度为 O(1)

# **实现：**

```go
package main

import "fmt"

func main() {
    sample := []int{3, 4, 5, 2, 1}
    bubbleSort(sample)
    sample = []int{3, 4, 5, 2, 1, 7, 8, -1, -3}
    bubbleSort(sample)
}

func bubbleSort(arr []int) {
    len := len(arr)
    for i := 0; i < len-1; i++ {
        for j := 0; j < len-i-1; j++ {
            if arr[j] > arr[j+1] {
                arr[j], arr[j+1] = arr[j+1], arr[j]
            }
        }
    }
    fmt.Println("\nAfter Bubble Sorting")
    for _, val := range arr {
        fmt.Println(val)
    }
}
```

**输出：**

```go
After Bubble Sorting
1
2
3
4
5

After Bubble Sorting
-3
-1
1
2
3
4
5
7
8
```

+   [算法](https://golangbyexample.com/tag/algorithms/)*   [Golang 中的冒泡排序](https://golangbyexample.com/tag/bubble-sort-in-golang/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [排序](https://golangbyexample.com/tag/sort/)*
