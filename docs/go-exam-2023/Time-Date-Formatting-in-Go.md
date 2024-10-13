<!--yml

分类：未分类

日期：2024-10-13 06:07:56

-->

# Go语言中的时间/日期格式化

> 来源：[https://golangbyexample.com/time-date-formatting-in-go/](https://golangbyexample.com/time-date-formatting-in-go/)

如果你在其他语言中处理过时间/日期格式化，你可能会注意到其他语言使用特殊占位符进行时间/日期格式化。例如，Ruby语言使用

+   %d 表示日期

+   %Y 表示年份

等等

在Golang中，日期和时间格式占位符看起来像日期和时间。请参考以下占位符表

| **类型** | **占位符** |
| --- | --- |
| 日期 | **2** 或 **02** 或 **_2** |
| 星期几 | **Monday** 或 **Mon** |
| 月 | **01** 或 **1** 或 **Jan** 或 **January** |
| 年 | **2006** 或 **06** |
| 小时 | **03** 或 **3** 或 **15** |
| 分钟 | **04** 或 **4** |
| 秒 | **05** 或 **5** |
| 毫秒 (ms) | **.000** //尾随零将被包含或 **.999**   //尾随零将被省略 |
| 微秒 (μs) | **.000000** //尾随零将被包含或 **.999999**        //尾随零将被省略 |
| 纳秒 (ns) | **.000000000** //尾随零将被包含或 **.999999999** //尾随零将被省略 |
| 上午/下午 | **PM** 或 **pm** |
| 时区 | **MST** |
| 时区偏移 | **Z0700** 或 **Z070000** 或 **Z07** 或 **Z07:00** 或 **Z07:00:00** 或 **-0700** 或 **-070000** 或 **-07** 或 **-07:00** 或 **-07:00:00** |

上述占位符按顺序表示时容易记忆。

**星期-月-日-小时-分钟-秒-年-时区** 将是 **Mon-01-02-03-04-05-06–07**

让我们看看一些时间格式代码示例

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

+   [golang](https://golangbyexample.com/tag/golang/) * [time package](https://golangbyexample.com/tag/time-package/)
