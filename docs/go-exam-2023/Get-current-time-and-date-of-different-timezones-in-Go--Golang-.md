<!--yml

类别：未分类

日期：2024-10-13 06:08:13

-->

# 获取不同时区的当前时间和日期在 Go (Golang) 中

> 来源：[https://golangbyexample.com/get-current-time-and-date-of-different-timezones-golang/](https://golangbyexample.com/get-current-time-and-date-of-different-timezones-golang/)

**time** 包有一个 **LoadLocation(name string)** 函数，可以用来加载任何地方的位置。一旦你有了位置，**In** 函数可以用来改变与特定 **time.Time** 对象关联的位置。

对于 **LoadLocation(locationName)** 函数

+   如果 locationName 是 “” 或 “UTC”，则返回 UTC

+   如果 locationName 是本地的，它会返回本地位置

+   否则，位置名称必须对应于 IANA 时区数据库进行指定（参考维基 - [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)）。例如：

    +   欧洲/柏林

    +   美国/纽约

    +   亚洲/迪拜

让我们来看一个例子：

```
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now()
	loc, _ := time.LoadLocation("UTC")
	fmt.Printf("UTC Time: %s\n", now.In(loc))

	loc, _ = time.LoadLocation("Local")
	fmt.Printf("Local Time: %s\n", now.In(loc))

	loc, _ = time.LoadLocation("Europe/Berlin")
	fmt.Printf("Berlin Time: %s\n", now.In(loc))

	loc, _ = time.LoadLocation("America/New_York")
	fmt.Printf("New York Time: %s\n", now.In(loc))

	loc, _ = time.LoadLocation("Asia/Dubai")
	fmt.Printf("Dubai Time: %s\n", now.In(loc))

} 
```

**输出**

```
UTC Time: 2020-01-31 17:36:26.481946 +0000 UTC
Local Time: 2020-02-01 01:36:26.481946 +0800 +08
Berlin Time: 2020-01-31 18:36:26.481946 +0100 CET
New York Time: 2020-01-31 12:36:26.481946 -0500 EST
Dubai Time: 2020-01-31 21:36:26.481946 +0400 +04
```

+   [时区](https://golangbyexample.com/tag/timezone/)
