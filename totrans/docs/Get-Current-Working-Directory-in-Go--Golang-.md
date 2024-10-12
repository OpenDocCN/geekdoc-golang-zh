<!--yml
category: 未分类
date: 2024-10-13 06:07:20
-->

# Get Current Working Directory in Go (Golang)

> 来源：[https://golangbyexample.com/current-working-directory-golang/](https://golangbyexample.com/current-working-directory-golang/)

**os.Getwd()** is used to get the current working directory. Output will similar to **pwd** command on linux

```
package main
import (
    "fmt"
    "log"
    "os"
)

func main() {
    currentWorkingDirectory, err := os.Getwd()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Current Wroking Direcoty: %s", currentWorkingDirectory)
}
```

**Output:**

Current Working Direcoty: <will be current working directory on the machine>

*   [directory](https://golangbyexample.com/tag/directory/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [pwd](https://golangbyexample.com/tag/pwd/)