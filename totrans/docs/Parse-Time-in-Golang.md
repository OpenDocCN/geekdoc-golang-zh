<!--yml
category: 未分类
date: 2024-10-13 06:08:05
-->

# Parse Time in Golang

> 来源：[https://golangbyexample.com/parse-time-in-golang/](https://golangbyexample.com/parse-time-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**Parse** function of **time** package in golang can be used to parse the string representation of time into **time.Time** object

Package link – [https://golang.org/pkg/time/#Parse](https://golang.org/pkg/time/#Parse)

If you have worked with time/date formatting/parsing in other languages you might have noticed that the other languages use special placeholders for time/date formatting. For eg ruby language uses

*   %d for day
*   %Y for year

etc. Golang, instead of using codes such as above, uses date and time format placeholders that look like date and time only. Go uses standard time, which is:

```
Mon Jan 2 15:04:05 MST 2006  (MST is GMT-0700)
or 
01/02 03:04:05PM '06 -0700
```

So if you notice Go uses

*   01 for the day of the month,
*   02 for the month
*   03 for hours,
*   04 for minutes
*   05 for second
*   and so on

Below placeholder table describes the exact mapping. Go takes a more pragmatic approach where you don’t need to remember or lookup for the traditional formatting codes as in other languages

| **Type** | **Placeholder** |
| Day | **2** or **02** or **_2** |
| Day of Week | **Monday** or **Mon** |
| Month | **01** or **1** or **Jan** or **January** |
| Year | **2006** or **06** |
| Hour | **03** or **3** or **15** |
| Minutes | **04** or **4** |
| Seconds | **05** or **5** |
| Milli Seconds  (ms) | **.000** //Trailing zero will be includedor **.999**   //Trailing zero will be omitted |
| Micro Seconds (μs) | **.000000** //Trailing zero will be includedor **.999999**        //Trailing zero will be omitted |
| Nano Seconds (ns) | **.000000000** //Trailing zero will be includedor **.999999999** //Trailing zero will be omitted |
| am/pm | **PM** or **pm** |
| Timezone | **MST** |
| Timezone offset | **Z0700** or **Z070000** or **Z07** or **Z07:00** or **Z07:00:00**  or **-0700** or  **-070000** or **-07** or **-07:00** or **-07:00:00** |

Now coming back to time.Parse

**time.Parse** function takes in two arguments

*   First argument is the layout consisting of time format placeholder
*   Second argument is the actual formatted string representing time.

The way you have to go about this is to make sure that the layout string (first argument ) matches the string representation (second argument) of the time you want to parse into time.Time.

*   For parsing **2020-01-29**, layout string should be **06-01-02** or **2006-01-02** or something which maps correctly based on the above placeholder table.

*   Similarly for parsing **“2020-Jan-29 Wednesday 12:19:25”** the layout string can be **“2006-Jan-02 Monday 03:04:05”**

**time.Parse** will raise an error if it encounters an error while parsing time.

# **Code**

Below are the working Code Examples of **time.Parse().**

```
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

**Output:**

```
2020-01-29 00:00:00 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
2020-01-29 12:19:25 +0000 UTC
2020-01-29 00:19:25 +0530 IST
```*