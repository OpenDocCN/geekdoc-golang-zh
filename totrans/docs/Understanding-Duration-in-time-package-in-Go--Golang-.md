<!--yml
category: 未分类
date: 2024-10-13 06:08:22
-->

# Understanding Duration in time package in Go (Golang)

> 来源：[https://golangbyexample.com/understanding-duration-go/](https://golangbyexample.com/understanding-duration-go/)

**duration** is the time that has elapsed between two instants of time. It is represented as **int64nanosecond** count. So duration is nothing in Go but just a number representing time in nanoseconds in Go. So if duration value is equal to **1000000000** then it represents **1 sec** or **1000 milliseconds** or **10000000000 nanoseconds**. Since duration is **int64,** the largest duration that can be represented is 290 years apart. So duration is used to capture the number of nanoseconds between two **time.Time** objects. As an example duration between two time values 1 hour apart will be below value which is equal to the number of nanoseconds in 1 hour.

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

type **Duration** also defines several utility function which can be used to convert a duration value to-

*   Hours
*   Minutes
*   Seconds

The print value of duration is represented as in the form “10h8m0.5s” where **h** representing **hour**, **m** representing **minute** and **s** representing **seconds** respectively

Let’s see a working code of what we have learned so far

```
package main

import (
    "fmt"
    "time"
)

func main() {
    now := time.Now()
    oldTime := time.Date(2020, 1, 2, 0, 0, 0, 0, time.UTC)
    diff := now.Sub(oldTime)
    //diff is of type Duration now

    //In hours
    fmt.Printf("Hours: %f\n", diff.Hours())

    //In minutes
    fmt.Printf("Minutes: %f\n", diff.Minutes())

    //In seconds
    fmt.Printf("Seconds: %f\n", diff.Seconds())

    //time Since
    fmt.Println(time.Since(now.Add(-time.Hour * 1)))

    //time until
    fmt.Println(time.Until(now.Add(time.Hour * 1)))
}
```

**Output:**

```
Hours: 714.565222
Minutes: 42873.913306
Seconds: 2572434.798350
1h0m0.000093629s
59m59.999888401s
```