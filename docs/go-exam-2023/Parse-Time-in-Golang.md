<!--yml

类别：未分类

日期：2024-10-13 06:08:05

-->

# 在 Golang 中解析时间

> 来源：[https://golangbyexample.com/parse-time-in-golang/](https://golangbyexample.com/parse-time-in-golang/)

目录

**[概述](#Overview "Overview")**

+   [代码](#Code "Code")*  *# **概述**

**time** 包中的 **Parse** 函数可以用来解析时间的字符串表示为 **time.Time** 对象

包链接 – [https://golang.org/pkg/time/#Parse](https://golang.org/pkg/time/#Parse)

如果你曾在其他语言中处理时间/日期格式化/解析，你可能会注意到其他语言使用特殊占位符来格式化时间/日期。例如，Ruby 语言使用

+   %d 表示天

+   %Y 表示年份

等等。Golang 不使用上述代码，而是使用看起来像日期和时间的日期和时间格式占位符。Go 使用标准时间，标准时间是：

```go
Mon Jan 2 15:04:05 MST 2006  (MST is GMT-0700)
or 
01/02 03:04:05PM '06 -0700
```

所以如果你注意到 Go 使用

+   01表示月份中的天数，

+   02为月份

+   03表示小时，

+   04表示分钟

+   05表示秒

+   依此类推

下面的占位符表描述了确切的映射。Go 采用了更务实的方法，你无需记住或查找其他语言中的传统格式代码

| **类型** | **占位符** |
| --- | --- |
| 天 | **2** 或 **02** 或 **_2** |
| 星期几 | **星期一** 或 **Mon** |
| 月 | **01** 或 **1** 或 **Jan** 或 **January** |
| 年 | **2006** 或 **06** |
| 小时 | **03** 或 **3** 或 **15** |
| 分钟 | **04** 或 **4** |
| 秒 | **05** 或 **5** |
| 毫秒 (ms) | **.000** //尾随零将包含或 **.999** //尾随零将省略 |
| 微秒 (μs) | **.000000** //尾随零将包含或 **.999999** //尾随零将省略 |
| 纳秒 (ns) | **.000000000** //尾随零将包含或 **.999999999** //尾随零将省略 |
| 上午/下午 | **PM** 或 **pm** |
| 时区 | **MST** |
| 时区偏移 | **Z0700** 或 **Z070000** 或 **Z07** 或 **Z07:00** 或 **Z07:00:00** 或 **-0700** 或 **-070000** 或 **-07** 或 **-07:00** 或 **-07:00:00** |

现在回到 time.Parse

**time.Parse** 函数接受两个参数

+   第一个参数是包含时间格式占位符的布局

+   第二个参数是表示时间的实际格式化字符串。

你需要确保布局字符串（第一个参数）与要解析为 time.Time 的时间字符串表示（第二个参数）相匹配。

+   对于解析 **2020-01-29**，布局字符串应为 **06-01-02** 或 **2006-01-02**，或基于上述占位符表正确映射的其他格式。

+   类似地，对于解析 **“2020-Jan-29 星期三 12:19:25”**，布局字符串可以是 **“2006-Jan-02 星期一 03:04:05”**

如果 **time.Parse** 在解析时间时遇到错误，将会抛出错误。

# **代码**

下面是 **time.Parse()** 的工作代码示例。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    //Parse YYYY-MM-DD
    timeT, _ := time.Parse("2006-01-02", "2020-01-29")
    fmt.Println(timeT)

    //Parse YY-MM-DD
    timeT, _ = time.Parse("06-01-02", "20-01-29")
    fmt.Println(timeT)

    //Parse YYYY-#{MonthName}-DD
    timeT, _ = time.Parse("2006-Jan-02", "2020-Jan-29")
    fmt.Println(timeT)

    //Parse YYYY-#{MonthName}-DD WeekDay HH:MM:SS
    timeT, _ = time.Parse("2006-Jan-02 Monday 03:04:05", "2020-Jan-29 Wednesday 12:19:25")
    fmt.Println(timeT)

    //Parse YYYY-#{MonthName}-DD WeekDay HH:MM:SS PM Timezone TimezoneOffset
    timeT, _ = time.Parse("2006-Jan-02 Monday 03:04:05 PM MST -07:00", "2020-Jan-29 Wednesday 12:19:25 AM IST +05:30")
    fmt.Println(timeT)
}
```

**输出：**

```go
2020-01-29 00:00:00 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
2020-01-29 12:19:25 +0000 UTC
2020-01-29 00:19:25 +0530 IST
```*
