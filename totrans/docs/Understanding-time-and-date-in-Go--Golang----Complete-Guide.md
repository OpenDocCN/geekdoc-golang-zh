<!--yml
category: 未分类
date: 2024-10-13 06:08:40
-->

# Understanding time and date in Go (Golang) – Complete Guide

> 来源：[https://golangbyexample.com/all-about-time-and-date-golang/](https://golangbyexample.com/all-about-time-and-date-golang/)

**Note:** If you are interested in learning Golang, then for that we have a golang comprehensive tutorial series. Do check it out – [Golang Comprehensive Tutorial Series](https://golangbyexample.com/golang-comprehensive-tutorial/). Now let’s see the current tutorial. Below is the table of contents.

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Structure](#Structure "Structure")
*   [Create a new time](#Create_a_new_time "Create a new time")
    *   [Using time.Now()](#Using_timeNow "Using time.Now()")
    *   [Using time.Date()](#Using_timeDate "Using time.Date()")
*   [Understanding Duration](#Understanding_Duration "Understanding Duration")
*   [Add or Subtract to a time](#Add_or_Subtract_to_a_time "Add or Subtract to a time")
    *   [Add to time](#Add_to_time "Add to time ")
    *   [Subtract to time](#Subtract_to_time "Subtract to time ")
*   [Time Parsing/Formatting](#Time_ParsingFormatting "Time Parsing/Formatting")
    *   [Time Parse Example](#Time_Parse_Example "Time Parse Example")
    *   [Time Formatting Example](#Time_Formatting_Example "Time Formatting Example")
*   [Time Diff](#Time_Diff "Time Diff")
*   [Time Conversion](#Time_Conversion "Time Conversion ")
    *   [Convert time between different timezones](#Convert_time_between_different_timezones "Convert time between different timezones")*  *# **Overview**

**Time** or **Date** is represented in Go using **time.Time** struct. time can be also be represented as a

*   **Unix Time (Also known as Epoch Time)** – It is the number of seconds elapsed since 00:00:00 UTC on 1 January 1970\. This time is also known as the Unix epoch.

# **Structure**

**time.Time** object is used to represent a specific point in time. The **time.Time** struct is as below

```
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

As you can notice that every **time.Time** object has an associated **location** value which is used to determine the minute, hour, month, day and year corresponding to that time.

# **Create a new time**

## **Using time.Now()**

This function can be used to get the current local timestamp. The signature of the function is

```
func Now() Time
```

## **Using time.Date()**

This function returns the time which is **yyyy-mm-dd hh:mm:ss + nsec** nanoseconds with the appropriate time zone corresponding to the given location. The signature of the function is:

```
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

# **Understanding Duration**

**duration** is the time that has elapsed between two instants of time. It is represented as **int64nanosecond** count. So duration is nothing in Go but just a number representing time in nanoseconds. So if duration value is  equal to **1000000000** then it represents **1 sec** or **1000 milliseconds** or **10000000000 nanoseconds**

As an example duration between two time values 1 hour apart will be below value which is equal number of nanoseconds in 1 hour.

```
1 *60*60*1000*1000*1000
```

It is represented as below in the **time** package.

```
type Duration int64
```

Below are some common duration which are defined in **time** package

```
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```

Some of the function defined on **time.Time** object that returns the **Duration** are

*   **func (t Time) Sub(u Time) Duration** – It returns the duration t-u
*   **func Since(t Time) Duration –** It returns the duration which has elapsed since t
*   **func Until(t Time) Duration** – It returns the duration until t

# **Add or Subtract to a time**

Now that you have understood what duration , let’s see how we can add or subtract to a time instance

**time** package in golang defines two ways of adding or subtracting to a time.

*   **Add** function – It is used to add/subtract a duration to time t. Since duration can be represented in hours, minutes, seconds, milliseconds, microseconds and nanoseconds, therefore Add function can be used to add/subtract hours, minutes, seconds, milliseconds, microseconds and nanoseconds from a time . Its signature is

```
func (t Time) Add(d Duration) Time
```

*   **AddDate** function – It is used to add/subtract years, months and days to time t. Its signature is

```
func (t Time) AddDate(years int, months int, days int) Time
```

Note: Positive values are used to add to time and negative values are used to subtract. Let’s see a working example of Add and Subtract to time.

## **Add to time**

Below code can be used to add to time

```
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

**Output:**

```
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

## **Subtract to time**

Below code can be used to subtract to time

```
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

**Output:**

```
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

# **Time Parsing/Formatting**

If you have worked with time/date formatting/parsing in other languages you might have noticed that the other languages use special placeholders for time/date formatting. For eg ruby language uses

*   %d for day
*   %Y for year

etc

Golang, instead of using codes such as above, uses date and time format placeholders that look like date and time only. Go uses standard time, which is:

```
Mon Jan 2 15:04:05 MST 2006  (MST is GMT-0700)
or 
01/02 03:04:05PM '06 -0700
```

So if you notice go uses

*   01 for day of the month ,
*   02 for the month
*   03 for hours ,
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

## **Time Parse Example**

Now coming back to **time.Parse**. The signature of the function is

```
func Parse(layout, value string) (Time, error)
```

**time.Parse** function takes in two arguments

*   First argument is the layout consisting of time format placeholder

*   Second argument is the actual formatted string representing a time.

The way you have to go about this is to make sure that the layout string (first argument ) matches the string representation (second argument) of the time you want to parse into time.Time. For parsing

*   For parsing **2020-01-29**, layout string should be **06-01-02** or **2006-01-02** or something which maps correctly based on above placeholder table.

*   Similarly for parsing **“2020-Jan-29 Wednesday 12:19:25”** the layout string can be **“2006-Jan-02 Monday 03:04:05”**

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
```

## **Time Formatting Example**

**time.Format** function can be used to format time to a string representation. The signature of the function is

```
func (t Time) Format(layout string)
```

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

# **Time Diff**

**time** package has a method **Sub** which can be used to get the difference between two different time values. The signature of the function is

```
func (t Time) Sub(u Time) Duration
```

```
currentTime := time.Now()
oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
diff := currentTime.Sub(oldTime)
```

# **Time Conversion**

Below code shows conversion of

*   time.Time to Unix Timestamp
*   Unix Timestamp to time.Time

```
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

**Output:**

```
timeUnix 1257894000
time.Time: 2009-11-10 23:00:00 +0000 UTC
```

## **Convert time between different timezones**

The **In** function can be used to change the **location** associated with a particular **time.Time** object. Whenever the **In** function is called on any **time.Time** object (say t)  then,

*   A copy of **t** is created representing the same time instant.

*   t’s location is set to the location passed to In function for display purposes

*   **t** is returned back

Let’s see the below working code which can be used to change the location value associated with a particular time.

```
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

**Output:**

```
UTC Time: 2020-01-31 18:09:41.705858 +0000 UTC
Berlin Time: 2020-01-31 19:09:41.705858 +0100 CET
New York Time: 2020-01-31 13:09:41.705858 -0500 EST
Dubai Time: 2020-01-31 22:09:41.705858 +0400 +04
```

*   [complete guide](https://golangbyexample.com/tag/complete-guide/)*   [format](https://golangbyexample.com/tag/format/)*   [parse](https://golangbyexample.com/tag/parse/)*   [timezone](https://golangbyexample.com/tag/timezone/)*   [using](https://golangbyexample.com/tag/using/)*