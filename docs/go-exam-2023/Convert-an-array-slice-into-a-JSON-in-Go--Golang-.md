<!--yml

分类：未分类

日期：2024-10-13 06:33:26

-->

# 在 Go (Golang) 中将数组/切片转换为 JSON

> 来源：[https://golangbyexample.com/array-slice-json-golang/](https://golangbyexample.com/array-slice-json-golang/)

目录

**   [概述](#Overview "Overview")

+   [示例](#Example "Example")*  *# **概述**

**encoding/json** 包提供了 **Marshal** 函数，可用于将 golang 数组或切片转换为 JSON 字符串，反之亦然。

让我们看看一个例子

# **示例**

```
package main
import (
    "encoding/json"
    "fmt"
)
func main() {
    a := make([]string, 2)
    a[0] = "John"
    a[1] = "Sam"
    j, err := json.Marshal(a)
    if err != nil {
        fmt.Printf("Error: %s", err.Error())
    } else {
        fmt.Println(string(j))
    }
}
```

**输出**

```
["John","Sam"]
```*
