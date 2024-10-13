<!--yml

分类：未分类

日期：2024-10-13 06:10:18

-->

# 在Go (Golang)中删除/移除一个文件夹

> 来源：[https://golangbyexample.com/delete-folder-go/](https://golangbyexample.com/delete-folder-go/)

**os.Remove()**函数可以用于删除Golang中的文件夹。以下是该函数的签名。

```
func Remove(name string) error
```

**代码：**

```
package main

import (
    "log"
    "os"
)

func main() {
    err := os.Remove("sample")
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

```
Deletes sample folder from the current working directory
```

+   [删除](https://golangbyexample.com/tag/remove/)
