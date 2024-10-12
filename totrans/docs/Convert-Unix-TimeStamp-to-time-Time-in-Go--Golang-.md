<!--yml
category: 未分类
date: 2024-10-13 06:08:31
-->

# Convert Unix TimeStamp to time.Time in Go (Golang)

> 来源：[https://golangbyexample.com/parse-unix-timestamp-time-go/](https://golangbyexample.com/parse-unix-timestamp-time-go/)

Time can be represented in GO in both **time.Time** and **Unix Timestamp** format. The Unix Timestamp also known as Epoch Time is the number of seconds elapsed since 00:00:00 UTC on 1 January 1970\. This time is also known as the Unix epoch. Below code shows the conversion of

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

*   [time.Time](https://golangbyexample.com/tag/time-time/)*   [unix](https://golangbyexample.com/tag/unix/)