<!--yml

分类：未分类

日期：2024-10-13 06:08:00

-->

# Go（Golang）中的时间转换

> 来源：[`golangbyexample.com/time-conversion-in-golang/`](https://golangbyexample.com/time-conversion-in-golang/)

目录

**   概述

+   转换

    +   time.Time 到其他

        +   时间 Unix

        +   时间 Unix 毫秒

        +   时间 Unix 微秒

        +   时间 Unix 纳秒

        +   完整工作代码

    +   时间 Unix 到其他

        +   time.Time

        +   时间 Unix 毫秒

        +   时间 Unix 微秒

        +   时间 Unix 纳秒

        +   完整工作代码

    +   时间 Unix 毫秒到其他

        +   time.Time

        +   时间 Unix

        +   时间 Unix 微秒

        +   时间 Unix 纳秒

        +   完整工作代码：

    +   时间 Unix 微秒到其他

        +   time.Time

        +   时间 Unix

        +   时间 Unix 毫秒

        +   时间 Unix 纳秒

        +   完整工作代码：

    +   时间 Unix 纳秒到其他

        +   time.Time

        +   时间 Unix

        +   时间 Unix 毫秒

        +   时间 Unix 微秒

        +   完整工作代码*  *# **概述**

时间可以在 GO 中以以下 5 种格式表示：

+   **time.Time** 对象

+   **Unix 时间（也称为纪元时间）** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的秒数。此时间也称为 Unix 纪元

+   **Unix 纳秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的纳秒数

+   **Unix 毫秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的毫秒数

+   **Unix 微秒** – 自 1970 年 1 月 1 日 00:00:00 UTC 以来经过的微秒数

# **转换**

## **time.Time 到其他**

设：

```go
timeT := time.Now()
```

### **时间 Unix**

```go
tUnix := timeT.Unix()
```

### **时间 Unix 毫秒**

```go
tUnixMilli := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Millisecond)
```

### **时间 Unix 微秒**

```go
tUnixMicro := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Microsecond)
```

### **时间 Unix 纳秒**

```go
tUnixNano := timeT.UnixNano()
```

### **完整工作代码**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    timeT := time.Now() //It will return time.Time object with current timestamp
    fmt.Printf("time.Time %s\n", timeT)

    //Converstion to Time Unix also known as epoch time
    tUnix := timeT.Unix()
    fmt.Printf("timeUnix: %d\n", tUnix)

    //Conversion to Time Unix Millisecond
    tUnixMilli := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Millisecond)
    fmt.Printf("timeUnixMilli: %d\n", tUnixMilli)

    //Conversion to Time Unix Microsecond
    tUnixMicro := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Microsecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)

    //Conversion to Time Unix Nanosecond
    tUnixNano := timeT.UnixNano()
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)
}
```

**输出：**

```go
time.Time 2020-01-27 23:03:21.528106 +0530 IST m=+0.000191621
timeUnix: 1580146401
timeUnixMilli: 1580146401528
timeUnixMicro: 1580146401528106
timeUnixNano: 1580146401528106000
```

## **时间 Unix 到其他**

设

```go
tUnix := time.Now().Unix()
```

### **time.Time**

```go
tUnix := timeT.Unix()
```

### **时间 Unix 毫秒**

```go
 tUnixMili := tUnix * int64(time.Microsecond)
```

### **时间 Unix 微秒**

```go
tUnixMicro := tUnix * int64(time.Millisecond)
```

### **时间 Unix 纳秒**

```go
tUnixNano := tUnix * int64(time.Second)
```

### **完整工作代码**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tUnix := time.Now().Unix()
    fmt.Printf("timeUnix %d\n", tUnix)

    //Conversion to time.Time
    timeT := time.Unix(tUnix, 0)
    fmt.Printf("time.Time: %s\n", timeT)

    //Conversion to Time Unix Millisecond
    tUnixMili := tUnix * int64(time.Microsecond)
    fmt.Printf("timeUnixMilli: %d\n", tUnixMili)

    //Conversion to Time Unix Microsecond
    tUnixMicro := tUnix * int64(time.Millisecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)

    //Conversion to Time Unix Nanosecond
    tUnixNano := tUnix * int64(time.Second)
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)
}
```

**输出：**

```go
timeUnix 1580146705
time.Time: 2020-01-27 23:08:25 +0530 IST
timeUnixMilli: 1580146705000
timeUnixMicro: 1580146705000000
timeUnixNano: 1580146705000000000
```

## **时间 Unix 毫秒到其他**

让

```go
tUnixMilli := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Millisecond)
```

### **time.Time**

```go
tUnix := tUnixMilli / int64(time.Microsecond)
tUnixNanoRemainder := (tUnixMilli % int64(time.Microsecond)) * int64(time.Millisecond)
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **时间 Unix**

```go
tUnix = tUnixMilli / int64(time.Microsecond)
```

### **时间 Unix 微秒**

```go
tUnixMicro := tUnixMilli * int64(time.Microsecond)
```

### **时间 Unix 纳秒**

```go
tUnixNano := tUnixMilli * int64(time.Millisecond)
```

### **完整工作代码：**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tUnixMilli := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Millisecond)
    fmt.Printf("timeMilli %d\n", tUnixMilli)

    //Conversion to time.Time
    tUnix := tUnixMilli / int64(time.Microsecond)
    tUnixNanoRemainder := (tUnixMilli % int64(time.Microsecond)) * int64(time.Millisecond)
    timeT := time.Unix(tUnix, tUnixNanoRemainder)
    fmt.Printf("time.Time: %s\n", timeT)

    //Conversion to Time Unix
    tUnix = tUnixMilli / int64(time.Microsecond)
    fmt.Printf("timeUnix: %d\n", tUnix)

    //Conversion to Time Unix Microsecond
    tUnixMicro := tUnixMilli * int64(time.Microsecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)

    //Conversion to Time Unix Nanosecond
    tUnixNano := tUnixMilli * int64(time.Millisecond)
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)
}
```

**输出：**

```go
timeMilli 1580146846747
time.Time: 2020-01-27 23:10:46.747 +0530 IST
timeUnix: 1580146846
timeUnixMicro: 1580146846747000
timeUnixNano: 1580146846747000000
```

## **时间 Unix 微秒到其他**

让

```go
tUnixMicro := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Microsecond)
```

### **time.Time**

```go
tUnix := tUnixMicro / int64(time.Millisecond)
tUnixNanoRemainder := (tUnixMicro % int64(time.Millisecond)) * int64(time.Microsecond)
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **时间 Unix**

```go
tUnix = tUnixMicro / int64(time.Millisecond)
```

### **时间 Unix 毫秒**

```go
tUnixMilli := tUnixMicro / int64(time.Microsecond)
```

### **时间 Unix 纳秒**

```go
tUnixNano := tUnixMicro * int64(time.Microsecond)
```

### **完整工作代码：**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tUnixMicro := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Microsecond)
    fmt.Printf("tUnixMicro %d\n", tUnixMicro)

    //Conversion to time.Time
    tUnix := tUnixMicro / int64(time.Millisecond)
    tUnixNanoRemainder := (tUnixMicro % int64(time.Millisecond)) * int64(time.Microsecond)
    timeT := time.Unix(tUnix, tUnixNanoRemainder)
    fmt.Printf("time.Time: %s\n", timeT)

    //Converstion to Time Unix
    tUnix = tUnixMicro / int64(time.Millisecond)
    fmt.Printf("timeUnix: %d\n", tUnix)

    //Converstion to Time Unix Milli
    tUnixMilli := tUnixMicro / int64(time.Microsecond)
    fmt.Printf("timeUnixMill: %d\n", tUnixMilli)

    //Converstion to Time Unix Nano
    tUnixNano := tUnixMicro * int64(time.Microsecond)
    fmt.Printf("timeUnixNano: %d\n", tUnixNano)
}
```

**输出：**

```go
tUnixMicro 1580147023233931
time.Time: 2020-01-27 23:13:43.233931 +0530 IST
timeUnix: 1580147023
timeUnixMill: 1580147023233
timeUnixNano: 1580147023233931000
```

## **时间 Unix 纳秒到其他**

让

```go
tUnixNano := time.Now().UnixNano()
```

### **time.Time**

```go
tUnix := tUnixNano / int64(time.Second)
tUnixNanoRemainder := (tUnixNano % int64(time.Second))
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **时间 Unix**

```go
tUnix = tUnixNano / int64(time.Second)
```

### **时间 Unix 毫秒**

```go
tUnixMilli := tUnixNano / int64(time.Millisecond)
```

### **时间 Unix 微秒**

```go
tUnixMicro := tUnixNano / int64(time.Microsecond)
```

### **完整工作代码**

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tUnixNano := time.Now().UnixNano()
    fmt.Printf("tUnixNano %d\n", tUnixNano)

    //Conversion to time.Time
    tUnix := tUnixNano / int64(time.Second)
    tUnixNanoRemainder := (tUnixNano % int64(time.Second))
    timeT := time.Unix(tUnix, tUnixNanoRemainder)
    fmt.Printf("time.Time: %s\n", timeT)

    //Conversion to Time Unix
    tUnix = tUnixNano / int64(time.Second)
    fmt.Printf("timeUnix: %d\n", tUnix)

    //Conversion to Time Unix Milli
    tUnixMilli := tUnixNano / int64(time.Millisecond)
    fmt.Printf("timeUnixMilli: %d\n", tUnixMilli)

    //Conversion to Time Unix Micro
    tUnixMicro := tUnixNano / int64(time.Microsecond)
    fmt.Printf("timeUnixMicro: %d\n", tUnixMicro)
}
```

**输出：**

```go
tUnixNano 1580147160564568000
time.Time: 2020-01-27 23:16:00.564568 +0530 IST
timeUnix: 1580147160
timeUnixMill: 1580147160564
timeUnixMicro: 1580147160564568
```

+   [转换](https://golangbyexample.com/tag/conversion/) * [纪元](https://golangbyexample.com/tag/epcoh/) * [time.Time](https://golangbyexample.com/tag/time-time/) *
