<!--yml

分类：未分类

日期：2024-10-13 06:08:18

-->

# 在 Go 语言中转换不同时区的时间。

> 来源：[`golangbyexample.com/convert-time-timezones-go/`](https://golangbyexample.com/convert-time-timezones-go/)

每个**time.Time**对象都有一个关联的**location**值。当你将任何**time.Time**对象的位置更改为其他位置时，该时间瞬间并不会改变。只有与该时间关联的**location**值会发生变化。与 time.Time 对象关联的**location**仅具有表示或显示逻辑。

**In**函数可用于更改与特定**time.Time**对象关联的**location**。每当在任何**time.Time**对象（例如 t）上调用**In**函数时，

+   创建了一个表示相同时间瞬间的**t**副本。

+   **t**的位置设置为传递给**In**函数的显示位置。

+   **t**被返回。

让我们看看下面的代码，它可以用来更改与特定时间相关的地点值。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    now := time.Now()

    loc, _ := time.LoadLocation("UTC")
    fmt.Printf("UTC Time: %s\n", now.In(loc))

    loc, _ = time.LoadLocation("Europe/Berlin")
    fmt.Printf("Berlin Time: %s\n", now.In(loc))

    loc, _ = time.LoadLocation("America/New_York")
    fmt.Printf("New York Time: %s\n", now.In(loc))

    loc, _ = time.LoadLocation("Asia/Dubai")
    fmt.Printf("Dubai Time: %s\n", now.In(loc))
}
```

**输出：**

```go
UTC Time: 2020-01-31 18:09:41.705858 +0000 UTC
Berlin Time: 2020-01-31 19:09:41.705858 +0100 CET
New York Time: 2020-01-31 13:09:41.705858 -0500 EST
Dubai Time: 2020-01-31 22:09:41.705858 +0400 +04
```

+   [time 包](https://golangbyexample.com/tag/time-package/) * [time.Time](https://golangbyexample.com/tag/time-time/)
