<!--yml

类别：未分类

日期：2024-10-13 06:08:26

-->

# 在 Go（Golang）中加或减时间

> 来源：[https://golangbyexample.com/add-or-subtract-to-time-go/](https://golangbyexample.com/add-or-subtract-to-time-go/)

目录

**[概览](#Overview "Overview")

+   [Add to time](#Add_to_time "Add to time ")

+   [Subtract to time](#Subtract_to_time "Subtract to time")*  *# **概览**

golang 中的 **time** 包定义了两种添加或减去时间的方法。

+   **Add** 函数 – 用于 **添加/减去** 一个 **持续时间** 到 **时间** t。由于 **持续时间** 可以用小时、分钟、秒、毫秒、微秒和纳秒表示，因此 **Add** 函数可以用于从时间中添加/减去这些单位。它的签名是

```
func (t Time) Add(d Duration) Time
```

+   **AddDate** 函数 – 用于给时间 t 添加/减去年份、月份和天数。它的签名是

```
func (t Time) AddDate(years int, months int, days int) Time
```

注意：正值用于加时间，负值用于减时间。让我们看一个关于加减时间的工作示例。

# **加时间**

以下代码可以用于加时间

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

**输出**

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

# **减去时间**

以下代码可以用于减去时间

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

**输出：**

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
