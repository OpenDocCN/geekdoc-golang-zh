<!--yml

类别：未分类

日期：2024-10-13 06:08:09

-->

# Go 中两个时间值之间的时间差

> 来源：[https://golangbyexample.com/time-difference-between-two-time-value-golang/](https://golangbyexample.com/time-difference-between-two-time-value-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码：](#Code "Code:")*  *# **概述**

在 **Go** 中，时间由 time.Time 结构体表示。该结构体有一个 **Sub** 方法，可用于获取两个不同时间值之间的差值。

```go
currentTime := time.Now()
oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
diff := currentTime.Sub(oldTime)
```

**Sub** 函数返回的是类型为 **Duration** 的差值。它以 **int64** 纳秒计数表示。类型 Duration 还定义了多个实用函数，可以用来获取差值。

+   小时

+   分钟

+   秒

+   纳秒

# **代码：**

让我们看一个实际的例子：

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    currentTime := time.Now()
    oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
    diff := currentTime.Sub(oldTime)

    //In hours
    fmt.Printf("Hours: %f\n", diff.Hours())

    //In minutes
    fmt.Printf("Minutes: %f\n", diff.Minutes())

    //In seconds
    fmt.Printf("Seconds: %f\n", diff.Seconds())

    //In nanoseconds
    fmt.Printf("Nanoseconds: %d\n", diff.Nanoseconds())
}
```

**输出：**

```go
Hours 712.985400
Minutes 42779.124024
Seconds 2566747.441457
Nanoseconds 2566747441457000
```*
