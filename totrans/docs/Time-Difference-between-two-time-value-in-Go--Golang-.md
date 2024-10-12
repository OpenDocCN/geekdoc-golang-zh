<!--yml
category: 未分类
date: 2024-10-13 06:08:09
-->

# Time Difference between two time value in Go (Golang)

> 来源：[https://golangbyexample.com/time-difference-between-two-time-value-golang/](https://golangbyexample.com/time-difference-between-two-time-value-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code:](#Code "Code:")*  *# **Overview**

In **Go** time is represented by the time.Time struct. It struct has a method **Sub** which can be used to get the difference between two different time values.

```
currentTime := time.Now()
oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
diff := currentTime.Sub(oldTime)
```

The **Sub** function returns diff which is of type **Duration.** It is represented as **int64** nanosecond count. type Duration also defines several utility functions which can be used to get difference in

*   Hours
*   Minutes
*   Seconds
*   Nanoseconds

# **Code:**

Let’s see a working example of it:

```
package main

import (
    "fmt"
    "time"
)

func main() {
    currentTime := time.Now()
    oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
    diff := currentTime.Sub(oldTime)

    //In hours
    fmt.Printf("Hours: %f\n", diff.Hours())

    //In minutes
    fmt.Printf("Minutes: %f\n", diff.Minutes())

    //In seconds
    fmt.Printf("Seconds: %f\n", diff.Seconds())

    //In nanoseconds
    fmt.Printf("Nanoseconds: %d\n", diff.Nanoseconds())
}
```

**Output:**

```
Hours 712.985400
Minutes 42779.124024
Seconds 2566747.441457
Nanoseconds 2566747441457000
```*