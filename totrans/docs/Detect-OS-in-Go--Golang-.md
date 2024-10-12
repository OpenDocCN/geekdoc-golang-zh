<!--yml
category: 未分类
date: 2024-10-13 06:10:27
-->

# Detect OS in Go (Golang)

> 来源：[https://golangbyexample.com/detect-os-golang/](https://golangbyexample.com/detect-os-golang/)

**runtime.GOOS** constant can be used to detect the OS at runtime since this constant gets set only at runtime.

**runtime.GOARCH** can be used to know the architecture target of the running program.

To see all possible combinations of GOOS and GOARCH, run the command

```
go tool dist list
```

Below is the code to know the currently running OS:

```
package main

import (
    "fmt"
    "runtime"
)

func main() {
    os := runtime.GOOS
    switch os {
    case "windows":
        fmt.Println("Windows")
    case "darwin":
        fmt.Println("MAC operating system")
    case "linux":
        fmt.Println("Linux")
    default:
        fmt.Printf("%s.\n", os)
    }
}
```

**Output:**

```
Your current operating system
```