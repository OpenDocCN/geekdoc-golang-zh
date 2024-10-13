<!--yml

类别：未分类

日期：2024-10-13 06:09:47

-->

# 检查 Go (Golang) 中是否设置了环境变量

> 来源：[https://golangbyexample.com/check-environment-variable-set-go/](https://golangbyexample.com/check-environment-variable-set-go/)

**os.LookupEnv** 函数可用于检查特定环境变量是否已设置。它返回一个布尔值，如果给定的环境变量已设置，则为 true，否则为 false。让我们来看一个工作代码：

```
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    //Set env a to b
    err := os.Setenv("a", "x")
    if err != nil {
        log.Fatal(err)
    }

    val, present := os.LookupEnv("a")
    fmt.Printf("a env variable present: %t\n", present)
    fmt.Println(val)

    val, present = os.LookupEnv("b")
    fmt.Printf("b env variable present: %t\n", present)
}
```

**输出：**

```
a env variable present: true
x
b env variable present: false
```

+   [环境变量](https://golangbyexample.com/tag/env/)
