<!--yml
category: 未分类
date: 2024-10-13 06:10:44
-->

# Check if an item exists in a slice in Go (Golang)

> 来源：[https://golangbyexample.com/item-exists-slice-golang/](https://golangbyexample.com/item-exists-slice-golang/)

Let’s see a working code

```
package main

import "fmt"

func main() {
    source := []string{"san", "man", "tan"}
    result := find(source, "san")
    fmt.Printf("Item san found: %t\n", result)
    result = find(source, "can")
    fmt.Printf("Item san found: %t\n", result)
}

func find(source []string, value string) bool {
    for _, item := range source {
        if item == value {
            return true
        }
    }
    return false
}
```

**Output:**

```
Item san found: true
Item can found: false
```