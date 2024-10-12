<!--yml
category: 未分类
date: 2024-10-13 06:17:57
-->

# cd command in Go or change current working directory in Go (Golang)

> 来源：[https://golangbyexample.com/change-current-working-directory-go/](https://golangbyexample.com/change-current-working-directory-go/)

**Overview**

**os.Chdir()** is used to change the current working directory to the named directory in golang. It is similar to the cd command.

Below is the signature of the function

```
func Chdir(dir string) error
```

**Code**

```
package main

import (
    "fmt"
    "os"
)

func main() {
    os.Chdir("/Users")
    newDir, err := os.Getwd()
    if err != nil {
    }
    fmt.Printf("Current Working Direcotry: %s\n", newDir)
}
```

**Output:**

```
Current Working Direcoty: /Users
```

*   [cd command](https://golangbyexample.com/tag/cd-command/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)