<!--yml
category: 未分类
date: 2024-10-13 06:07:33
-->

# Represent Date of Birth in Go (Golang)

> 来源：[https://golangbyexample.com/dob-golang/](https://golangbyexample.com/dob-golang/)

**time.Date** function of **time** package can be used to create a particular date that can represent a DOB.

See below example

*   getDOB is the function that takes in a year, month and day and returns a date.

```
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

**Output:**

```
2011-04-02
April 02, 2011
```

*   [date](https://golangbyexample.com/tag/date/)*   [dob](https://golangbyexample.com/tag/dob/)*   [package](https://golangbyexample.com/tag/package/)