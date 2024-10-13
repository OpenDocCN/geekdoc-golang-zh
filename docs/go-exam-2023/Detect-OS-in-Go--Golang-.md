<!--yml

类别：未分类

日期：2024-10-13 06:10:27

-->

# 在 Go (Golang) 中检测操作系统

> 来源：[https://golangbyexample.com/detect-os-golang/](https://golangbyexample.com/detect-os-golang/)

**runtime.GOOS** 常量可以用于在运行时检测操作系统，因为这个常量仅在运行时设置。

**runtime.GOARCH** 可以用来知道正在运行的程序的架构目标。

要查看所有可能的 GOOS 和 GOARCH 的组合，请运行以下命令

```go
go tool dist list
```

以下是获取当前运行操作系统的代码：

```go
package main

import (
    "fmt"
    "runtime"
)

func main() {
    os := runtime.GOOS
    switch os {
    case "windows":
        fmt.Println("Windows")
    case "darwin":
        fmt.Println("MAC operating system")
    case "linux":
        fmt.Println("Linux")
    default:
        fmt.Printf("%s.\n", os)
    }
}
```

**输出：**

```go
Your current operating system
```
