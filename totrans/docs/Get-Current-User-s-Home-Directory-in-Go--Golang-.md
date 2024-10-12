<!--yml
category: 未分类
date: 2024-10-13 06:17:52
-->

# Get Current User’s Home Directory in Go (Golang)

> 来源：[https://golangbyexample.com/get-current-user-home-directory-go/](https://golangbyexample.com/get-current-user-home-directory-go/)

**Overview**

**‘os/user’** package can be used to get the current user home directory

Let’s see a working code

**Code**

```
package main

import (
    "fmt"
    "log"
    "os/user"
)

func main() {
    user, err := user.Current()
    if err != nil {
        log.Fatalf(err.Error())
    }
    homeDirectory := user.HomeDir
    fmt.Printf("Home Directory: %s\n", homeDirectory)
}
```

**Output**

```
Home Directory: "some_home_directory"
```

*   [directory](https://golangbyexample.com/tag/directory/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [home](https://golangbyexample.com/tag/home/)