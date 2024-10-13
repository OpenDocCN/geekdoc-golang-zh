<!--yml

分类：未分类

日期：2024-10-13 06:04:47

-->

# Go 日志轮换

> 来源：[`golangbyexample.com/go-logger-rotation/`](https://golangbyexample.com/go-logger-rotation/)

本文讨论的是如何在 go 应用程序中轮换日志文件，而无需担心磁盘空间不足的警报。

我们将使用[`github.com/lestrrat/go-file-rotatelogs`](https://github.com/lestrrat/go-file-rotatelogs)进行日志轮换。

**示例：**在下面的示例中，我们正在设置

+   **MaxAge**设置为 10 秒。这表示在日志文件被从文件系统中清除之前的最大年龄。也就是说，文件将在 10 秒后被删除。见

```go
rotatelogs.WithMaxAge(time.Second*10)
```

+   **RotationTime**是 1 秒**。**它设置了文件轮换的时间。所以文件将每秒轮换一次。见

```go
rotatelogs.WithRotationTime(time.Second*1)
```

+   **Location**是/var/log/service/。见

```go
path := "/var/log/service/"
```

+   文件的**Pattern**是“old.UTC..2019-11-30.22:40:10”。见

```go
 fmt.Sprintf("%s.%s", path, "%Y-%m-%d.%H:%M:%S")
```

+   **Link Path**是/var/log/service/current。这个文件将是实际文件的符号链接。见

```go
rotatelogs.WithLinkName("/var/log/service/current")
```

**代码：**

```go
package main

import (
    "fmt"
    "log"
    "time"
    rotatelogs "github.com/lestrrat/go-file-rotatelogs"
    "github.com/sirupsen/logrus"
)

func initiLogger() {
    path := "/var/log/service/old.UTC."
    writer, err := rotatelogs.New(
        fmt.Sprintf("%s.%s", path, "%Y-%m-%d.%H:%M:%S"),
        rotatelogs.WithLinkName("/var/log/service/current"),
        rotatelogs.WithMaxAge(time.Second*10),
        rotatelogs.WithRotationTime(time.Second*1),
    )
    if err != nil {
        log.Fatalf("Failed to Initialize Log File %s", err)
    }
    log.SetOutput(writer)
    return
}

func main() {
    initiLogger()
    for i := 0; i < 100; i++ {
        time.Sleep(time.Second * 1)
        log.Printf("Hello, World!")
    }
    fmt.Scanln()
}
```

前往位置/var/log/service/。你会注意到

+   文件每秒轮换一次。

+   文件的最大年龄是 10 秒。因此，每个文件将在 10 秒后被删除。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [logger](https://golangbyexample.com/tag/logger/)*   [rotate](https://golangbyexample.com/tag/rotate/)
