<!--yml

类别：未分类

日期：2024-10-13 06:17:11

-->

# 在 Go (Golang)中重命名文件或文件夹

> 来源：[`golangbyexample.com/rename-file-folder-golang/`](https://golangbyexample.com/rename-file-folder-golang/)

目录

**   概述

+   代码

    +   重命名文件

    +   重命名文件夹*  *# **概述**

可以使用 os.Rename()函数来重命名文件或文件夹。下面是该函数的签名。

```go
func Rename(old, new string) error
```

**old**和**new**也可以是完整路径。如果**old**和**new**路径不在同一目录中，则**os.Rename()**函数的行为与移动文件或文件夹相同。

# **代码**

## **重命名文件**

下面是重命名文件的代码

```go
package main

import (
    "log"
    "os"
)

func main() {
    //Create a file
    file, err := os.Create("temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    //Change permission so that it can be moved
    err = os.Chmod("temp.txt", 0777)
    if err != nil {
        log.Println(err)
    }

    err = os.Rename("temp.txt", "newTemp.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出**

首先，它将在当前工作目录中创建一个名为 temp.txt 的文件。然后它将其重命名为 newTemp.txt。

## **重命名文件夹**

下面是重命名文件夹的代码

```go
package main

import (
    "log"
    "os"
)

func main() {
    //Create a directory
    err := os.Mkdir("temp", 0755)
    if err != nil {
        log.Fatal(err)
    }
    err = os.Rename("temp", "newTemp")
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

首先，它将在当前工作目录中创建一个名为 temp 的文件夹。然后它将其重命名为 newTemp。

+   [文件](https://golangbyexample.com/tag/file/)*   [文件夹](https://golangbyexample.com/tag/folder/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [重命名](https://golangbyexample.com/tag/rename/)*
