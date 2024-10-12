<!--yml
category: 未分类
date: 2024-10-13 06:07:47
-->

# Get age given a DOB in Go (Golang)

> 来源：[https://golangbyexample.com/get-age-given-dob-go/](https://golangbyexample.com/get-age-given-dob-go/)

This tutorial will talk about how given a date of birth we can compute the age of a person. **go-age** comes to our rescue for doing that. It also takes into account the complexities of leap year while calculating age.

See a below working example:

```
package main

import (
    "fmt"
    "time"
    age "github.com/bearbin/go-age"
)

func main() {
    dob := getDOB(2011, 4, 2)
    fmt.Printf("Age is %d\n", age.Age(dob))
}

func getDOB(year, month, day int) time.Time {
    dob := time.Date(year, time.Month(month), day, 0, 0, 0, 0, time.UTC)
    return dob
}
```

**Output:**

```
Age is 8
```

*   [age](https://golangbyexample.com/tag/age/)*   [golang](https://golangbyexample.com/tag/golang/)