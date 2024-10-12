<!--yml
category: 未分类
date: 2024-10-13 06:17:37
-->

# Change folder/directory permissions in Go (Golang)

> 来源：[https://golangbyexample.com/change-folder-directory-permissions-go/](https://golangbyexample.com/change-folder-directory-permissions-go/)

**os.Chmod()** function can be used to change the permissions of an existing folder or directory. Below is the signature of the function

```
func Chmod(name string, mode FileMode) error
```

**Code**

```
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

**Output:**

```
Permission Folder Before: drwxr-xr-x
Permission Folder After:  drwx------
```

*   [directory](https://golangbyexample.com/tag/directory/)*   [folder](https://golangbyexample.com/tag/folder/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [permissions](https://golangbyexample.com/tag/permissions/)