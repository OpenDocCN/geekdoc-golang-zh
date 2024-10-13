<!--yml

分类：未分类

日期：2024-10-13 06:07:03

-->

# 检查文件是否是目录在Go (Golang)

> 来源：[https://golangbyexample.com/check-if-file-is-a-directory-go/](https://golangbyexample.com/check-if-file-is-a-directory-go/)

请查看下面的代码以了解一个文件是否是文件或是目录

+   如果temp是一个文件，输出将是 = “temp是一个文件”

+   如果temp是一个目录，输出将是 = “temp是一个目录”

```go
package main

import (
    "fmt"
    "log"
    "os"
)

var (
    fileInfo *os.FileInfo
    err      error
)

func main() {
    info, err := os.Stat("temp")
    if os.IsNotExist(err) {
        log.Fatal("File does not exist.")
    }
    if info.IsDir() {
        fmt.Println("temp is a directory")
    } else {
        fmt.Println("temp is a file")
    }
}
```

+   [目录](https://golangbyexample.com/tag/directory/)
