<!--yml
category: 未分类
date: 2024-10-13 06:09:25
-->

# Execute shell file from Go (Golang)

> 来源：[https://golangbyexample.com/execute-shell-file-golang/](https://golangbyexample.com/execute-shell-file-golang/)

Table of Contents

 **   [Overview:](#Overview "Overview:")
*   [Code:](#Code "Code:")*  *# **Overview:**

**os/exec** package can be used to trigger any OS command from Go. The same can be used for triggering .sh file.

*   First, create a sample.sh file in the same directory. Make sure it either starts with #!/bin/sh or #!/bin/bash

**sample.sh**

```
#!/bin/sh
echo "Triggered from .go file" > out.out
```

*   Make this sample.sh file executable

```
chmod +x sample.sh
```

# **Code:**

Create a below **main.go** file in the same directory

```
package main

import (
    "fmt"
    "log"
    "os/exec"
)

func main() {
    out, err := exec.Command("/bin/sh", "sample.sh").Output()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(out)
}
```

**Output:**

```
A out.out file will be created in the same directory
```

*   [bash](https://golangbyexample.com/tag/bash/)*