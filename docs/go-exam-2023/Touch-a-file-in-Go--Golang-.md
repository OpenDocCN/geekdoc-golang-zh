<!--yml

分类: 未分类

日期: 2024-10-13 06:07:25

-->

# 在 Go（Golang）中触摸文件

> 来源：[`golangbyexample.com/touch-file-golang/`](https://golangbyexample.com/touch-file-golang/)

触摸一个文件意味着

+   如果文件不存在，则创建一个空文件

+   如果文件已经存在，则更新文件的修改时间。

```go
package main

import (
    "fmt"
    "log"
    "os"
    "time"
)

func main() {
    fileName := "temp.txt"
    _, err := os.Stat(fileName)
    if os.IsNotExist(err) {
        file, err := os.Create("temp.txt")
        if err != nil {
            log.Fatal(err)
        }
        defer file.Close()
    } else {
        currentTime := time.Now().Local()
        err = os.Chtimes(fileName, currentTime, currentTime)
        if err != nil {
            fmt.Println(err)
        }
    }
}
```

**输出:**

第一次运行时会创建文件。第一次之后，它会更新文件的修改时间。

+   [空文件](https://golangbyexample.com/tag/empty/) * [文件](https://golangbyexample.com/tag/file/)
