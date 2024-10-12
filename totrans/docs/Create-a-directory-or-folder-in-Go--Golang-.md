<!--yml
category: 未分类
date: 2024-10-13 06:17:22
-->

# Create a directory or folder in Go (Golang)

> 来源：[https://golangbyexample.com/create-directory-folder-golang/](https://golangbyexample.com/create-directory-folder-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**os.Mkdir()** function can be used to create a directory or folder in go.

Below is the signature of the function.

```
func Mkdir(name string, perm FileMode)
```

It takes in two parameters

*   The first parameter is the named directory. If the named directory is a fully qualified path it will create a directory at that path. If not it will create a directory with respect to current working directory

*   The second parameter specifies the permission bits. The folder or directory is created with this permission bits

# **Code**

```
package main

import (
    "log"
    "os"
)

func main() {
    //Create a folder/directory at a full qualified path
    err := os.Mkdir("/Users/temp", 0755)
    if err != nil {
        log.Fatal(err)
    }

    //Create a folder/directory at a full qualified path
    err = os.Mkdir("temp", 0755)
    if err != nil {
        log.Fatal(err)
    }
}
```

**Output**

```
It will create a directory temp at location /Users location and at the current working directory location
```

*   [create](https://golangbyexample.com/tag/create/)*   [directory](https://golangbyexample.com/tag/directory/)*   [folder](https://golangbyexample.com/tag/folder/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*