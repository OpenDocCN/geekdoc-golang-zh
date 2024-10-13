<!--yml

类别：未分类

日期：2024-10-13 06:13:41

-->

# 在 Go（Golang）中查找并删除数组中的元素

> 来源：[`golangbyexample.com/find-delete-array-golang/`](https://golangbyexample.com/find-delete-array-golang/)

可能有两种情况：

目录

**   修改原数组

+   请勿修改原数组*  *# **修改原数组**

+   继续复制到原数组，跳过需要删除的项。

+   在最后进行切片。

```go
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

**输出：**

```go
[2,3]
```

# **请勿修改原数组**

创建一个新数组并持续插入数据。

```go
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

**输出**

```go
[2,3]
```*
