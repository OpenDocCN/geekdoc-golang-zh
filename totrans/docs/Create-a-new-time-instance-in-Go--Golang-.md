<!--yml
category: 未分类
date: 2024-10-13 06:08:49
-->

# Create a new time instance in Go (Golang)

> 来源：[https://golangbyexample.com/create-new-time-instance-go/](https://golangbyexample.com/create-new-time-instance-go/)

In Go **time.Time** struct is used to represent an instance of time or date. Below are three ways of creating a new time instance

Table of Contents

 **   [Using time.Now()](#Using_timeNow "Using time.Now()")
*   [Using time.Date()](#Using_timeDate "Using time.Date()")
*   [Using time.Parse()](#Using_timeParse "Using time.Parse()")*  *# **Using time.Now()**

**time.Now()** function can be used to get the current local timestamp. The signature of the function is

```
func Now() Time
```

# **Using time.Date()**

time.Date() function takes in year, month, day, hour, min, sec, nanosecond and location and returns a time which is  **yyyy-mm-dd hh:mm:ss + nsec** with the appropriate timezone corresponding to the given location. The signature of the function is

```
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

# **Using time.Parse()**

time.Parse() can be used to convert a string representation of time into a **time.Time** instance. The signature of the function is 

```
func Parse(layout, value string) (Time, error)
```

**time.Parse** function takes in two arguments –

*   First argument is the layout consisting of time format placeholder

*   Second argument is the actual formatted string representing time.

Below is a working code example demonstrating all the three ways mentioned above

```
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

**Output:**

```
2020-02-03 11:34:10.85639 +0530 IST m=+0.000297951
2021-02-21 01:10:30 +0000 UTC
2020-01-29 00:00:00 +0000 UTC
```

*   [golang](https://golangbyexample.com/tag/golang/)*