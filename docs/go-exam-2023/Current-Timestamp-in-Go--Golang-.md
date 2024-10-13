<!--yml

分类：未分类

日期：2024-10-13 06:07:51

-->

# Go 语言中的当前时间戳

> 来源：[`golangbyexample.com/current-timestamp-in-golang/`](https://golangbyexample.com/current-timestamp-in-golang/)

目录

**概述**

+   代码*  *# **概述**

在本教程中，我们将看到如何使用 Go 语言中的 time 包获取当前时间戳。当前时间可以用不同的方式表示。

+   **time.Time**对象

```go
t := time.Now() //It will return time.Time object with current timestamp
```

+   **Unix 时间（也称为纪元时间）** – 它是自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的秒数。这个时间也被称为 Unix 纪元。

```go
t := time.Now().Unix() 
//Will return number of seconds passed since Unix epoch
```

+   **Unix 纳秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的纳秒数

```go
t := time.Now().UnixNano() 
//Will return number of nano seconds passed since Unix epoch
```

+   **Unix 毫秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的毫秒数

```go
t:= int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)/ time.Millisecond  
//Number of millisecond elapsed since Unix epoch
```

+   **Unix 微秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的微秒数

```go
t:= int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)/ time.Millisecond  
//Number of millisecond elapsed since Unix epoch
```

# 代码

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Now() //It will return time.Time object with current timestamp
    fmt.Printf("time.Time %s\n", t)

    tUnix := t.Unix()
    fmt.Printf("timeUnix: %d\n", tUnix)

    tUnixNano := t.UnixNano()
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)

    tUnixMilli := int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)
    fmt.Printf("timeUnixMilli: %d\n", tUnixMilli)

    tUnixMicro := int64(time.Nanosecond) * t.UnixNano() / int64(time.Microsecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)
}
```

**输出：**

```go
time.Time 2020-01-24 09:43:42.196901 UTC m=+0.000229700
timeUnix: 1579839222
timeUnixNano: 1579839222196901000
timeUnixMilli: 1579839222196
timeUnixMicro: 1579839222196901
```

+   [纪元](https://golangbyexample.com/tag/epoch/)*   [毫秒](https://golangbyexample.com/tag/miliseconds/)*   [纳秒](https://golangbyexample.com/tag/nanoseconds/)*   [unix](https://golangbyexample.com/tag/unix/)*
