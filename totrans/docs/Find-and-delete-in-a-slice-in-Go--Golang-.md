<!--yml
category: 未分类
date: 2024-10-13 06:13:37
-->

# Find and delete in a slice in Go (Golang)

> 来源：[https://golangbyexample.com/find-delete-slice-golang/](https://golangbyexample.com/find-delete-slice-golang/)

There can be two cases:

**Modify Original Slice**

*   Keep copying to the original slice skipping the item which needs to be deleted

*   Reslice at the end

```
package main

import "fmt"

func main() {
    s := []int{"a", "b", "c", "a"}
    after := findAndDelete(s, "a")
    fmt.Println(after)
}
func findAndDelete(s []int, item int) []int {
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

**Do not modify the original slice**

*   Create a new slice and keep inserting into it.

```
package main

import "fmt"

func main() {
    before := []int{1, 2, 3, 1}
    after := findAndDelete(before, 1)
    fmt.Println(after)
}

func findAndDelete(s []int, itemToDelete int) []int {
    var new = make([]int, len(s))
    index := 0
    for _, i := range s {
        if i != itemToDelete {
            new = append(new, i)
            index++
        }
    }
    return new[:index]
}
```

**Output**:

```
[2,3]
```