<!--yml

分类：未分类

日期：2024-10-13 06:17:01

-->

# 在 Go (Golang) 中返回退出状态码

> 来源：[https://golangbyexample.com/return-exit-status-code-go/](https://golangbyexample.com/return-exit-status-code-go/)

目录

**   [概述](#Overview "概述")**

+   [代码](#Code "代码")*  *# **概述**

golang 的 **‘os’** 包提供了一个 **Exit** 函数，可以用来以状态码退出当前程序。

+   状态码零表示成功

+   非零状态码表示错误。

一旦调用此函数，程序会立即退出。即使是延迟函数也不会被调用。

还需注意状态码应在范围 [0, 125] 内

```
func Exit(code int)
```

让我们来看一个工作代码

# **代码**

```
package main

import (
    "fmt"
    "os"
)

func main() {
    success := true
    if success {
        fmt.Println("Success")
        os.Exit(0)
    } else {
        fmt.Println("Failure")
        os.Exit(1)
    }
}
```

**输出**

尝试将成功设置为 false，以查看不同的输出

```
Success
$ echo $?
0
```

+   [代码](https://golangbyexample.com/tag/code/)*   [退出](https://golangbyexample.com/tag/exit/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [操作系统](https://golangbyexample.com/tag/os/)*   [状态](https://golangbyexample.com/tag/status/)*
