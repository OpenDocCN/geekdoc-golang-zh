<!--yml
category: 未分类
date: 2024-10-13 06:06:18
-->

# Selection Sort in Go(Golang)

> 来源：[https://golangbyexample.com/go-selection-sort/](https://golangbyexample.com/go-selection-sort/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Time Complexity](#Time_Complexity "Time Complexity")
*   [Space Complexity](#Space_Complexity "Space Complexity")
*   [Implementation:](#Implementation "Implementation:")*  *# **Introduction**

In selection sort, we maintain two parts

1.  Sorted Part
2.  Unsorted Part

*   In each iteration, the max or min element (depending upon order is asc or desc) is picked from the unsorted part and then is put at the end of the sorted part.

*   Hence the length of the sorted part in each iteration increases by 1\. In the end, the entire array is sorted.

# **Time Complexity**

*   O(n*n)

# **Space Complexity**

*   Space Complexity of selection sort is O(1)

# **Implementation:**

```
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

**Output:**

```
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