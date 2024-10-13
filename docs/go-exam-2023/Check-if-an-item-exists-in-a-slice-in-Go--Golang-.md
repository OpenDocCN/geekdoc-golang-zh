<!--yml

类别：未分类

日期：2024-10-13 06:10:44

-->

# 检查一个项是否存在于 Go (Golang) 的切片中

> 来源：[`golangbyexample.com/item-exists-slice-golang/`](https://golangbyexample.com/item-exists-slice-golang/)

让我们看看一个工作代码

```go
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

**输出：**

```go
Item san found: true
Item can found: false
```
