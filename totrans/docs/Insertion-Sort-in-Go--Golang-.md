<!--yml
category: 未分类
date: 2024-10-13 06:06:14
-->

# Insertion Sort in Go (Golang)

> 来源：[https://golangbyexample.com/insertion-sort-in-go/](https://golangbyexample.com/insertion-sort-in-go/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Time Complexity](#Time_Complexity "Time Complexity")
*   [Space Complexity](#Space_Complexity "Space Complexity")
*   [Implementation:](#Implementation "Implementation:")*  *# **Introduction**

Insertion Sort is one of the simplest sorting algorithms. In insertion sort, the input array is broken down into two parts

1.  Sorted
2.  Unsorted

*   Initially sorted part only contains the first element of the array.

*   At each step, we pick the next element from the Unsorted part and insert it into the right position in the sorted part.

# **Time Complexity**

*   Worst Case- **O(n*n)**
*   Best Case- **O(n)** – When the array is already sorted

# **Space Complexity**

Space Complexity of insertion sort is **O(1)**

# **Implementation:**

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

**Output:**

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

*   [go](https://golangbyexample.com/tag/go/)*   [insertion](https://golangbyexample.com/tag/insertion/)*   [sorting](https://golangbyexample.com/tag/sorting/)*