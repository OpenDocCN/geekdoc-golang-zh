<!--yml
category: 未分类
date: 2024-10-13 06:10:31
-->

# Wait for all Go routines to finish execution in Golang

> 来源：[https://golangbyexample.com/wait-all-goroutines-go/](https://golangbyexample.com/wait-all-goroutines-go/)

**sync** package of golang provides **WaitGroup** struct which can be used to wait for collection of goroutines to finish execution. The WaitGroup provides:

*   **Add** method to set the number of goroutines to wait for.

*   **Done** method which is called by each of the goroutines when finished.

*   **Wait** method which is used to block till all goroutines have finished and have called the **Done** method

Let’s see a working code

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

**Output:**

```
Finished Execution
Finished Execution
All goroutines finished
```

*   [go](https://golangbyexample.com/tag/go/)*   [goroutines](https://golangbyexample.com/tag/goroutines/)