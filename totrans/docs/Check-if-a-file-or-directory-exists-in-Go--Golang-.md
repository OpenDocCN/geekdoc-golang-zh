<!--yml
category: 未分类
date: 2024-10-13 06:06:59
-->

# Check if a file or directory exists in Go (Golang)

> 来源：[https://golangbyexample.com/check-if-file-or-directory-exists-go/](https://golangbyexample.com/check-if-file-or-directory-exists-go/)

**os.Stat** and **os.IsNotExist()** can be used to check whether a particular file or directory exist or not.

Table of Contents

 **   [File Exists   ](#File_Exists "File Exists    ")
*   [Folder Exists](#Folder_Exists "Folder Exists")*  *# **File Exists   **

```
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

# **Folder Exists**

```
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

*   [directory](https://golangbyexample.com/tag/directory/)*   [golang](https://golangbyexample.com/tag/golang/)*