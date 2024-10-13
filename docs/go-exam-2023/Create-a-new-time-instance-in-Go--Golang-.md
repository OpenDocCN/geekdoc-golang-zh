<!--yml

分类：未分类

日期：2024-10-13 06:08:49

-->

# 在 Go (Golang) 中创建一个新时间实例

> 来源：[https://golangbyexample.com/create-new-time-instance-go/](https://golangbyexample.com/create-new-time-instance-go/)

在 Go 中，**time.Time** 结构用于表示时间或日期的实例。下面是创建新时间实例的三种方法

目录

**   [使用 time.Now()](#Using_timeNow "使用 time.Now()")

+   [使用 time.Date()](#Using_timeDate "使用 time.Date()")

+   [使用 time.Parse()](#Using_timeParse "使用 time.Parse()")*  *# **使用 time.Now()**

**time.Now()** 函数可用于获取当前本地时间戳。该函数的签名是

```go
func Now() Time
```

# **使用 time.Date()**

time.Date() 函数接受年份、月份、日期、小时、分钟、秒、纳秒和位置，并返回一个时间格式为 **yyyy-mm-dd hh:mm:ss + nsec**，具有与给定位置相对应的适当时区。该函数的签名是

```go
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

# **使用 time.Parse()**

time.Parse() 可用于将时间的字符串表示转换为 **time.Time** 实例。该函数的签名是

```go
func Parse(layout, value string) (Time, error)
```

**time.Parse** 函数接受两个参数 –

+   第一个参数是包含时间格式占位符的布局

+   第二个参数是表示时间的实际格式化字符串。

下面是一个演示上述三种方法的工作代码示例

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    //time.Now() example
    t := time.Now()
    fmt.Println(t)

    //time.Date() Example
    t = time.Date(2021, time.Month(2), 21, 1, 10, 30, 0, time.UTC)
    fmt.Println(t)

    //time.Parse() Example
    //Parse YYYY-MM-DD
    t, _ = time.Parse("2006-01-02", "2020-01-29")
    fmt.Println(t)
}
```

**输出：**

```go
2020-02-03 11:34:10.85639 +0530 IST m=+0.000297951
2021-02-21 01:10:30 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
```

+   [golang](https://golangbyexample.com/tag/golang/)*
