<!--yml
category: 未分类
date: 2024-10-13 06:07:56
-->

# Time/Date Formatting in Go

> 来源：[https://golangbyexample.com/time-date-formatting-in-go/](https://golangbyexample.com/time-date-formatting-in-go/)

If you have worked with time/date formatting in other languages you might have noticed that the other languages use special placeholders for time/date formatting. For eg ruby language uses

*   %d for day
*   %Y for year

etc

In Golang date and time format placeholders look like date and time only. Refer to below placeholder table

| **Type** | **Placeholder** |
| Day | **2** or **02** or **_2** |
| Day of Week | **Monday** or **Mon** |
| Month | **01** or **1** or **Jan** or **January** |
| Year | **2006** or **06** |
| Hour | **03** or **3** or **15** |
| Minutes | **04** or **4** |
| Seconds | **05** or **5** |
| MilliSeconds  (ms) | **.000** //Trailing zero will be included or **.999**   //Trailing zero will be omitted |
| Micro Seconds (μs) | **.000000** //Trailing zero will be included or **.999999**        //Trailing zero will be omitted |
| Nano Seconds (ns) | **.000000000** //Trailing zero will be included or **.999999999** //Trailing zero will be omitted |
| am/pm | **PM** or **pm** |
| Timezone | **MST** |
| Timezone offset | **Z0700** or **Z070000** or **Z07** or **Z07:00** or **Z07:00:00**  or **-0700** or  **-070000** or **-07** or **-07:00** or **-07:00:00** |

It is easy to remember the above placeholders when represented in sequence.

**Weekday-Month-Day-Hour-Min-Seconds-Year-Timezone** will be **Mon-01-02-03-04-05-06–07**

Let’s see some time format code examples

```
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

**Output:**

```
YYYY-MM-DD: 2020-01-25
YY-MM-DD: 20-01-25
YYYY-#{MonthName}-DD: 2020-Jan-25
HH:MM:SS: 11:14:16
HH:MM:SS Millisecond: 11:14:16 .213
YYYY-#{MonthName}-DD WeekDay HH:MM:SS: 2020-Jan-25 Saturday 11:14:16
YYYY-#{MonthName}-DD WeekDay HH:MM:SS PM Timezone TimezoneOffset: 2020-Jan-25 Saturday 11:14:16 PM IST +05:30
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [time package](https://golangbyexample.com/tag/time-package/)