<!--yml
category: 未分类
date: 2024-10-13 06:07:03
-->

# Check if file is a directory or not in Go (Golang)

> 来源：[https://golangbyexample.com/check-if-file-is-a-directory-go/](https://golangbyexample.com/check-if-file-is-a-directory-go/)

See the below code to know if a file is a file or it is a directory

*   If temp is a file output will be = “temp is a file”
*   If temp is a directory output will be = “temp is a directory”

```
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

*   [directory](https://golangbyexample.com/tag/directory/)