<!--yml

类别：未分类

日期：2024-10-13 06:09:55

-->

# 在 Go (Golang)中将字符串解析为浮点数

> 来源：[`golangbyexample.com/parse-string-representation-float-go/`](https://golangbyexample.com/parse-string-representation-float-go/)

**strconv.ParseFloat()**函数可以用于解析浮点数的字符串表示。

[`golang.org/pkg/strconv/#ParseFloat`](https://golang.org/pkg/strconv/#ParseFloat)

以下是函数的签名

```go
func ParseFloat(s string, bitSize int) (float64, error) 
```

一些值得注意的要点

+   第一个参数是浮点数的字符串表示。

+   第二个参数是 bitSize，指定精度。float32 为 32，float64 为 64。

+   返回值始终为 float64，但可以无损地转换为 float32。

让我们看看一个工作代码

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    e1 := "1.3434"
    if s, err := strconv.ParseFloat(e1, 32); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
    if s, err := strconv.ParseFloat(e1, 64); err == nil {
        fmt.Printf("%T, %v\n", s, s)
    }
}
```

**输出**

```go
float64, 1.343400001525879
float64, 1.3434
```
