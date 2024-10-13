<!--yml

category: 未分类

date: 2024-10-13 06:17:57

-->

# 在 Go 中的 cd 命令或更改当前工作目录

> 来源：[`golangbyexample.com/change-current-working-directory-go/`](https://golangbyexample.com/change-current-working-directory-go/)

**概述**

**os.Chdir()** 用于将当前工作目录更改为在 Golang 中指定的目录。它类似于 cd 命令。

以下是该函数的签名

```go
func Chdir(dir string) error
```

**代码**

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    os.Chdir("/Users")
    newDir, err := os.Getwd()
    if err != nil {
    }
    fmt.Printf("Current Working Direcotry: %s\n", newDir)
}
```

**输出：**

```go
Current Working Direcoty: /Users
```

+   [cd 命令](https://golangbyexample.com/tag/cd-command/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)
