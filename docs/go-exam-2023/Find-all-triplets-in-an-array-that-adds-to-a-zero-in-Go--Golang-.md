<!--yml

类别：未分类

日期：2024-10-13 06:42:29

-->

# 在 Go 语言中查找数组中所有和为零的三元组

> 来源：[`golangbyexample.com/array-triplets-add-zero-golang/`](https://golangbyexample.com/array-triplets-add-zero-golang/)

目录

+   概述

+   程序

## **概述**

假设输入为

```go
[-3, 0, 3, 4, -1, -6]
```

那么答案将是

```go
[[-3 0 3] [-3 4 -1]]
```

我们可以使用哈希来解决这个问题。这基于一个想法

+   如果两个数字是 **x** 和 **y**

+   另一个数字将是 **-(x+y)**

## **程序**

```go
package main

import (
    "fmt"
)

func main() {
    output := threeSumZero([]int{-3, 0, 3, 4, -1, -6})
    fmt.Println(output)
}

func threeSumZero(nums []int) [][]int {
    numsLength := len(nums)
    var results [][]int
    for k := 0; k < numsLength-2; k++ {
        numsMap := make(map[int]int)
        for i := k + 1; i < numsLength; i++ {
            if numsMap[0-(nums[i]+nums[k])] > 0 {
                result := []int{nums[k], 0 - (nums[i] + nums[k]), nums[i]}
                results = append(results, result)
            }
            numsMap[nums[i]] = i
        }
    }
    return results
}
```

**输出**

```go
[[-3 0 3] [-3 4 -1]]
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
