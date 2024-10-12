<!--yml
category: 未分类
date: 2024-10-13 06:06:23
-->

# Bubble Sort in Go (Golang)

> 来源：[https://golangbyexample.com/go-bubble-sort/](https://golangbyexample.com/go-bubble-sort/)

Table of Contents

 **   [Introduction](#Introduction "Introduction")
*   [Time Complexity](#Time_Complexity "Time Complexity")
*   [Space Complexity](#Space_Complexity "Space Complexity")
*   [Implementation:](#Implementation "Implementation:")*  *# **Introduction**

In bubble sort:

*   In each iteration i (starting with 0) , we start from first element and repeatedly swap adjacent elements if in wrong order up till length (**len-i**) where **len** is the length of the array.

*   At the end of the iteration, either the largest or smallest element (depending upon order is ascending or descending) is at position **(len-i)**

# **Time Complexity**

*   O(n*n)

# **Space Complexity**

*   Space Complexity of bubble sort is O(1)

# **Implementation:**

```
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

**Output:**

```
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

*   [algorithms](https://golangbyexample.com/tag/algorithms/)*   [bubble sort in golang](https://golangbyexample.com/tag/bubble-sort-in-golang/)*   [golang](https://golangbyexample.com/tag/golang/)*   [sort](https://golangbyexample.com/tag/sort/)*