<!--yml
category: 未分类
date: 2024-10-13 06:13:28
-->

# Swap two strings in Golang

> 来源：[https://golangbyexample.com/swap-two-strings-in-golang/](https://golangbyexample.com/swap-two-strings-in-golang/)

GO provides a very neat way of swapping of two strings. See below program

```
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

**Output:**

```
Before a:123 b:xyz
After a:xyz b:123
```