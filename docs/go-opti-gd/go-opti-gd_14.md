# 原子操作和同步原语

> 原文：[`goperf.dev/01-common-patterns/atomic-ops/`](https://goperf.dev/01-common-patterns/atomic-ops/)

在高并发系统中，性能不仅关乎你做了什么，还关乎你避免了什么。锁竞争、缓存行弹跳和内存栅栏在触及你的扩展天花板之前，默默地塑造了吞吐量。原子操作是 Go 提供的瘦工具之一，可以帮助你避开这些陷阱。

虽然 Go 提供了完整的同步原语套件，但仍有一类问题，锁似乎过于冗余。原子操作为低级协调提供了清晰和速度——计数器、标志和简单的状态机，尤其是在压力之下。

## 理解原子操作

原子操作允许在没有显式锁定机制（如互斥锁）的情况下安全地并发访问共享数据。`sync/atomic` 包提供了适用于计数器、标志或简单状态转换的低级原子内存原语。

原子操作的关键优势是在竞争条件下的性能。锁定引入了协调开销——当许多 goroutine 竞争互斥锁时，由于上下文切换和锁队列管理，性能可能会下降。原子操作通过直接在硬件级别使用 CPU 指令如`CAS`（比较并交换）来避免这一点，这使得它们特别有用：

+   高吞吐量计数器和标志

+   无锁队列和自由列表

+   锁太昂贵导致低延迟路径

### 内存模型与 C++的比较

理解内存模型对于推理并发至关重要。在 C++ 中，开发者可以通过内存顺序对原子操作进行细粒度控制，这允许他们在性能和一致性之间进行权衡。默认情况下，Go 的原子操作强制执行顺序一致性，这意味着它们的行为类似于 C++ 中的 `std::memory_order_seq_cst`。这是最强和最安全的内存顺序：

+   所有线程都以相同的顺序观察到原子操作。

+   在每个操作前后应用完整的内存屏障。

+   读取和写入不会在原子操作之间重新排序。

| C++内存顺序 | Go 等价 | 备注 |
| --- | --- | --- |
| `memory_order_seq_cst` | 所有 `atomic.*` 操作 | 完全顺序一致性 |
| `memory_order_acquire` | 未公开 | Go 中不可用 |
| `memory_order_release` | 未公开 | Go 中不可用 |
| `memory_order_relaxed` | 未公开 | Go 中不可用 |

Go 不公开较弱的内存模型，如 `relaxed`、`acquire` 或 `release`。这是一个有意的简化，旨在提高安全性和降低微妙的竞态条件风险。Go 中的所有原子操作都隐含了跨 goroutine 的同步，确保了正确的行为，无需手动内存栅栏。

这意味着你不需要在低级别上推理指令重排或内存可见性——但它也意味着你无法像 C++或 Rust 开发者使用松散原子操作那样进行性能微调。

Go 中存在对松散内存排序的低级访问（例如，在运行时或通过`go:linkname`），但它不安全或不支持在应用程序级代码中使用。

### 常见原子操作

+   `atomic.AddInt64`, `atomic.AddUint32`, 等：原子地添加值。

+   `atomic.LoadInt64`, `atomic.LoadPointer`: 原子读取值。

+   `atomic.StoreInt64`, `atomic.StorePointer`: 原子写入值。

+   `atomic.CompareAndSwapInt64`: 条件性地原子更新一个值。

### 当在现实生活中使用原子操作时：

#### 高吞吐量指标和计数器

跟踪请求数量、丢弃的数据包或其他轻量级统计信息：

```go
var requests atomic.Int64

func handleRequest() {
    requests.Add(1)
} 
```

这段代码允许多个 goroutine 在不需要锁的情况下安全地递增共享计数器。`atomic.AddInt64`确保每次添加都是原子性的，防止竞争条件并在高负载下保持高性能。

#### 快速、无锁的标志

在线程间共享的简单布尔状态：

```go
var shutdown atomic.Int32

func mainLoop() {
    for {
        if shutdown.Load() == 1 {
            break
        }
        // do work
    }
}

func stop() {
    shutdown.Store(1)
} 
```

这种模式允许一个 goroutine 向另一个 goroutine 发送停止信号。`atomic.LoadInt32`读取带有同步保证的标志，而`atomic.StoreInt32`以对所有 goroutine 可见的方式设置标志。这有助于实现安全的关闭信号。

#### 单次初始化

对于`sync.Once`不够灵活的场景——例如需要可重试或可重启的初始化——可以使用原子操作实现更精确的控制：

```go
import (
    "runtime"
    "sync/atomic"
    "unsafe"
)

var resource unsafe.Pointer
var initStatus int32 // 0: not started, 1: in progress, 2: completed

func getResource() *MyResource {
    if atomic.LoadInt32(&initStatus) == 2 {
        return (*MyResource)(atomic.LoadPointer(&resource))
    }

    if atomic.CompareAndSwapInt32(&initStatus, 0, 1) {
        newRes := expensiveInit() // initialization logic
        atomic.StorePointer(&resource, unsafe.Pointer(newRes))
        atomic.StoreInt32(&initStatus, 2)
        return newRes
    }

    for atomic.LoadInt32(&initStatus) != 2 {
        runtime.Gosched() // yield until the initializer finishes
    }
    return (*MyResource)(atomic.LoadPointer(&resource))
} 
```

这种模式使用一个显式的三态协议：

+   `0` = 未初始化

+   `1` = 初始化进行中

+   `2` = 已初始化

第一个成功将状态从`0`翻转至`1`的 goroutine 负责初始化。其余的 goroutine 在一个轻量级的自旋循环中等待，通过`runtime.Gosched()`短暂地让出 CPU，直到初始化完成。一旦状态翻转至`2`，它们读取资源并继续。

与`sync.Once`不同，这种方法避免了互斥锁的开销，并让你完全控制初始化的方式和时机。它非常适合高性能路径或需要部分或可重试初始化的系统。

#### 无锁队列或 Freelist 结构

构建高性能数据结构：

```go
type node struct {
    next *node
    val  any
}

var head atomic.Pointer[node]

func push(n *node) {
    for {
        old := head.Load()
        n.next = old
        if head.CompareAndSwap(old, n) {
            return
        }
    }
} 
```

这实现了一个无锁栈（后进先出队列）。它通过原子地替换头指针（只有当它未改变时）来重复尝试在列表头部插入节点——一个经典的`CAS`循环。它常用于对象池和工作窃取队列。

#### 减少锁竞争

在现实世界中，这种方法很常见，以减少不必要的锁定竞争，例如功能切换、一次性初始化路径或条件缓存机制。原子操作作为在获取更昂贵的锁之前的一个快速路径过滤器。

结合原子操作和互斥锁来控制昂贵的工作：

```go
if atomic.LoadInt32(&someFlag) == 0 {
    return
}
mu.Lock()
defer mu.Unlock()
// do something heavy 
```

当`someFlag`由另一个 goroutine 设置，而当前 goroutine 仅将其用作只读信号以确定是否应该继续时，此模式是有效的。它避免了在高吞吐量路径上不必要的锁定获取，例如当功能被禁用或任务已完成时的短路。

然而，如果同一个 goroutine 也负责设置标志，简单的加载后跟锁定是不安全的。另一个 goroutine 可能在检查和锁定之间交错，导致不一致的行为。

为了使操作安全且原子，使用`CompareAndSwap`：

```go
if !atomic.CompareAndSwapInt32(&someFlag, 0, 1) {
    return // work already in progress or completed
}
mu.Lock()
defer mu.Unlock()
// perform one-time expensive initialization 
```

此版本保证只有一个 goroutine 继续进行，其他 goroutine 提前退出。它确保对`someFlag`的检查和更新是原子的。

在这里，原子读取充当快速门卫。如果标志未设置，则不需要获取互斥锁。这避免了在高频代码路径上的不必要的锁定，提高了负载下的响应性。

## 同步原语

本节故意保持最小化。Go 的同步原语，如`sync.Mutex`、`sync.RWMutex`和`sync.Cond`，已经得到了充分的文档记录和广泛的理解。它们是管理共享内存和协调 goroutine 的基本工具，但它们不是本节的重点。

在本文的上下文中，我们仅将它们作为与原子操作的性能比较基准。当适当的时候，这些原语提供了清晰和正确性，但它们在高竞争场景中往往代价更高，而原子操作可以提供更精简的替代方案。

我们将使用它们作为对比点，以突出原子操作何时以及为什么可能提供性能优势。

## 基准影响

为了理解原子操作与互斥锁之间的影响，我们可以通过一个简单的基准来比较跨 goroutine 增加共享计数器所需的时间。

```go
func BenchmarkAtomicIncrement(b *testing.B) {
    var counter int64
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next() {
            atomic.AddInt64(&counter, 1)
        }
    })
}

func BenchmarkMutexIncrement(b *testing.B) {
    var (
        counter int64
        mu      sync.Mutex
    )
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next() {
            mu.Lock()
            counter++
            mu.Unlock()
        }
    })
} 
```

基准结果：

| 基准 | 迭代次数 | 每次操作时间 (ns) | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| BenchmarkAtomicIncrement-14 | 39,910,514 | 80.40 | 0 | 0 |
| BenchmarkMutexIncrement-14 | 32,629,298 | 110.7 | 0 | 0 |

原子操作在吞吐量和延迟方面都优于基于互斥锁的增量。在高竞争情况下，这种差异变得更加显著，避免锁定获取有助于减少上下文切换和调度开销。

<details class="example"><summary>显示完整的基准文件</summary>

```go
package perf

import (
    "testing"
    "sync/atomic"
    "sync"
)

// bench-start
func BenchmarkAtomicIncrement(b *testing.B) {
    var counter int64
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next() {
            atomic.AddInt64(&counter, 1)
        }
    })
}

func BenchmarkMutexIncrement(b *testing.B) {
    var (
        counter int64
        mu      sync.Mutex
    )
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next() {
            mu.Lock()
            counter++
            mu.Unlock()
        }
    })
}
// bench-end 
```</details>

## 在何时使用原子操作与互斥锁

原子操作在简单、高频的场景中尤为出色——如计数器、标志、协调信号等——在这些场景中，锁的开销会不成比例。它们避免了锁队列并减少了上下文切换。但它们也有局限性：无法对多个操作进行分组，没有回滚机制，并且当应用于其领域之外时，复杂性会增加。

互斥锁仍然是管理复杂共享状态、保护多步关键部分和维护不变性的正确工具。当逻辑扩展到几行以上时，它们更容易推理且通常更安全。

在原子操作和锁之间进行选择并不是关于意识形态，而是关于范围。当任务简单时，原子操作不会妨碍；当任务变得复杂时，锁能确保安全。
