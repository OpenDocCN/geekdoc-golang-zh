<!--yml

分类：未分类

日期：2024-10-13 06:10:18

-->

# 在 Go (Golang)中删除/移除一个文件夹

> 来源：[`golangbyexample.com/delete-folder-go/`](https://golangbyexample.com/delete-folder-go/)

**os.Remove()**函数可以用于删除 Golang 中的文件夹。以下是该函数的签名。

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
    err := os.Remove("sample")
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

```go
Deletes sample folder from the current working directory
```

+   [删除](https://golangbyexample.com/tag/remove/)
