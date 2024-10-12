<!--yml
category: 未分类
date: 2024-10-13 06:07:25
-->

# Touch a file in Go (Golang)

> 来源：[https://golangbyexample.com/touch-file-golang/](https://golangbyexample.com/touch-file-golang/)

Touching a file means

*   Create an empty file if the file doesn’t already exist

*   If the file already exists then update the modified time of the file.

```
package main

import (
    "fmt"
    "log"
    "os"
    "time"
)

func main() {
    fileName := "temp.txt"
    _, err := os.Stat(fileName)
    if os.IsNotExist(err) {
        file, err := os.Create("temp.txt")
        if err != nil {
            log.Fatal(err)
        }
        defer file.Close()
    } else {
        currentTime := time.Now().Local()
        err = os.Chtimes(fileName, currentTime, currentTime)
        if err != nil {
            fmt.Println(err)
        }
    }
}
```

**Output:**

When running the first time it will create the file. After the first time, it will update the modified time of the file.

*   [empty](https://golangbyexample.com/tag/empty/)*   [file](https://golangbyexample.com/tag/file/)