<!--yml
category: 未分类
date: 2024-10-13 06:17:01
-->

# Return exit status code in Go (Golang)

> 来源：[https://golangbyexample.com/return-exit-status-code-go/](https://golangbyexample.com/return-exit-status-code-go/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**‘os’** package of golang provides an **Exit** function that can be used to exit the current program with a status code.

*   Status code zero means success
*   Non-zero status code means an error.

Once this function is called the program exits immediately. Even the deferred functions are not called.

Also to note that status code should be in the range [0, 125]

```
func Exit(code int)
```

Let’s see a working code

# **Code**

```
package main

import (
    "fmt"
    "os"
)

func main() {
    success := true
    if success {
        fmt.Println("Success")
        os.Exit(0)
    } else {
        fmt.Println("Failure")
        os.Exit(1)
    }
}
```

**Output**

Try setting success to false to see a different output

```
Success
$ echo $?
0
```

*   [code](https://golangbyexample.com/tag/code/)*   [exit](https://golangbyexample.com/tag/exit/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [os](https://golangbyexample.com/tag/os/)*   [status](https://golangbyexample.com/tag/status/)*