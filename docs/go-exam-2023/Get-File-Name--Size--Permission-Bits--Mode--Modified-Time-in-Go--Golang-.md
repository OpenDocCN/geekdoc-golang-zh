<!--yml

类别：未分类

日期：2024-10-13 06:17:16

-->

# 在 Go（Golang）中获取文件名、大小、权限位、模式、修改时间

> 来源：[`golangbyexample.com/file-info-golang/`](https://golangbyexample.com/file-info-golang/)

目录

+   概述

+   代码

# **概述**

**os.Stat()**函数可用于获取 Go 中文件的信息。该函数返回可用于获取的信息统计数据。

+   文件的名称

+   文件的大小（以字节为单位）

+   文件的修改时间

+   文件的权限位或模式

以下是函数的签名。它接受指定的文件并返回**FileInfo**结构体，该结构体定义了获取上述信息的实用方法。

```go
func Stat(name string) (FileInfo, error)
```

# **代码**

```go
package main

import (
    "fmt"
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

    //Write something to the file
    file.WriteString("some sample text" + "\n")

    //Gets stats of the file
    stats, err := os.Stat("temp.txt")
    if err != nil {
        log.Fatal(err)
    }

    //Prints stats of the file
    fmt.Printf("Permission: %s\n", stats.Mode())
    fmt.Printf("Name: %s\n", stats.Name())
    fmt.Printf("Size: %d\n", stats.Size())
    fmt.Printf("Modification Time: %s\n", stats.ModTime())
}
```

**输出：**

```go
Permission: -rwxrwxrwx
Name: temp.txt
Size: 17
Modification Time: 2020-04-16 22:26:47.080128602 +0530 IST
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [模式](https://golangbyexample.com/tag/mode/)*   [修改时间](https://golangbyexample.com/tag/modified-time/)*   [权限位](https://golangbyexample.com/tag/permission-bits/)*   [大小](https://golangbyexample.com/tag/size/)*
