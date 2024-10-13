<!--yml

类别：未分类

日期：2024-10-13 06:06:59

-->

# 在 Go (Golang) 中检查文件或目录是否存在

> 来源：[`golangbyexample.com/check-if-file-or-directory-exists-go/`](https://golangbyexample.com/check-if-file-or-directory-exists-go/)

**os.Stat** 和 **os.IsNotExist()** 可用于检查特定文件或目录是否存在。

目录

**   文件存在   

+   文件夹存在*  *# **文件存在   **

```go
package main

import (
    "log"
    "os"
)

func main() {
    fileinfo, err := os.Stat("temp.txt")
    if os.IsNotExist(err) {
        log.Fatal("File does not exist.")
    }
    log.Println(fileinfo)
}
```

# **文件夹存在**

```go
package main

import (
    "log"
    "os"
)

func main() {
    folderInfo, err := os.Stat("temp")
    if os.IsNotExist(err) {
        log.Fatal("Folder does not exist.")
    }
    log.Println(folderInfo)
}
```

+   [目录](https://golangbyexample.com/tag/directory/)*   [golang](https://golangbyexample.com/tag/golang/)*
