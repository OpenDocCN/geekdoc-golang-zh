<!--yml
category: 未分类
date: 2024-10-13 06:04:47
-->

# Go Logger Rotation

> 来源：[https://golangbyexample.com/go-logger-rotation/](https://golangbyexample.com/go-logger-rotation/)

This article is about how we rotate log files from within the go app without having to worry about the disk full alerts.

We are going to use [https://github.com/lestrrat/go-file-rotatelogs](https://github.com/lestrrat/go-file-rotatelogs) for log rotation.

**Example:** In the example below we are setting

*   **MaxAge** to 10 seconds. It sets the max-age of a log file before it is purged from the file system. Meaning the file will be deleted after 10 seconds. See

```
rotatelogs.WithMaxAge(time.Second*10)
```

*   **RotationTime** is 1 second**.** It sets the time when the file will be rotated. So the file will be rotated every second. See

```
rotatelogs.WithRotationTime(time.Second*1)
```

*   **Location** is /var/log/service/ . See

```
path := "/var/log/service/"
```

*   **Pattern** of the file is “old.UTC..2019-11-30.22:40:10” . See

```
 fmt.Sprintf("%s.%s", path, "%Y-%m-%d.%H:%M:%S")
```

*   **Link Path** is/var/log/service/current. This file would be a symlink for the actual file. See

```
rotatelogs.WithLinkName("/var/log/service/current")
```

**Code:**

```
package main

import (
    "fmt"
    "log"
    "time"
    rotatelogs "github.com/lestrrat/go-file-rotatelogs"
    "github.com/sirupsen/logrus"
)

func initiLogger() {
    path := "/var/log/service/old.UTC."
    writer, err := rotatelogs.New(
        fmt.Sprintf("%s.%s", path, "%Y-%m-%d.%H:%M:%S"),
        rotatelogs.WithLinkName("/var/log/service/current"),
        rotatelogs.WithMaxAge(time.Second*10),
        rotatelogs.WithRotationTime(time.Second*1),
    )
    if err != nil {
        log.Fatalf("Failed to Initialize Log File %s", err)
    }
    log.SetOutput(writer)
    return
}

func main() {
    initiLogger()
    for i := 0; i < 100; i++ {
        time.Sleep(time.Second * 1)
        log.Printf("Hello, World!")
    }
    fmt.Scanln()
}
```

Go to the location /var/log/service/. You will notice

*   Files are rotated every second.
*   Max age of the file is 10 seconds. So every file will be deleted after 10 seconds.

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [logger](https://golangbyexample.com/tag/logger/)*   [rotate](https://golangbyexample.com/tag/rotate/)