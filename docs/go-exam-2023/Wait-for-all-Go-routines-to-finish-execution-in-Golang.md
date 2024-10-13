<!--yml

类别：未分类

日期：2024-10-13 06:10:31

-->

# 等待所有 Go 程序完成在 Golang 中的执行

> 来源：[https://golangbyexample.com/wait-all-goroutines-go/](https://golangbyexample.com/wait-all-goroutines-go/)

golang 的 **sync** 包提供 **WaitGroup** 结构，可以用来等待一组 goroutine 完成执行。WaitGroup 提供：

+   **添加**方法以设置要等待的 goroutine 数量。

+   **完成**方法在每个 goroutine 完成时被调用。

+   **等待**方法用于阻塞直到所有 goroutine 完成并调用 **完成** 方法

让我们看看一个有效的代码

```
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

```
Finished Execution
Finished Execution
All goroutines finished
```

+   [go](https://golangbyexample.com/tag/go/) *   [goroutines](https://golangbyexample.com/tag/goroutines/)
