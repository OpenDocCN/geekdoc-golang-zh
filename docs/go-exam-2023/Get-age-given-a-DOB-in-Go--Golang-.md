<!--yml

类别：未分类

日期：2024-10-13 06:07:47

-->

# 在Go（Golang）中根据DOB获取年龄

> 来源：[https://golangbyexample.com/get-age-given-dob-go/](https://golangbyexample.com/get-age-given-dob-go/)

本教程将讨论如何根据出生日期计算一个人的年龄。**go-age** 帮助我们完成这个任务。在计算年龄时，它还考虑了闰年的复杂性。

请看下面的工作示例：

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

**输出：**

```
Age is 8
```

+   [年龄](https://golangbyexample.com/tag/age/) *   [Go语言](https://golangbyexample.com/tag/golang/)
