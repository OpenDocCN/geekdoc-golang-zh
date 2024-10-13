<!--yml

分类：未分类

日期：2024-10-13 06:17:06

-->

# 在Go（Golang）中执行操作系统/系统命令

> 来源：[https://golangbyexample.com/execute-os-system-command-golang/](https://golangbyexample.com/execute-os-system-command-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

**os/exec** 包可用于从Go触发任何操作系统或系统命令。它有两个可以实现相同功能的函数。

+   **命令** – 用于创建cmd对象

+   **输出** – 它运行命令并返回标准输出

# **代码**

```
package main

import (
    "fmt"
    "log"
    "os/exec"
)

func main() {
    out, err := exec.Command("pwd").Output()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(out))
}
```

**输出：**

```
It will output the location of current working directory
```

+   [命令](https://golangbyexample.com/tag/command/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [系统](https://golangbyexample.com/tag/system/)*
