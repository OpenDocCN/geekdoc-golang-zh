<!--yml
category: 未分类
date: 2024-10-13 06:08:00
-->

# Time Conversion in Go (Golang)

> 来源：[https://golangbyexample.com/time-conversion-in-golang/](https://golangbyexample.com/time-conversion-in-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Conversions](#Conversions "Conversions")
    *   [time.Time To Others](#timeTime_To_Others "time.Time To Others")
        *   [Time Unix](#Time_Unix "Time Unix")
        *   [Time Unix Millisecond](#Time_Unix_Millisecond "Time Unix Millisecond")
        *   [Time Unix Microsecond](#Time_Unix_Microsecond "Time Unix Microsecond")
        *   [Time Unix Nanosecond](#Time_Unix_Nanosecond "Time Unix Nanosecond")
        *   [Full Working Code](#Full_Working_Code "Full Working Code")
    *   [time Unix To Others](#time_Unix_To_Others "time Unix To Others")
        *   [time.Time](#timeTime "time.Time")
        *   [Time Unix Millisecond](#Time_Unix_Millisecond-2 "Time Unix Millisecond")
        *   [Time Unix Microsecond](#Time_Unix_Microsecond-2 "Time Unix Microsecond")
        *   [Time Unix Nanosecond](#Time_Unix_Nanosecond-2 "Time Unix Nanosecond")
        *   [Full Working Code](#Full_Working_Code-2 "Full Working Code")
    *   [time Unix MilliSecond To Others](#time_Unix_MilliSecond_To_Others "time Unix MilliSecond To Others")
        *   [time.Time](#timeTime-2 "time.Time")
        *   [Time Unix](#Time_Unix-2 "Time Unix ")
        *   [Time Unix Microsecond](#Time_Unix_Microsecond-3 "Time Unix Microsecond")
        *   [Time Unix Nanosecond](#Time_Unix_Nanosecond-3 "Time Unix Nanosecond")
        *   [Full Working Code:](#Full_Working_Code-3 "Full Working Code:")
    *   [time Unix MicroSecond To Others](#time_Unix_MicroSecond_To_Others "time Unix MicroSecond To Others")
        *   [time.Time](#timeTime-3 "time.Time")
        *   [Time Unix](#Time_Unix-3 "Time Unix ")
        *   [Time Unix Millisecond](#Time_Unix_Millisecond-3 "Time Unix Millisecond")
        *   [Time Unix Nanosecond](#Time_Unix_Nanosecond-4 "Time Unix Nanosecond")
        *   [Full Working Code:](#Full_Working_Code-4 "Full Working Code:")
    *   [time Unix NanoSecond To Others](#time_Unix_NanoSecond_To_Others "time Unix NanoSecond To Others")
        *   [time.Time](#timeTime-4 "time.Time")
        *   [Time Unix](#Time_Unix-4 "Time Unix ")
        *   [Time Unix Millisecond](#Time_Unix_Millisecond-4 "Time Unix Millisecond")
        *   [Time Unix Microsecond](#Time_Unix_Microsecond-4 "Time Unix Microsecond")
        *   [Full Working Code](#Full_Working_Code-5 "Full Working Code")*  *# **Overview**

Time can be represented in GO in below 5 formats:

*   **time.Time** object

*   **Unix Time (Also known as Epoch Time)** – It is the number of seconds elapsed since 00:00:00 UTC on 1 January 1970\. This time is also known as the Unix epoch

*   **Unix Nano** –  It is the number of nanoseconds elapsed since 00:00:00 UTC on 1 January 1970

*   **Unix MilliSecond** – It is the number of milliseconds elapsed since 00:00:00 UTC on 1 January 1970

*   **Unix MicroSecond** – It is the number of microseconds elapsed since 00:00:00 UTC on 1 January 1970

# **Conversions**

## **time.Time To Others**

Let:

```
timeT := time.Now()
```

### **Time Unix**

```
tUnix := timeT.Unix()
```

### **Time Unix Millisecond**

```
tUnixMilli := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Millisecond)
```

### **Time Unix Microsecond**

```
tUnixMicro := int64(time.Nanosecond) * timeT.UnixNano() / int64(time.Microsecond)
```

### **Time Unix Nanosecond**

```
tUnixNano := timeT.UnixNano()
```

### **Full Working Code**

```
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

**Output:**

```
time.Time 2020-01-27 23:03:21.528106 +0530 IST m=+0.000191621
timeUnix: 1580146401
timeUnixMilli: 1580146401528
timeUnixMicro: 1580146401528106
timeUnixNano: 1580146401528106000
```

## **time Unix To Others**

Let

```
tUnix := time.Now().Unix()
```

### **time.Time**

```
tUnix := timeT.Unix()
```

### **Time Unix Millisecond**

```
 tUnixMili := tUnix * int64(time.Microsecond)
```

### **Time Unix Microsecond**

```
tUnixMicro := tUnix * int64(time.Millisecond)
```

### **Time Unix Nanosecond**

```
tUnixNano := tUnix * int64(time.Second)
```

### **Full Working Code**

```
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

**Output:**

```
timeUnix 1580146705
time.Time: 2020-01-27 23:08:25 +0530 IST
timeUnixMilli: 1580146705000
timeUnixMicro: 1580146705000000
timeUnixNano: 1580146705000000000
```

## **time Unix MilliSecond To Others**

Let

```
tUnixMilli := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Millisecond)
```

### **time.Time**

```
tUnix := tUnixMilli / int64(time.Microsecond)
tUnixNanoRemainder := (tUnixMilli % int64(time.Microsecond)) * int64(time.Millisecond)
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **Time Unix**

```
tUnix = tUnixMilli / int64(time.Microsecond)
```

### **Time Unix Microsecond**

```
tUnixMicro := tUnixMilli * int64(time.Microsecond)
```

### **Time Unix Nanosecond**

```
tUnixNano := tUnixMilli * int64(time.Millisecond)
```

### **Full Working Code:**

```
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

**Output:**

```
timeMilli 1580146846747
time.Time: 2020-01-27 23:10:46.747 +0530 IST
timeUnix: 1580146846
timeUnixMicro: 1580146846747000
timeUnixNano: 1580146846747000000
```

## **time Unix MicroSecond To Others**

Let

```
tUnixMicro := int64(time.Nanosecond) * time.Now().UnixNano() / int64(time.Microsecond)
```

### **time.Time**

```
tUnix := tUnixMicro / int64(time.Millisecond)
tUnixNanoRemainder := (tUnixMicro % int64(time.Millisecond)) * int64(time.Microsecond)
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **Time Unix**

```
tUnix = tUnixMicro / int64(time.Millisecond)
```

### **Time Unix Millisecond**

```
tUnixMilli := tUnixMicro / int64(time.Microsecond)
```

### **Time Unix Nanosecond**

```
tUnixNano := tUnixMicro * int64(time.Microsecond)
```

### **Full Working Code:**

```
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

**Output:**

```
tUnixMicro 1580147023233931
time.Time: 2020-01-27 23:13:43.233931 +0530 IST
timeUnix: 1580147023
timeUnixMill: 1580147023233
timeUnixNano: 1580147023233931000
```

## **time Unix NanoSecond To Others**

Let

```
tUnixNano := time.Now().UnixNano()
```

### **time.Time**

```
tUnix := tUnixNano / int64(time.Second)
tUnixNanoRemainder := (tUnixNano % int64(time.Second))
timeT := time.Unix(tUnix, tUnixNanoRemainder)
```

### **Time Unix**

```
tUnix = tUnixNano / int64(time.Second)
```

### **Time Unix Millisecond**

```
tUnixMilli := tUnixNano / int64(time.Millisecond)
```

### **Time Unix Microsecond**

```
tUnixMicro := tUnixNano / int64(time.Microsecond)
```

### **Full Working Code**

```
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

**Output:**

```
tUnixNano 1580147160564568000
time.Time: 2020-01-27 23:16:00.564568 +0530 IST
timeUnix: 1580147160
timeUnixMill: 1580147160564
timeUnixMicro: 1580147160564568
```

*   [conversion](https://golangbyexample.com/tag/conversion/)*   [epcoh](https://golangbyexample.com/tag/epcoh/)*   [time.Time](https://golangbyexample.com/tag/time-time/)*