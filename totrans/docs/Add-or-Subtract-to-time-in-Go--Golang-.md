<!--yml
category: 未分类
date: 2024-10-13 06:08:26
-->

# Add or Subtract to time in Go (Golang)

> 来源：[https://golangbyexample.com/add-or-subtract-to-time-go/](https://golangbyexample.com/add-or-subtract-to-time-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Add to time](#Add_to_time "Add to time ")
*   [Subtract to time](#Subtract_to_time "Subtract to time")*  *# **Overview**

**time** package in golang defines two ways of adding or subtracting to a time.

*   **Add** function – It is used to **add/subtract** a **duration** to **time** t. Since **duration** can be represented in hours, minutes, seconds, milliseconds, microseconds and nanoseconds, therefore **Add** function can be used to add/subtract hours, minutes, seconds, milliseconds, microseconds and nanoseconds from a time. Its signature is

```
func (t Time) Add(d Duration) Time
```

*   **AddDate** function – It is used to add/subtract years, months and days to time t. Its signature is

```
func (t Time) AddDate(years int, months int, days int) Time
```

Note: Positive values are used to add to time and negative values are used to subtract. Let’s see a working example of Add and Subtract to time.

# **Add to time**

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

**Output**

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

# **Subtract to time**

Below code can be used to subtract to time

```
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Now()

    //Subtract 1 hours
    newT := t.Add(-time.Hour * 1)
    fmt.Printf("Subtracting 1 hour:\n %s\n", newT)

    //Subtract 15 min
    newT = t.Add(-time.Minute * 15)
    fmt.Printf("Subtracting 15 minute:\n %s\n", newT)

    //Subtract 10 sec
    newT = t.Add(-time.Second * 10)
    fmt.Printf("Subtracting 10 sec:\n %s\n", newT)

    //Subtract 100 millisecond
    newT = t.Add(-time.Millisecond * 10)
    fmt.Printf("Subtracting 100 millisecond:\n %s\n", newT)

    //Subtract 1000 microsecond
    newT = t.Add(-time.Millisecond * 10)
    fmt.Printf("Subtracting 1000 microsecond:\n %s\n", newT)

    //Subtract 10000 nanosecond
    newT = t.Add(-time.Nanosecond * 10000)
    fmt.Printf("Subtracting 1000 nanosecond:\n %s\n", newT)

    //Subtract 1 year 2 month 4 day
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
```*