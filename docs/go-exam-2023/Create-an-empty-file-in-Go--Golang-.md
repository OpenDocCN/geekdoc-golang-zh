<!--yml

category: 未分类

date: 2024-10-13 06:09:20

-->

# 在 Go（Golang）中创建一个空文件。

> 来源：[`golangbyexample.com/create-empty-file-go/`](https://golangbyexample.com/create-empty-file-go/)

目录

+   概述

+   代码：

# **概述**

**os.Create()** 可用于在 Go 中创建一个空文件。该函数的签名为

```go
func Create(name string) (*File, error) 
```

基本上这个函数

+   创建一个模式为 0666 的命名文件。

+   如果文件已经存在，它将截断该文件。

+   如果路径有问题，它将返回一个路径错误。

+   它返回一个文件描述符，可用于读取和写入。

# **代码：**

```go
package main

import (
    "log"
    "os"
)

func main() {
    file, err := os.Create("emptyFile.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
}
```

**输出**：

```go
Check the contents of the file. It will be empty
```

+   [空文件](https://golangbyexample.com/tag/empty/)*   [文件](https://golangbyexample.com/tag/file/)*
