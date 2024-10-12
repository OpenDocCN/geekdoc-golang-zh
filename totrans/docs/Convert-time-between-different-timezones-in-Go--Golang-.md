<!--yml
category: 未分类
date: 2024-10-13 06:08:18
-->

# Convert time between different timezones in Go (Golang)

> 来源：[https://golangbyexample.com/convert-time-timezones-go/](https://golangbyexample.com/convert-time-timezones-go/)

Every **time.Time** object has an associated **location** value. When you change the location of any **time.Time** object to any other location, then that instant of time is not changed. Only the **location** value associated with that time changes. The **location** associated with the time.Time object only has a presentation or display logic.

The **In** function can be used to change the **location** associated with a particular **time.Time** object. Whenever the **In** function is called on any **time.Time** object (say t) then,

*   A copy of **t** is created representing the same time instant.
*   **t’s** location is set to the location passed to In function for display purposes
*   **t** is returned back

Let’s see the below-working code which can be used to change the location value associated with a particular time.

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

*   [time package](https://golangbyexample.com/tag/time-package/)*   [time.Time](https://golangbyexample.com/tag/time-time/)