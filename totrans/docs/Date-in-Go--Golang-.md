<!--yml
category: 未分类
date: 2024-10-13 06:08:35
-->

# Date in Go (Golang)

> 来源：[https://golangbyexample.com/date-in-golang/](https://golangbyexample.com/date-in-golang/)

Date in Go is represented using **time.Time** struct only. There is no separate Date struct in Go. **time.Date** function can be used to construct a date. This function returns the time which is **yyyy-mm-dd hh:mm:ss + nsec** nanoseconds with the appropriate time zone corresponding to the given location. The signature of the function is:

```
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

As can be seen from the signature the arguments to the function are

*   Year
*   Month
*   Day
*   Hour
*   Min
*   Sec
*   Millisecond
*   Location

Some points worth noting about **time.Date** function

*   If the location passed is nil, the Date function will panic
*   The month, day, hour, min, sec, nsec values are normalized. So if we pass a month 14 it will be converted to 2

Let’s see a working example:

```
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Date(2021, time.Month(2), 21, 1, 10, 30, 0, time.UTC)
    fmt.Println(t)
}
```

**Output:**

```
2021-02-21 01:10:30 +0000 UTC
```