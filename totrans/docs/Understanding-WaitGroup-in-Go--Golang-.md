<!--yml
category: 未分类
date: 2024-10-13 06:10:35
-->

# Understanding WaitGroup in Go (Golang)

> 来源：[https://golangbyexample.com/understanding-waitgroup-go/](https://golangbyexample.com/understanding-waitgroup-go/)

**WaitGroup** is a struct that is part of **sync** package in Go. It is generally used to wait for a collection of goroutines to finish execution. The WaitGroup has below methods

*   **Add** method to set the number of goroutines to wait for.

*   **Done** method which is called by each of the goroutines when finished.

*   **Wait** method which is used to block till all goroutines have finished and have called the **Done** method

Let’s see a working code:

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