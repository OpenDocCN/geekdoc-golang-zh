<!--yml

类别：未分类

日期：2024-10-13 06:09:25

-->

# 从 Go (Golang) 执行 shell 文件

> 来源：[https://golangbyexample.com/execute-shell-file-golang/](https://golangbyexample.com/execute-shell-file-golang/)

目录

**   [概述：](#Overview "概述：")

+   [代码：](#Code "代码：")*  *# **概述：**

**os/exec** 包可用于从 Go 中触发任何操作系统命令。也可以用于触发 .sh 文件。

+   首先，在同一目录下创建一个 sample.sh 文件。确保它以 #!/bin/sh 或 #!/bin/bash 开头。

**sample.sh**

```go
#!/bin/sh
echo "Triggered from .go file" > out.out
```

+   使此 sample.sh 文件可执行

```go
chmod +x sample.sh
```

# **代码：**

在同一目录下创建一个 **main.go** 文件

```go
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

**输出：**

```go
A out.out file will be created in the same directory
```

+   [bash](https://golangbyexample.com/tag/bash/)*
