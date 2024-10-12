<!--yml
category: 未分类
date: 2024-10-13 06:07:51
-->

# Current Timestamp in Go (Golang)

> 来源：[https://golangbyexample.com/current-timestamp-in-golang/](https://golangbyexample.com/current-timestamp-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

In this tutorial, we will see how to get the current timestamp using the time package in Go. The current time can be represented in different ways

*   **time.Time** object

```
t := time.Now() //It will return time.Time object with current timestamp
```

*   **Unix Time (Also known as Epoch Time)** – It is the number of seconds elapsed since 00:00:00 UTC on 1 January 1970\. This time is also known as the Unix epoch.

```
t := time.Now().Unix() 
//Will return number of seconds passed since Unix epoch
```

*   **Unix Nano** –  It is number of nanoseconds elapsed since 00:00:00 UTC on 1 January 1970

```
t := time.Now().UnixNano() 
//Will return number of nano seconds passed since Unix epoch
```

*   **Unix MilliSecond** – It is the number of milliseconds elapsed since 00:00:00 UTC on 1 January 1970

```
t:= int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)/ time.Millisecond  
//Number of millisecond elapsed since Unix epoch
```

*   **Unix MicroSecond** – It is the number of microseconds elapsed since 00:00:00 UTC on 1 January 1970

```
t:= int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)/ time.Millisecond  
//Number of millisecond elapsed since Unix epoch
```

# Code

```
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Now() //It will return time.Time object with current timestamp
    fmt.Printf("time.Time %s\n", t)

    tUnix := t.Unix()
    fmt.Printf("timeUnix: %d\n", tUnix)

    tUnixNano := t.UnixNano()
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)

    tUnixMilli := int64(time.Nanosecond) * t.UnixNano() / int64(time.Millisecond)
    fmt.Printf("timeUnixMilli: %d\n", tUnixMilli)

    tUnixMicro := int64(time.Nanosecond) * t.UnixNano() / int64(time.Microsecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)
}
```

**Output:**

```
time.Time 2020-01-24 09:43:42.196901 UTC m=+0.000229700
timeUnix: 1579839222
timeUnixNano: 1579839222196901000
timeUnixMilli: 1579839222196
timeUnixMicro: 1579839222196901
```

*   [epoch](https://golangbyexample.com/tag/epoch/)*   [miliseconds](https://golangbyexample.com/tag/miliseconds/)*   [nanoseconds](https://golangbyexample.com/tag/nanoseconds/)*   [unix](https://golangbyexample.com/tag/unix/)*