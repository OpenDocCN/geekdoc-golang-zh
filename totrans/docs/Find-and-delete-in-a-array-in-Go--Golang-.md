<!--yml
category: 未分类
date: 2024-10-13 06:13:41
-->

# Find and delete in a array in Go (Golang)

> 来源：[https://golangbyexample.com/find-delete-array-golang/](https://golangbyexample.com/find-delete-array-golang/)

There can be two cases:

Table of Contents

 **   [Modify Original Array](#Modify_Original_Array "Modify Original Array")
*   [Do not modify the original array](#Do_not_modify_the_original_array "Do not modify the original array")*  *# **Modify Original Array**

*   Keep copying to the original array skipping the item which needs to be deleted

*   Reslice at the end

```
package main

import "fmt"

func main() {
    s := [4]int{1, 2, 3, 1}
    after := findAndDelete(s, 1)
    fmt.Println(after)
}

func findAndDelete(s [4]int, item int) []int {
    index := 0
    for _, i := range s {
        if i != item {
            s[index] = i
            index++
        }
    }
    return s[:index]
}
```

**Output:**

```
[2,3]
```

# **Do not modify the original array**

Create a new array and keep inserting into it.

```
package main

import "fmt"

func main() {
    before := [4]int{1, 2, 3, 1}
    after := findAndDelete(before, 1)
    fmt.Println(after)
}

func findAndDelete(s [4]int, itemToDelete int) []int {
    var new [4]int
    index := 0
    for _, i := range s {
        if i != itemToDelete {
            new[index] = i
            index++
        }
    }
    return new[:index]
}
```

**Output**

```
[2,3]
```*