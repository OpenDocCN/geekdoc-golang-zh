<!--yml

类别：未分类

日期：2024-10-13 06:08:22

-->

# 理解 Go（Golang）中的时间包中的持续时间

> 来源：[`golangbyexample.com/understanding-duration-go/`](https://golangbyexample.com/understanding-duration-go/)

**duration** 是两个时刻之间经过的时间。它表示为 **int64 纳秒** 计数。因此，持续时间在 Go 中只是一个表示纳秒的数字。如果持续时间值等于 **1000000000**，则表示 **1 秒** 或 **1000 毫秒** 或 **10000000000 纳秒**。由于持续时间是 **int64，** 因此可以表示的最长持续时间为 290 年。因此，持续时间用于捕获两个 **time.Time** 对象之间的纳秒数。例如，两个时间值相隔 1 小时的持续时间将是以下值，相当于 1 小时中的纳秒数。

```go
1 *60*60*1000*1000*1000
```

在 **time** 包中表示如下。

```go
type Duration int64
```

以下是 **time** 包中定义的一些常见持续时间

```go
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```

一些在 **time.Time** 对象上定义的返回 **Duration** 的函数是

+   **func (t Time) Sub(u Time) Duration** – 它返回持续时间 t-u。

+   **func Since(t Time) Duration –** 它返回自 t 以来经过的持续时间。

+   **func Until(t Time) Duration** – 它返回到 t 的持续时间。

类型 **Duration** 还定义了几个实用函数，可用于将持续时间值转换为-

+   小时

+   分钟

+   秒

持续时间的打印值表示为“10h8m0.5s”的形式，其中 **h** 代表 **小时**，**m** 代表 **分钟**，**s** 代表 **秒**。

让我们看看到目前为止我们所学内容的示例代码。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    now := time.Now()
    oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
    diff := now.Sub(oldTime)
    //diff is of type Duration now

    //In hours
    fmt.Printf("Hours: %f\n", diff.Hours())

    //In minutes
    fmt.Printf("Minutes: %f\n", diff.Minutes())

    //In seconds
    fmt.Printf("Seconds: %f\n", diff.Seconds())

    //time Since
    fmt.Println(time.Since(now.Add(-time.Hour * 1)))

    //time until
    fmt.Println(time.Until(now.Add(time.Hour * 1)))
}
```

**输出：**

```go
Hours: 714.565222
Minutes: 42873.913306
Seconds: 2572434.798350
1h0m0.000093629s
59m59.999888401s
```
