<!--yml

类别：未分类

日期：2024-10-13 06:09:42

-->

# 获取当前运行/活动的 goroutine 数量

> 来源：[`golangbyexample.com/number-currently-running-active-goroutines/`](https://golangbyexample.com/number-currently-running-active-goroutines/)

**NumGoroutine** 函数来自 **runtime** 包，可用于了解当前运行/活动的 goroutine 数量。

[`golang.org/pkg/runtime/#NumGoroutine`](https://golang.org/pkg/runtime/#NumGoroutine)

以下是该函数的签名

```go
func NumGoroutine() int
```

**工作代码：**

```go
package main

import (
    "fmt"
    "runtime"
    "time"
)

func main() {
    for i := 0; i < 20; i++ {
        go execute()
    }
    fmt.Println(runtime.NumGoroutine())
}

func execute() {
    time.Sleep(time.Second * 10)
}
```

**输出：**

```go
21
```

+   [golang](https://golangbyexample.com/tag/golang/)*   [goroutines](https://golangbyexample.com/tag/goroutines/)
