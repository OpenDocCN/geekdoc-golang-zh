<!--yml

类别：未分类

日期：2024-10-13 06:10:00

-->

# 在 Go (Golang) 中解析布尔值或检查给定字符串是否为布尔值

> 来源：[`golangbyexample.com/check-string-bool-golang/`](https://golangbyexample.com/check-string-bool-golang/)

**strconv.ParseBool()** 函数可以用于解析布尔值的字符串表示。

[`golang.org/pkg/strconv/#ParseBool`](https://golang.org/pkg/strconv/#ParseBool)

下面是该函数的签名

```go
func ParseBool(str string) (bool, error)
```

让我们看看一个工作代码

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    input := "true"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    }

    input = "false"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    }

    input = "garbage"
    if val, err := strconv.ParseBool(input); err == nil {
        fmt.Printf("%T, %v\n", val, val)
    } else {
        fmt.Println("Given input is not a bool")
    }
}
```

**输出：**

```go
bool, true
bool, false
Given input is not a bool
```

+   [布尔值](https://golangbyexample.com/tag/boolean/)*   [Go](https://golangbyexample.com/tag/go/)
