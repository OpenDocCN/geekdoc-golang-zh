<!--yml

分类：未分类

日期：2024-10-13 06:13:28

-->

# 在 Golang 中交换两个字符串

> 来源：[`golangbyexample.com/swap-two-strings-in-golang/`](https://golangbyexample.com/swap-two-strings-in-golang/)

GO 提供了一种非常简洁的方式来交换两个字符串。请看下面的程序

```go
package main

import "fmt"

func main() {
    a := "123"
    b := "xyz"
    fmt.Printf("Before a:%s b:%s\n", a, b)
    a, b = b, a
    fmt.Printf("After a:%s b:%s\n", a, b)
}
```

**输出：**

```go
Before a:123 b:xyz
After a:xyz b:123
```
