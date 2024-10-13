<!--yml

类别：未分类

日期：2024-10-13 06:08:31

-->

# 将 Unix 时间戳转换为 Go (Golang) 中的 time.Time

> 来源：[`golangbyexample.com/parse-unix-timestamp-time-go/`](https://golangbyexample.com/parse-unix-timestamp-time-go/)

时间可以在 GO 中以 **time.Time** 和 **Unix 时间戳** 格式表示。Unix 时间戳，也称为纪元时间，是自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的秒数。此时间也称为 Unix 纪元。下面的代码显示了转换过程。

+   time.Time 转 Unix 时间戳

+   Unix 时间戳转 time.Time

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tNow := time.Now()

    //time.Time to Unix Timestamp
    tUnix := tNow.Unix()
    fmt.Printf("timeUnix %d\n", tUnix)

    //Unix Timestamp to time.Time
    timeT := time.Unix(tUnix, 0)
    fmt.Printf("time.Time: %s\n", timeT)
}
```

**输出：**

```go
timeUnix 1257894000
time.Time: 2009-11-10 23:00:00 +0000 UTC
```

+   [time.Time](https://golangbyexample.com/tag/time-time/) * [unix](https://golangbyexample.com/tag/unix/)
