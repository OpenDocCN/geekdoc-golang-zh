<!--yml

category: 未分类

date: 2024-10-13 06:17:52

-->

# 获取当前用户的主目录（Go/Golang）

> 来源：[`golangbyexample.com/get-current-user-home-directory-go/`](https://golangbyexample.com/get-current-user-home-directory-go/)

**概述**

**‘os/user’** 包可用于获取当前用户的主目录

让我们看看一个工作示例

**代码**

```go
package main

import (
    "fmt"
    "log"
    "os/user"
)

func main() {
    user, err := user.Current()
    if err != nil {
        log.Fatalf(err.Error())
    }
    homeDirectory := user.HomeDir
    fmt.Printf("Home Directory: %s\n", homeDirectory)
}
```

**输出**

```go
Home Directory: "some_home_directory"
```

+   [目录](https://golangbyexample.com/tag/directory/)*   [去](https://golangbyexample.com/tag/go/)*   [Golang](https://golangbyexample.com/tag/golang/)*   [主页](https://golangbyexample.com/tag/home/)
