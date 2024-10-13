<!--yml

分类：未分类

日期：2024-10-13 06:07:20

-->

# 在 Go (Golang) 中获取当前工作目录

> 来源：[`golangbyexample.com/current-working-directory-golang/`](https://golangbyexample.com/current-working-directory-golang/)

**os.Getwd()** 用于获取当前工作目录。输出将类似于 linux 上的 **pwd** 命令

```go
package main
import (
    "fmt"
    "log"
    "os"
)

func main() {
    currentWorkingDirectory, err := os.Getwd()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Current Wroking Direcoty: %s", currentWorkingDirectory)
}
```

**输出：**

当前工作目录：<将是机器上的当前工作目录>

+   [目录](https://golangbyexample.com/tag/directory/)*   [Go](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [pwd](https://golangbyexample.com/tag/pwd/)
