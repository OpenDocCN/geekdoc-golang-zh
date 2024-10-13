<!--yml

分类：未分类

日期：2024-10-13 06:10:13

-->

# 在 Go (Golang)中删除文件

> 来源：[`golangbyexample.com/delete-file-go/`](https://golangbyexample.com/delete-file-go/)

**os.Remove()** 函数可用于在 Golang 中删除文件。以下是该函数的签名。

```go
func Remove(name string) error
```

**代码：**

```go
package main

import (
    "log"
    "os"
)

func main() {
    err := os.Remove("sample.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

```go
Deletes sample.txt file from the current working directory
```

+   [删除](https://golangbyexample.com/tag/delete/) *   [golang](https://golangbyexample.com/tag/golang/) *   [移除](https://golangbyexample.com/tag/remove/)
