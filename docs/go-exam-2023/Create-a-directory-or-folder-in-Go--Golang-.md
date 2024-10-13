<!--yml

分类：未分类

日期：2024-10-13 06:17:22

-->

# 在 Go (Golang) 中创建目录或文件夹

> 来源：[`golangbyexample.com/create-directory-folder-golang/`](https://golangbyexample.com/create-directory-folder-golang/)

目录

+   概述

+   代码

# **概述**

**os.Mkdir()** 函数可以用于在 Go 中创建目录或文件夹。

以下是该函数的签名。

```go
func Mkdir(name string, perm FileMode)
```

它接受两个参数。

+   第一个参数是命名目录。如果命名目录是一个完全合格的路径，它将在该路径下创建一个目录。如果不是，它将相对于当前工作目录创建一个目录。

+   第二个参数指定权限位。文件夹或目录是使用此权限位创建的。

# **代码**

```go
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

**输出**

```go
It will create a directory temp at location /Users location and at the current working directory location
```

+   [创建](https://golangbyexample.com/tag/create/)*   [目录](https://golangbyexample.com/tag/directory/)*   [文件夹](https://golangbyexample.com/tag/folder/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
