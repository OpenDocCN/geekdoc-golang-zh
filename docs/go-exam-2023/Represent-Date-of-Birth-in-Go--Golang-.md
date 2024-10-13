<!--yml

类别：未分类

日期：2024-10-13 06:07:33

-->

# 在 Go (Golang) 中表示出生日期

> 来源：[https://golangbyexample.com/dob-golang/](https://golangbyexample.com/dob-golang/)

**time** 包的 **time.Date** 函数可用于创建一个特定日期，以表示出生日期。

请参见以下示例

+   `getDOB` 是一个接受年份、月份和日期并返回日期的函数。

```go
package main

import (
    "fmt"
    "time"
)

const (
    //TimeFormat1 to format date into
    TimeFormat1 = "2006-01-02"
    //TimeFormat2 Other format to format date time
    TimeFormat2 = "January 02, 2006"
)

func main() {
    dob := getDOB(2011, 4, 2)
    fmt.Println(dob.Format(TimeFormat1))
    fmt.Println(dob.Format(TimeFormat2))
}

func getDOB(year, month, day int) time.Time {
    dob := time.Date(year, time.Month(month), day, 0, 0, 0, 0, time.UTC)
    return dob
}
```

**输出：**

```go
2011-04-02
April 02, 2011
```

+   [日期](https://golangbyexample.com/tag/date/)*   [出生日期](https://golangbyexample.com/tag/dob/)*   [包](https://golangbyexample.com/tag/package/)
