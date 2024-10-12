<!--yml
category: 未分类
date: 2024-10-13 06:10:22
-->

# Change the modified/updated time and access time of a file in Go (Golang)

> 来源：[https://golangbyexample.com/change-updated-time-file-go/](https://golangbyexample.com/change-updated-time-file-go/)

**os.Chtimes()** function can be used to change the mtime(modified time) or atime(access time) of a file in Golang. Below is the signature of the function.

```
func Chtimes(name string, atime time.Time, mtime time.Time)
```

**Code:**

```
package main

import (
    "fmt"
    "os"
    "time"
)

func main() {
    fileName := "sample.txt"
    currentTime := time.Now().Local()

    //Set both access time and modified time of the file to the current time
    err := os.Chtimes(fileName, currentTime, currentTime)
    if err != nil {
        fmt.Println(err)
    }
}
```

**Output:**

```
Changes the atime and mtime of the file.
```

*   [updated](https://golangbyexample.com/tag/updated/)