<!--yml

类别：未分类

日期：2024-10-13 06:09:51

-->

# 在 Go (Golang) 中检查字符串是否为数字

> 来源：[`golangbyexample.com/check-if-string-is-number-golang/`](https://golangbyexample.com/check-if-string-is-number-golang/)

**strconv.Atoi()** 函数可用于检查字符串是否为数字。如果传入的字符串不是数字，此函数将返回错误。该函数将数字解析为基数 10。

[`golang.org/pkg/strconv/#Atoi`](https://golang.org/pkg/strconv/#Atoi)

函数的签名是

```go
func Atoi(s string) (int, error)
```

**有效代码：**

```go
package main

import (
    "fmt"
    "strconv"
)
func main() {
    x := "1234"
    val, err := strconv.Atoi(x)
    if err != nil {
        fmt.Printf("Supplied value %s is not a number\n", x)
    } else {
        fmt.Printf("Supplied value %s is a number with value %d\n", x, val)
    }

    y := "123b"
    val, err = strconv.Atoi(y)
    if err != nil {
        fmt.Printf("Supplied value %s is not a number\n", y)
    } else {
        fmt.Printf("Supplied value %s is a number with value %d\n", y, val)
    }
}
```

**输出：**

```go
Supplied value 1234 is a number with value 1234
Supplied value 123b is not a number
```


