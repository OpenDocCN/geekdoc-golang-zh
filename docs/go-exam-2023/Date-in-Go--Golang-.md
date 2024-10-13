<!--yml

类别：未分类

日期：2024-10-13 06:08:35

-->

# Go 中的日期 (Golang)

> 来源：[https://golangbyexample.com/date-in-golang/](https://golangbyexample.com/date-in-golang/)

在 Go 中，日期仅通过 **time.Time** 结构表示。Go 中没有单独的 Date 结构。可以使用 **time.Date** 函数构造日期。该函数返回格式为 **yyyy-mm-dd hh:mm:ss + nsec** 的时间，包含与给定位置对应的适当时区。函数的签名是：

```
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
```

从签名中可以看出，函数的参数是

+   年

+   月

+   天

+   小时

+   分

+   秒

+   毫秒

+   位置

关于 **time.Date** 函数的一些注意事项

+   如果传入的位置信息为 nil，Date 函数将会引发 panic。

+   月、日、时、分、秒、纳秒值会被规范化。因此，如果传入的月份是 14，它将被转换为 2。

让我们看看一个工作示例：

```
package main

import (
    "fmt"
    "time"
)

func main() {
    t := time.Date(2021, time.Month(2), 21, 1, 10, 30, 0, time.UTC)
    fmt.Println(t)
}
```

**输出：**

```
2021-02-21 01:10:30 +0000 UTC
```
