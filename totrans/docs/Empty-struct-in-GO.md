<!--yml
category: 未分类
date: 2024-10-13 06:05:23
-->

# Empty struct in GO

> 来源：[https://golangbyexample.com/empty-struct-in-go/](https://golangbyexample.com/empty-struct-in-go/)

Empty struct **struct{}** in GO doesn’t occupy any memory. It is of zero byte.

Below are some of the uses of empty struct

*   Useful for creating implementations that require no data.

Eg. One example is in while implementing Null Object Design Pattern in Go. The Null object doesn’t have any data. See an example of Null Object Design Pattern in GO – [https://golangbyexample.com/null-object-design-pattern-golang/](https://golangbyexample.com/null-object-design-pattern-golang/)

*   Empty struct also a very good use case in a channel when you only want to use a channel for notification and not for actually passing in any data.

Eg: In context package of GO we have **cancelCtx** which is represented as below. See done channel is using an empty struct as it is only used for notifying cancellation and has no data value.

```
type cancelCtx struct {
    Context
    mu       sync.Mutex            // protects following fields
    done     chan struct{}         // created lazily, closed by first cancel call
    children map[canceler]struct{} // set to nil by the first cancel call
    err      error                 // set to non-nil by the first cancel call
}
```

*   Implementation of Set data structure. A set is a data structure that holds elements without any particular order. An element only appears once in a set. We use map[keyType]struct{} for set. struct{} is only just to let us know if an element exists in the set or not.

See this link for set implementation in golang – [https://golangbyexample.com/set-implementation-in-golang/](https://golangbyexample.com/set-implementation-in-golang/)

*   Perfect replacement for a channel of Boolean. A boolean does occupy some space as struct does not, hence more efficient to use a channel of struct{}. See below example

```
package main

import (
    "fmt"
    "time"
)

func main() {
    done := make(chan struct{}, 4)
    for i := 0; i < 4; i++ {
        go runasync(done)
    }
    for i := 0; i < 4; i++ {
        <-done
    }
    close(done)
    fmt.Printf("Finish")
}

func runasync(done chan<- struct{}) {
    time.Sleep(1 * time.Second)
    done <- struct{}{}
    return
}
```