<!--yml

分类：未分类

日期：2024-10-13 06:13:37

-->

# 在 Go (Golang) 中查找并删除切片

> 来源：[https://golangbyexample.com/find-delete-slice-golang/](https://golangbyexample.com/find-delete-slice-golang/)

可能有两种情况：

**修改原始切片**

+   保持复制到原始切片，跳过需要删除的项

+   在末尾重新切片

```go
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

**输出：**

```go
[2,3]
```

**不要修改原始切片**

+   创建一个新切片并持续插入内容。

```go
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

**输出**：

```go
[2,3]
```
