<!--yml

分类：未分类

日期：2024-10-13 06:17:32

-->

# 在 Go (Golang) 中更改文件权限

> 来源：[`golangbyexample.com/change-file-permissions-golang/`](https://golangbyexample.com/change-file-permissions-golang/)

os.Chmod() 函数可用于更改现有文件的权限。下面是该函数的签名

```go
func Chmod(name string, mode FileMode) error
```

**代码**

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    // Create new file
    new, err := os.Create("new.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer new.Close()

    stats, err := os.Stat("new.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Permission File Before: %s\n", stats.Mode())
    err = os.Chmod("new.txt", 0700)
    if err != nil {
        log.Fatal(err)
    }

    stats, err = os.Stat("new.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Permission File After: %s\n", stats.Mode())
}
```

**输出：**

```go
Permission File Before: -rw-r--r--
Permission File After: -rwx------
```

+   [文件](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [权限](https://golangbyexample.com/tag/permissions/)
