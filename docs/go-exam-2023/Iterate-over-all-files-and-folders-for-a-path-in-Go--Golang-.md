<!--yml

分类：未分类

日期：2024-10-13 06:07:16

-->

# 在 Go (Golang) 中遍历路径下的所有文件和文件夹。

> 来源：[https://golangbyexample.com/iterate-over-all-files-and-folders-go/](https://golangbyexample.com/iterate-over-all-files-and-folders-go/)

**‘Walk’** 函数来自 **‘filepath’** 包，可用于递归遍历目录树中的所有文件/文件夹。

```
https://golang.org/pkg/path/filepath/
```

**‘Walk’** 函数将遍历以根路径为根的整个树，包括所有子目录。以下是该函数的签名。

```
type WalkFunc func(path string, info os.FileInfo, err error) error
```

WalkFunc 将被调用，并传入文件/文件夹的 **path** 和 **fileInfo** 或 **error**（如果在遍历该文件/文件夹时发生了错误）。

关于 Walk 函数的一些注意事项

+   所有错误都被过滤。打开/访问文件时可能会出现错误。

+   该函数不遵循符号链接。

+   文件以字典顺序遍历。

让我们看一个例子：

```
package main

import (
    "fmt"
    "log"
    "os"
    "path/filepath"
)

func main() {
    currentDirectory, err := os.Getwd()
    if err != nil {
        log.Fatal(err)
    }
    iterate(currentDirectory)
}

func iterate(path string) {
    filepath.Walk(path, func(path string, info os.FileInfo, err error) error {
        if err != nil {
            log.Fatalf(err.Error())
        }
        fmt.Printf("File Name: %s\n", info.Name())
        return nil
    })
}
```
