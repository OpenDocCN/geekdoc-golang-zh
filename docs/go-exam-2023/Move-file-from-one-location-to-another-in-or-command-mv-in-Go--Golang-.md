<!--yml

分类：未分类

日期：2024-10-13 06:09:38

-->

# 在 Go (Golang) 中将文件从一个位置移动到另一个位置或使用命令 mv

> 来源：[`golangbyexample.com/move-file-from-one-location-to-another-golang/`](https://golangbyexample.com/move-file-from-one-location-to-another-golang/)

os.Rename() 函数可用于在不同位置之间移动文件。它相当于 Linux 的命令 **‘mv’**。以下是该函数的签名

```go
func Rename(oldpath, newpath string) error
```

**代码：**

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

    newLocation := "~/Desktop/temp.txt"
    err = os.Rename("temp.txt", newLocation)
    if err != nil {
        log.Fatal(err)
    }
}
```
