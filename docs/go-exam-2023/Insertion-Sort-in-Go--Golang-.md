<!--yml

分类：未分类

日期：2024-10-13 06:06:14

-->

# Go 语言中的插入排序

> 来源：[https://golangbyexample.com/insertion-sort-in-go/](https://golangbyexample.com/insertion-sort-in-go/)

目录

**   [介绍](#Introduction "Introduction")

+   [时间复杂度](#Time_Complexity "Time Complexity")

+   [空间复杂度](#Space_Complexity "Space Complexity")

+   [实现：](#Implementation "Implementation:")*  *# **介绍**

插入排序是最简单的排序算法之一。在插入排序中，输入数组被分为两部分。

1.  已排序

1.  未排序

+   最初的已排序部分仅包含数组的第一个元素。

+   在每一步中，我们从未排序部分选择下一个元素，并将其插入到已排序部分的正确位置。

# **时间复杂度**

+   最坏情况 - **O(n*n)**

+   最好情况 - **O(n)** – 当数组已经排序时

# **空间复杂度**

插入排序的空间复杂度是 **O(1)**

# **实现：**

```
package main

import "fmt"

func main() {
    sample := []int{3, 4, 5, 2, 1}
    insertionSort(sample)

    sample = []int{3, 4, 5, 2, 1, 7, 8, -1, -3}
    insertionSort(sample)
}

func insertionSort(arr []int) {
    len := len(arr)
    for i := 1; i < len; i++ {
        for j := 0; j < i; j++ {
            if arr[j] > arr[i] {
                arr[j], arr[i] = arr[i], arr[j]
            }
        }
    }

    fmt.Println("After Sorting")
    for _, val := range arr {
        fmt.Println(val)
    }
}
```

**输出：**

```
After Sorting
1
2
3
4
5

After Sorting
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

+   [go](https://golangbyexample.com/tag/go/)*   [insertion](https://golangbyexample.com/tag/insertion/)*   [sorting](https://golangbyexample.com/tag/sorting/)*
