# 结构体字段对齐

> 原文：[`goperf.dev/01-common-patterns/fields-alignment/`](https://goperf.dev/01-common-patterns/fields-alignment/)

在优化 Go 程序以获得性能时，结构体布局和内存对齐往往被忽视——然而，它们对内存使用和缓存效率有可测量的影响。Go 会根据平台特定的规则自动对齐结构体字段，插入填充以满足对齐约束。理解和控制内存对齐不仅是一个低级细节——它可以对您的 Go 程序的性能产生实际影响，尤其是在紧密循环或高吞吐量系统中。适当的对齐可以减少整体内存占用，更好地利用 CPU 缓存，并消除在负载下累积的微妙性能惩罚。

## 为什么对齐很重要

现代处理器针对可预测的内存访问进行了优化。当结构体字段未对齐或跨越缓存行时，处理器通常需要做额外的工作来获取数据。这可能意味着额外的内存周期、更多的缓存未命中，以及整体性能的降低。在日常代码中，这些成本很容易被忽视，但在对吞吐量或延迟敏感的代码中会迅速显现。在 Go 中，结构体字段根据其类型要求进行对齐，编译器会插入填充字节以满足这些约束。如果字段排列不当，不必要的填充可能会显著增加结构体的大小，影响内存使用和带宽。

考虑以下两个结构体：

```go
type PoorlyAligned struct {
    flag bool
    count int64
    id byte
}

type WellAligned struct {
    count int64
    flag bool
    id byte
} 
```

在 64 位系统上，`PoorlyAligned` 由于字段之间的填充需要 24 字节，而 `WellAligned` 通过按从大到小的对齐要求排序字段，可以适应 16 字节的空间。

## 基准测试影响

我们通过分配每种结构体类型的 1000 万个实例，并测量分配时间和内存使用来对两种结构体布局进行了基准测试：

```go
func BenchmarkPoorlyAligned(b *testing.B) {
    for b.Loop() {
        var items = make([]PoorlyAligned, 10_000_000)
        for j := range items {
            items[j].count = int64(j)
        }
    }
}

func BenchmarkWellAligned(b *testing.B) {
    for b.Loop() {
        var items = make([]WellAligned, 10_000_000)
        for j := range items {
            items[j].count = int64(j)
        }
    }
} 
```

基准测试结果

| 基准测试 | 迭代次数 | 每次操作时间 (ns) | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| 差对齐-14 | 177 | 20,095,621 | 240,001,029 | 1 |
| WellAligned-14 | 186 | 19,265,714 | 160,006,148 | 1 |

在一个包含 1000 万个结构体的测试中，`WellAligned` 版本比其差对齐版本少使用了 80MB 的内存，并且运行速度也略快。这不仅仅关乎节省 RAM；它展示了结构体布局如何直接影响分配行为和内存带宽。当您处理大量数据或性能关键路径时，重新排列字段以获得更好的对齐可以带来可测量的收益，而所需的工作量却很小。

## 避免并发工作负载中的伪共享

除了内存布局效率外，结构体对齐在并发系统中也起着至关重要的作用。当多个 goroutine 访问同一结构体上同一 CPU 缓存行的不同字段时，它们可能会遭受假共享——即对一个字段的更改会导致另一个字段的无效化，即使它们在逻辑上无关。

在现代 CPU 上，典型的缓存行宽度为 64 字节。当在内存中访问结构体时，CPU 会加载包含它的整个缓存行，而不仅仅是特定的字段。这意味着同一 64 字节块内的两个无关字段都将位于同一行——即使它们被不同的 goroutine 独立使用。如果一个 goroutine 向其字段写入，缓存行就会失效，必须在其他核心上重新加载，导致由于假共享而性能下降。

为了说明，我们比较了两个结构体——一个容易受到假共享的影响，另一个通过填充来分隔缓存行之间的字段：

```go
type SharedCounterBad struct {
    a int64
    b int64
}

type SharedCounterGood struct {
    a int64
    _ [56]byte // Padding to prevent a and b from sharing a cache line
    b int64
} 
```

每个字段由一个单独的 goroutine 增加一百万次：

```go
func BenchmarkFalseSharing(b *testing.B) {
    var c SharedCounterBad  // (1)
    var wg sync.WaitGroup

    for b.Loop() {
        wg.Add(2)
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.a++
            }
            wg.Done()
        }()
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.b++
            }
            wg.Done()
        }()
        wg.Wait()
    }
} 
```

1.  `FalseSharing`和`NoFalseSharing`基准测试是相同的，除了我们将使用`SharedCounterGood`作为`NoFalseSharing`基准测试。

基准测试结果：

| 基准测试 | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- |
| FalseSharing | 996,234 | 55 | 2 |
| NoFalseSharing | 958,180 | 58 | 2 |

在两个字段之间添加填充可以防止假共享，从而带来可测量的性能提升。带有填充的版本完成速度提高了约~3.8%（该值可能在重新运行之间变化，从 3%到 6%），这在紧密的并发循环或高频计数器中可能带来差异。这也展示了假共享可能由于无效化开销而不规则地影响内存使用。

<details class="example"><summary>显示完整的基准测试文件</summary>

```go
package perf

import (
    "sync"
    "testing"
)

// types-simple-start
type PoorlyAligned struct {
    flag bool
    count int64
    id byte
}

type WellAligned struct {
    count int64
    flag bool
    id byte
}
// types-simple-end

// simple-start
func BenchmarkPoorlyAligned(b *testing.B) {
    for b.Loop() {
        var items = make([]PoorlyAligned, 10_000_000)
        for j := range items {
            items[j].count = int64(j)
        }
    }
}

func BenchmarkWellAligned(b *testing.B) {
    for b.Loop() {
        var items = make([]WellAligned, 10_000_000)
        for j := range items {
            items[j].count = int64(j)
        }
    }
}
// simple-end

// types-shared-start
type SharedCounterBad struct {
    a int64
    b int64
}

type SharedCounterGood struct {
    a int64
    _ [56]byte // Padding to prevent a and b from sharing a cache line
    b int64
}
// types-shared-end

// shared-start

func BenchmarkFalseSharing(b *testing.B) {
    var c SharedCounterBad  // (1)
    var wg sync.WaitGroup

    for b.Loop() {
        wg.Add(2)
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.a++
            }
            wg.Done()
        }()
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.b++
            }
            wg.Done()
        }()
        wg.Wait()
    }
}
// shared-end

func BenchmarkNoFalseSharing(b *testing.B) {
    var c SharedCounterGood
    var wg sync.WaitGroup

    for b.Loop() {
        wg.Add(2)
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.a++
            }
            wg.Done()
        }()
        go func() {
            for i := 0; i < 1_000_000; i++ {
                c.b++
            }
            wg.Done()
        }()
        wg.Wait()
    }
} 
```</details>

## 当对齐结构体

总是使结构体对齐。实现起来是免费的，并且通常会导致更好的内存效率，而无需更改任何逻辑——只需调整字段顺序即可。

结构体对齐的指南：

+   按照从大到小的顺序排列字段。从较大的字段开始可以帮助编译器避免插入填充以满足对齐要求。较小的字段可以自然地填补空隙。

+   将相同大小的字段分组。这可以让编译器更有效地打包它们，并最小化浪费的空间。

+   当需要时有意添加填充。在并发代码中，将不同 goroutine 访问的字段分开可以防止假共享——这是一个微妙但代价高昂的问题，多个 goroutine 会争夺相同的缓存行。

+   避免混合大小不同的字段。混合大小会导致由于字段之间的额外对齐填充而造成内存使用效率低下。

+   使用[fieldalignment](https://pkg.go.dev/golang.org/x/tools/go/analysis/passes/fieldalignment)代码检查工具进行验证。此工具有助于在开发过程中自动捕获次优布局。
