<!--yml

类别：未分类

日期：2024-10-13 06:22:57

-->

# Go（Golang）中的接口零值

> 来源：[`golangbyexample.com/zero-value-of-interface-go/`](https://golangbyexample.com/zero-value-of-interface-go/)

接口的默认值或零值为 nil。下面的程序演示了这一点。

```go
package main

import "fmt"
type animal interface {
    breathe()
    walk()
}

func main() {
    var a animal
    fmt.Println(a)
}
```

**输出**

```go
nil
```


