<!--yml 

分类：未分类 

日期：2024-10-13 06:17:37 

--> 

# 在 Go (Golang) 中更改文件夹/目录权限 

> 来源：[https://golangbyexample.com/change-folder-directory-permissions-go/](https://golangbyexample.com/change-folder-directory-permissions-go/) 

**os.Chmod()** 函数可用于更改现有文件夹或目录的权限。以下是该函数的签名。

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
    err := os.Mkdir("new", 0755)
    if err != nil {
        log.Fatal(err)
    }
    stats, err := os.Stat("new")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Permission Folder Before: %s\n", stats.Mode())
    err = os.Chmod("new", 0700)
    if err != nil {
        log.Fatal(err)
    }
    stats, err = os.Stat("new")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Permission Folder After:  %s\n", stats.Mode())
}
```

**输出：** 

```go
Permission Folder Before: drwxr-xr-x
Permission Folder After:  drwx------
```

+   [目录](https://golangbyexample.com/tag/directory/) *   [文件夹](https://golangbyexample.com/tag/folder/) *   [Go](https://golangbyexample.com/tag/go/) *   [Golang](https://golangbyexample.com/tag/golang/) *   [权限](https://golangbyexample.com/tag/permissions/) 
