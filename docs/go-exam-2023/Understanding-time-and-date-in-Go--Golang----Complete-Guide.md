<!--yml

分类：未分类

日期：2024-10-13 06:08:40

-->

# 理解 Go（Golang）中的时间和日期 – 完整指南

> 来源：[https://golangbyexample.com/all-about-time-and-date-golang/](https://golangbyexample.com/all-about-time-and-date-golang/)

**注意：** 如果你有兴趣学习 Golang，那么我们有一个全面的 golang 教程系列，欢迎查看 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)。现在让我们看看当前的教程。以下是内容目录。

目录

**   [概述](#Overview "概述")

+   [结构](#Structure "结构")

+   [创建一个新的时间](#Create_a_new_time "创建一个新的时间")

    +   [使用 time.Now()](#Using_timeNow "使用 time.Now()")

    +   [使用 time.Date()](#Using_timeDate "使用 time.Date()")

+   [理解持续时间](#Understanding_Duration "理解持续时间")

+   [加或减时间](#Add_or_Subtract_to_a_time "加或减时间")

    +   [加到时间](#Add_to_time "加到时间 ")

    +   [从时间中减去](#Subtract_to_time "从时间中减去")

+   [时间解析/格式化](#Time_ParsingFormatting "时间解析/格式化")

    +   [时间解析示例](#Time_Parse_Example "时间解析示例")

    +   [时间格式化示例](#Time_Formatting_Example "时间格式化示例")

+   [时间差](#Time_Diff "时间差")

+   [时间转换](#Time_Conversion "时间转换 ")

    +   [在不同的时区之间转换时间](#Convert_time_between_different_timezones "在不同的时区之间转换时间")*  *# **概述**

**时间** 或 **日期** 在 Go 中使用 **time.Time** 结构表示。时间也可以表示为一个

+   **Unix 时间（也称为 Epoch 时间）** – 自 1970 年 1 月 1 日 00:00:00 UTC 起经过的秒数。这段时间也被称为 Unix 纪元。

# **结构**

**time.Time** 对象用于表示特定的时间点。 **time.Time** 结构如下

```go
type Time struct {
    // wall and ext encode the wall time seconds, wall time nanoseconds,
    // and optional monotonic clock reading in nanoseconds.
    wall uint64
    ext  int64
    //Location to represent timeZone
    // The nil location means UTC
    loc *Location
}
```

如你所见，每个 **time.Time** 对象都有一个相关的 **location** 值，用于确定对应于该时间的分钟、小时、月份、日期和年份。

# **创建一个新的时间**

## **使用 time.Now()**

此函数可用于获取当前本地时间戳。函数的签名为

```go
func Now() Time
```

## **使用 time.Date()**

此函数返回对应于给定位置的 **yyyy-mm-dd hh:mm:ss + nsec** 纳秒的时间，并具有适当的时区。函数的签名为：

```go
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

# **理解持续时间**

**持续时间** 是两个时间瞬间之间经过的时间。它表示为 **int64 纳秒** 计数。因此在 Go 中，持续时间实际上是一个表示时间的纳秒数。如果持续时间的值等于 **1000000000**，则表示 **1 秒** 或 **1000 毫秒** 或 **10000000000 纳秒**。

例如，两次时间值之间相差 1 小时的持续时间将等于下面的值，表示 1 小时中的纳秒数。

```go
1 *60*60*1000*1000*1000
```

它在 **time** 包中表示如下。

```go
type Duration int64
```

以下是一些在**time**包中定义的常见持续时间

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

一些定义在**time.Time**对象上的函数返回**持续时间**：

+   **func (t Time) Sub(u Time) Duration** – 它返回持续时间t-u

+   **func Since(t Time) Duration –** 它返回自t以来经过的持续时间

+   **func Until(t Time) Duration** – 它返回直到t的持续时间

# **加或减时间**

现在你已经理解了持续时间，让我们看看如何对时间实例进行加减。

golang中的**time**包定义了两种添加或减去时间的方法。

+   **Add**函数 - 用于将持续时间加到时间t。由于持续时间可以表示为小时、分钟、秒、毫秒、微秒和纳秒，因此Add函数可以用于从时间中加/减小时、分钟、秒、毫秒、微秒和纳秒。其签名为

```go
func (t Time) Add(d Duration) Time
```

+   **AddDate**函数 - 用于给时间t加/减年份、月份和天数。其签名为

```go
func (t Time) AddDate(years int, months int, days int) Time
```

注意：正值用于加时间，负值用于减去。让我们看看加减时间的一个实际例子。

## **加到时间**

以下代码可用于加时间

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Now()

    //Add 1 hours
    newT := t.Add(time.Hour * 1)
    fmt.Printf("Adding 1 hour\n: %s\n", newT)

    //Add 15 min
    newT = t.Add(time.Minute * 15)
    fmt.Printf("Adding 15 minute\n: %s\n", newT)

    //Add 10 sec
    newT = t.Add(time.Second * 10)
    fmt.Printf("Adding 10 sec\n: %s\n", newT)

    //Add 100 millisecond
    newT = t.Add(time.Millisecond * 10)
    fmt.Printf("Adding 100 millisecond\n: %s\n", newT)

    //Add 1000 microsecond
    newT = t.Add(time.Millisecond * 10)
    fmt.Printf("Adding 1000 microsecond\n: %s\n", newT)

    //Add 10000 nanosecond
    newT = t.Add(time.Nanosecond * 10000)
    fmt.Printf("Adding 1000 nanosecond\n: %s\n", newT)

    //Add 1 year 2 month 4 day
    newT = t.AddDate(1, 2, 4)
    fmt.Printf("Adding 1 year 2 month 4 day\n: %s\n", newT)
}
```

**输出：**

```go
Adding 1 hour:
 2020-02-01 02:16:35.893847 +0530 IST m=+3600.000239893

Adding 15 minute:
 2020-02-01 01:31:35.893847 +0530 IST m=+900.000239893

Adding 10 sec:
 2020-02-01 01:16:45.893847 +0530 IST m=+10.000239893

Adding 100 millisecond:
 2020-02-01 01:16:35.903847 +0530 IST m=+0.010239893

Adding 1000 microsecond:
 2020-02-01 01:16:35.903847 +0530 IST m=+0.010239893

Adding 1000 nanosecond:
 2020-02-01 01:16:35.893857 +0530 IST m=+0.000249893

Adding 1 year 2 month 4 day:
 2021-04-05 01:16:35.893847 +0530 IST
```

## **减去时间**

以下代码可用于减去时间

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Now()

    //Add 1 hours
    newT := t.Add(-time.Hour * 1)
    fmt.Printf("Subtracting 1 hour:\n %s\n", newT)

    //Add 15 min
    newT = t.Add(-time.Minute * 15)
    fmt.Printf("Subtracting 15 minute:\n %s\n", newT)

    //Add 10 sec
    newT = t.Add(-time.Second * 10)
    fmt.Printf("Subtracting 10 sec:\n %s\n", newT)

    //Add 100 millisecond
    newT = t.Add(-time.Millisecond * 10)
    fmt.Printf("Subtracting 100 millisecond:\n %s\n", newT)

    //Add 1000 microsecond
    newT = t.Add(-time.Millisecond * 10)
    fmt.Printf("Subtracting 1000 microsecond:\n %s\n", newT)

    //Add 10000 nanosecond
    newT = t.Add(-time.Nanosecond * 10000)
    fmt.Printf("Subtracting 1000 nanosecond:\n %s\n", newT)

    //Add 1 year 2 month 4 day
    newT = t.AddDate(-1, -2, -4)
    fmt.Printf("Subtracting 1 year 2 month 4 day:\n %s\n", newT)
}
```

**输出：**

```go
Subtracting 1 hour:
 2020-02-01 00:18:29.772673 +0530 IST m=-3599.999784391

Subtracting 15 minute:
 2020-02-01 01:03:29.772673 +0530 IST m=-899.999784391

Subtracting 10 sec:
 2020-02-01 01:18:19.772673 +0530 IST m=-9.999784391

Subtracting 100 millisecond:
 2020-02-01 01:18:29.762673 +0530 IST m=-0.009784391

Subtracting 1000 microsecond:
 2020-02-01 01:18:29.762673 +0530 IST m=-0.009784391

Subtracting 1000 nanosecond:
 2020-02-01 01:18:29.772663 +0530 IST m=+0.000205609

Subtracting 1 year 2 month 4 day:
 2018-11-27 01:18:29.772673 +0530 IST
```

# **时间解析/格式化**

如果你曾在其他语言中处理过时间/日期格式化/解析，你可能会注意到其他语言使用特殊的占位符进行时间/日期格式化。例如，ruby语言使用

+   %d表示天

+   %Y表示年份

等等

Golang没有使用上述代码，而是使用看起来像日期和时间的日期和时间格式占位符。Go使用标准时间，即：

```go
Mon Jan 2 15:04:05 MST 2006  (MST is GMT-0700)
or 
01/02 03:04:05PM '06 -0700
```

所以如果你注意到，go使用

+   01表示月份的天数，

+   02表示月份

+   03表示小时，

+   04表示分钟

+   05代表秒

+   等等

以下占位符表描述了确切的映射。Go采用了一种更务实的方法，你不需要像其他语言一样记住或查找传统的格式化代码。

| **类型** | **占位符** |
| --- | --- |
| 天 | **2** 或 **02** 或 **_2** |
| 星期几 | **Monday** 或 **Mon** |
| 月 | **01** 或 **1** 或 **Jan** 或 **January** |
| 年 | **2006** 或 **06** |
| 小时 | **03** 或 **3** 或 **15** |
| 分钟 | **04** 或 **4** |
| 秒 | **05** 或 **5** |
| 毫秒 (ms) | **.000** //尾随零将被包括或 **.999** //尾随零将被省略 |
| 微秒 (μs) | **.000000** //尾随零将被包括或 **.999999** //尾随零将被省略 |
| 纳秒 (ns) | **.000000000** //尾随零将被包括或 **.999999999** //尾随零将被省略 |
| 上午/下午 | **PM** 或 **pm** |
| 时区 | **MST** |
| 时区偏移 | **Z0700** 或 **Z070000** 或 **Z07** 或 **Z07:00** 或 **Z07:00:00** 或 **-0700** 或 **-070000** 或 **-07** 或 **-07:00** 或 **-07:00:00** |

## **时间解析示例**

现在回到**time.Parse**。该函数的签名为

```go
func Parse(layout, value string) (Time, error)
```

**time.Parse**函数接受两个参数。

+   第一个参数是由时间格式占位符组成的布局。

+   第二个参数是表示时间的实际格式化字符串。

你需要确保布局字符串（第一个参数）与要解析的时间的字符串表示（第二个参数）相匹配。

+   对于解析**2020-01-29**，布局字符串应为**06-01-02**或**2006-01-02**，或其他基于上述占位符表正确映射的内容。

+   同样，对于解析**“2020-Jan-29 星期三 12:19:25”**，布局字符串可以是**“2006-Jan-02 星期一 03:04:05”**。

下面是**time.Parse()**的工作代码示例。

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
```

## **时间格式示例**

**time.Format**函数可以用来将时间格式化为字符串表示。该函数的签名是。

```go
func (t Time) Format(layout string)
```

让我们看看一些时间格式代码示例。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    now := time.Now()

    //Format YYYY-MM-DD
    fmt.Printf("YYYY-MM-DD: %s\n", now.Format("2006-01-02"))

    //Format YY-MM-DD
    fmt.Printf("YY-MM-DD: %s\n", now.Format("06-01-02"))

    //Format YYYY-#{MonthName}-DD
    fmt.Printf("YYYY-#{MonthName}-DD: %s\n", now.Format("2006-Jan-02"))

    //Format HH:MM:SS
    fmt.Printf("HH:MM:SS: %s\n", now.Format("03:04:05"))

    //Format HH:MM:SS Millisecond
    fmt.Printf("HH:MM:SS Millisecond: %s\n", now.Format("03:04:05 .999"))

    //Format YYYY-#{MonthName}-DD WeekDay HH:MM:SS
    fmt.Printf("YYYY-#{MonthName}-DD WeekDay HH:MM:SS: %s\n", now.Format("2006-Jan-02 Monday 03:04:05"))

    //Format YYYY-#{MonthName}-DD WeekDay HH:MM:SS PM Timezone TimezoneOffset
    fmt.Printf("YYYY-#{MonthName}-DD WeekDay HH:MM:SS PM Timezone TimezoneOffset: %s\n", now.Format("2006-Jan-02 Monday 03:04:05 PM MST -07:00"))
}
```

**输出：**

```go
YYYY-MM-DD: 2020-01-25
YY-MM-DD: 20-01-25
YYYY-#{MonthName}-DD: 2020-Jan-25
HH:MM:SS: 11:14:16
HH:MM:SS Millisecond: 11:14:16 .213
YYYY-#{MonthName}-DD WeekDay HH:MM:SS: 2020-Jan-25 Saturday 11:14:16
YYYY-#{MonthName}-DD WeekDay HH:MM:SS PM Timezone TimezoneOffset: 2020-Jan-25 Saturday 11:14:16 PM IST +05:30
```

# **时间差**

**time**包有一个方法**Sub**，可以用来获取两个不同时间值之间的差。该函数的签名是。

```go
func (t Time) Sub(u Time) Duration
```

```go
currentTime := time.Now()
oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
diff := currentTime.Sub(oldTime)
```

# **时间转换**

下面的代码展示了转换。

+   time.Time转换为Unix时间戳。

+   Unix时间戳转换为time.Time。

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

## **在不同时间区域之间转换时间**

**In**函数可以用来更改与特定**time.Time**对象相关联的**location**。每当在任何**time.Time**对象（例如t）上调用**In**函数时，

+   创建一个**t**的副本，表示相同的时间瞬间。

+   t的位置被设置为传递给In函数的地点，以便于显示。

+   **t**被返回。

让我们看看下面的工作代码，这可以用来更改与特定时间相关联的位置值。

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

+   [完整指南](https://golangbyexample.com/tag/complete-guide/)*   [格式](https://golangbyexample.com/tag/format/)*   [解析](https://golangbyexample.com/tag/parse/)*   [时区](https://golangbyexample.com/tag/timezone/)*   [使用](https://golangbyexample.com/tag/using/)*
