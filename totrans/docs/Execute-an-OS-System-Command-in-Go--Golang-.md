<!--yml
category: 未分类
date: 2024-10-13 06:17:06
-->

# Execute an OS/System Command in Go (Golang)

> 来源：[https://golangbyexample.com/execute-os-system-command-golang/](https://golangbyexample.com/execute-os-system-command-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

**os/exec** package can be used to trigger any OS or system command from Go. It has two functions which can be used to achieve the same

*   **Command** – Used to create the cmd object

*   **Output** – It runs the command and returns the standard output

# **Code**

```
package main

import (
    "fmt"
    "log"
    "os/exec"
)

func main() {
    out, err := exec.Command("pwd").Output()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(out))
}
```

**Output:**

```
It will output the location of current working directory
```

*   [command](https://golangbyexample.com/tag/command/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [system](https://golangbyexample.com/tag/system/)*