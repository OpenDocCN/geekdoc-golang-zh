<!--yml

分类：未分类

日期：2024-10-13 06:05:23

-->

# GO 中的空结构体

> 来源：[https://golangbyexample.com/empty-struct-in-go/](https://golangbyexample.com/empty-struct-in-go/)

GO 中的空结构体 **struct{}** 不占用任何内存。它的大小为零字节。

以下是空结构体的一些用途

+   对于创建不需要数据的实现非常有用。

例如，在实现 GO 中的空对象设计模式时，空对象没有任何数据。查看 GO 中空对象设计模式的示例 – [https://golangbyexample.com/null-object-design-pattern-golang/](https://golangbyexample.com/null-object-design-pattern-golang/)

+   空结构体在通道中也是一个非常好的用例，当你只想用通道进行通知而不实际传递任何数据时。

例如：在 GO 的 context 包中，我们有 **cancelCtx**，如下所示。可以看到，done 通道使用了空结构体，因为它仅用于通知取消，并没有数据值。

```go
type cancelCtx struct {
    Context
    mu       sync.Mutex            // protects following fields
    done     chan struct{}         // created lazily, closed by first cancel call
    children map[canceler]struct{} // set to nil by the first cancel call
    err      error                 // set to non-nil by the first cancel call
}
```

+   集合数据结构的实现。集合是一种不按特定顺序保存元素的数据结构。元素在集合中只出现一次。我们使用 `map[keyType]struct{}` 来表示集合。`struct{}` 仅仅用于让我们知道某个元素是否存在于集合中。

查看此链接了解 Go 中的集合实现 – [https://golangbyexample.com/set-implementation-in-golang/](https://golangbyexample.com/set-implementation-in-golang/)

+   是布尔通道的完美替代。布尔值占用一定空间，而结构体不占用，因此使用 `struct{}` 的通道更有效。请看下面的示例。

```go
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
