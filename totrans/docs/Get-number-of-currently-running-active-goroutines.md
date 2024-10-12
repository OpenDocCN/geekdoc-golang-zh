<!--yml
category: 未分类
date: 2024-10-13 06:09:42
-->

# Get number of currently running/active goroutines

> 来源：[https://golangbyexample.com/number-currently-running-active-goroutines/](https://golangbyexample.com/number-currently-running-active-goroutines/)

**NumGoroutine** function of **runtime** package can be used to know the currently running/active goroutines.

[https://golang.org/pkg/runtime/#NumGoroutine](https://golang.org/pkg/runtime/#NumGoroutine)

Below is the signature of the function

```
func NumGoroutine() int
```

**Working Code:**

```
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

**Output:**

```
21
```

*   [golang](https://golangbyexample.com/tag/golang/)*   [goroutines](https://golangbyexample.com/tag/goroutines/)