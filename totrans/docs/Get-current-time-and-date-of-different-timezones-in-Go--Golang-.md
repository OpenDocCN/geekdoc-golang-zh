<!--yml
category: 未分类
date: 2024-10-13 06:08:13
-->

# Get current time and date of different timezones in Go (Golang)

> 来源：[https://golangbyexample.com/get-current-time-and-date-of-different-timezones-golang/](https://golangbyexample.com/get-current-time-and-date-of-different-timezones-golang/)

**time** package has a **LoadLocation(name string)** function which can be used to load a location of anywhere. Once you have the location, then **In** function can be used to change the location associated with a particular **time.Time** object.

For **LoadLocation(locationName)** function

*   If the locationName is “” or “UTC” , it return UTC

*   If the locationName is local, it returns the local Location

*   Else the location name has to be specified corresponding to IANA Time Zone Database (Refer wiki- [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)). Eg are
    *   Europe/Berlin
    *   America/New_York
    *   Asia/Dubai

Let’s see an example:

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

**Output**

```
UTC Time: 2020-01-31 17:36:26.481946 +0000 UTC
Local Time: 2020-02-01 01:36:26.481946 +0800 +08
Berlin Time: 2020-01-31 18:36:26.481946 +0100 CET
New York Time: 2020-01-31 12:36:26.481946 -0500 EST
Dubai Time: 2020-01-31 21:36:26.481946 +0400 +04
```

*   [timezone](https://golangbyexample.com/tag/timezone/)