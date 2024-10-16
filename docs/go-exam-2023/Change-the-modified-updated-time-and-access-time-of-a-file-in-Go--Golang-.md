<!--yml

分类: 未分类

日期: 2024-10-13 06:10:22

-->

# 在 Go（Golang）中更改文件的修改/更新时间和访问时间

> 来源：[`golangbyexample.com/change-updated-time-file-go/`](https://golangbyexample.com/change-updated-time-file-go/)

**os.Chtimes()** 函数可以用来更改 Golang 中一个文件的 mtime（修改时间）或 atime（访问时间）。下面是该函数的签名。

```go
func Chtimes(name string, atime time.Time, mtime time.Time)
```

**代码:**

```go
package main

import (
    "fmt"
    "os"
    "time"
)

func main() {
    fileName := "sample.txt"
    currentTime := time.Now().Local()

    //Set both access time and modified time of the file to the current time
    err := os.Chtimes(fileName, currentTime, currentTime)
    if err != nil {
        fmt.Println(err)
    }
}
```

**输出:**

```go
Changes the atime and mtime of the file.
```

+   [更新](https://golangbyexample.com/tag/updated/)
