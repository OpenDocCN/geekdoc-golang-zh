<!--yml

分类：未分类

日期：2024-10-13 06:10:35

-->

# 在 Go 中理解 WaitGroup (Golang)

> 来源：[`golangbyexample.com/understanding-waitgroup-go/`](https://golangbyexample.com/understanding-waitgroup-go/)

**WaitGroup** 是 Go 的 **sync** 包中的一个结构体。它通常用于等待一组 goroutine 完成执行。WaitGroup 具有以下方法：

+   **Add** 方法用于设置要等待的 goroutine 数量。

+   **Done** 方法由每个 goroutine 在完成时调用。

+   **Wait** 方法用于阻塞，直到所有 goroutine 完成并调用了 **Done** 方法。

让我们来看一个工作的代码：

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var wg sync.WaitGroup
    wg.Add(2)
    go sleep(&wg, time.Second*1)
    go sleep(&wg, time.Second*2)
    wg.Wait()
    fmt.Println("All goroutines finished")
}

func sleep(wg *sync.WaitGroup, t time.Duration) {
    defer wg.Done()
    time.Sleep(t)
    fmt.Println("Finished Execution")
}
```

**输出：**

```go
Finished Execution
Finished Execution
All goroutines finished
```
