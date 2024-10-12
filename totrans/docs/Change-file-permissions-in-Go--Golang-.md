<!--yml
category: 未分类
date: 2024-10-13 06:17:32
-->

# Change file permissions in Go (Golang)

> 来源：[https://golangbyexample.com/change-file-permissions-golang/](https://golangbyexample.com/change-file-permissions-golang/)

os.Chmod() function can be used to change the permissions of an existing file. Below is the signature of the function

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

**Output:**

```
Permission File Before: -rw-r--r--
Permission File After: -rwx------
```

*   [file](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [permissions](https://golangbyexample.com/tag/permissions/)