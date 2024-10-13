<!--yml

类别：未分类

日期：2024-10-13 06:06:18

-->

# Go（Golang）中的选择排序

> 来源：[`golangbyexample.com/go-selection-sort/`](https://golangbyexample.com/go-selection-sort/)

目录

**   介绍

+   时间复杂度

+   空间复杂度

+   实现：*  *# **介绍**

在选择排序中，我们维护两个部分

1.  已排序部分

1.  未排序部分

+   在每次迭代中，从未排序部分中选取最大或最小元素（取决于顺序是升序还是降序），然后将其放到已排序部分的末尾。

+   因此，在每次迭代中，已排序部分的长度增加 1。最终，整个数组都被排序。

# **时间复杂度**

+   O(n*n)

# **空间复杂度**

+   选择排序的空间复杂度是 O(1)

# **实现：**

```go
package main

import "fmt"

func main() {
    sample := []int{3, 4, 5, 2, 1}
    selectionSort(sample)
    sample = []int{3, 4, 5, 2, 1, 7, 8, -1, -3}
    selectionSort(sample)
}

func selectionSort(arr []int) {
    len := len(arr)
    for i := 0; i < len-1; i++ {
        minIndex := i
        for j := i + 1; j < len; j++ {
            if arr[j] < arr[minIndex] {
                arr[j], arr[minIndex] = arr[minIndex], arr[j]
            }
        }
    }
    fmt.Println("\nAfter SelectionSort")
    for _, val := range arr {
        fmt.Println(val)
    }
}
```

**输出：**

```go
After SelectionSort
1
2
3
4
5

After SelectionSort
-3
-1
1
2
3
4
5
7
8
```*
