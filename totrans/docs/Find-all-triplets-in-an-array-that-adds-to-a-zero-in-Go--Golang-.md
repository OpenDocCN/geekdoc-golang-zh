<!--yml
category: 未分类
date: 2024-10-13 06:42:29
-->

# Find all triplets in an array that adds to a zero in Go (Golang)

> 来源：[https://golangbyexample.com/array-triplets-add-zero-golang/](https://golangbyexample.com/array-triplets-add-zero-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Program](#Program "Program")*  *## **Overview**

Let’s say the input is

```
[-3, 0, 3, 4, -1, -6]
```

Then the answer would be

```
[[-3 0 3] [-3 4 -1]]
```

We can use a hash for the solution. It is based upon the idea that

*   If two of the numbers are **x** and **y**

*   Then the other number will be **-(x+y)**

## **Program**

```
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

**Output**

```
[[-3 0 3] [-3 4 -1]]
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*