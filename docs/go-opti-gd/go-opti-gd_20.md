# Go中的批处理操作

> 原文：[https://goperf.dev/01-common-patterns/batching-ops/](https://goperf.dev/01-common-patterns/batching-ops/)

批处理是那些容易被忽视但非常有用的技术之一，当性能开始变得重要时。而不是一次处理一个操作，你可以将它们组合在一起——减少重复调用的开销，无论是击中网络、写入磁盘还是提交数据库。这是一个实用、低复杂度的方法，可以降低延迟并使你的系统吞吐量超出预期。

## 为什么批处理很重要

大多数系统不是因为单个操作太慢而挣扎——它们挣扎是因为它们做了太多的操作。每次调用数据库、API或文件系统都会增加一些固定成本：一个系统调用、一次网络往返，可能是一个锁或上下文切换。当这些成本在高负载工作负载中累积时，其影响很难忽视。批处理通过将这些调用合并成更少、更高效的作业单元来帮助，这通常会导致性能和资源使用方面的可测量收益。

考虑一个写入磁盘的日志服务：

```go
func logLine(line string) {
    f.WriteString(line + "\n")
} 
```

当每秒被调用数千次时，文件系统会被单个写系统调用淹没，这会显著降低性能。更好的方法可能是聚合日志条目并批量刷新：

```go
var batch []string

func logBatch(line string) {
    batch = append(batch, line)
    if len(batch) >= 100 {
        f.WriteString(strings.Join(batch, "\n") + "\n")
        batch = batch[:0]
    }
} 
```

使用批处理，每个写操作可以同时处理多个条目，减少系统调用开销并提高磁盘I/O效率。

警告

虽然批处理提供了显著的性能优势，但它也引入了数据丢失的风险。如果一个应用程序在批处理刷新之前崩溃，内存中的数据可能会丢失。处理关键或事务性数据的系统必须采用定期刷新、持久存储缓冲区或恢复机制等安全措施来减轻这种风险。

## 通用批处理器可能看起来如何

我们可以非常直接地实现一个通用批处理器：

```go
type Batcher[T any] struct {
    mu     sync.Mutex
    buffer []T
    size   int
    flush  func([]T)
}

func NewBatcher[T any](size int, flush func([]T)) *Batcher[T] {
    return &Batcher[T]{
        buffer: make([]T, 0, size),
        size:   size,
        flush:  flush,
    }
}

func (b *Batcher[T]) Add(item T) {
    b.mu.Lock()
    defer b.mu.Unlock()
    b.buffer = append(b.buffer, item)
    if len(b.buffer) >= b.size {
        b.flushNow()
    }
}

func (b *Batcher[T]) flushNow() {
    if len(b.buffer) == 0 {
        return
    }
    b.flush(b.buffer)
    b.buffer = b.buffer[:0]
} 
```

警告

这个批处理器实现期望你永远不会从你的`flush()`函数中调用`Batcher.Add(...)`。我们有限制是因为Go互斥锁[**不是**递归的](https://stackoverflow.com/questions/14670979/recursive-locking-in-go)。

这个批处理器与任何数据类型都兼容，使其成为聚合日志、指标、数据库写入或其他分组操作的灵活解决方案。内部，缓冲区充当队列，积累项目直到达到刷新阈值。使用`sync.Mutex`确保`Add()`和`flushNow()`对并发访问是安全的，这在大多数现实世界的系统中是必要的，在这些系统中，多个goroutine可能会写入批处理器。

从性能角度来看，确实，无锁实现——使用原子操作或并发环形缓冲区——可以在高负载下减少竞争并提高吞吐量。然而，这种设计更复杂，更难维护，通常只有在极端追求高并发或低延迟边界时才是合理的。对于大多数实际工作负载，基于`sync.Mutex`的设计在性能和可维护性之间提供了很好的平衡。

## 基准测试影响

为了验证批量性能，我们在三个类别中测试了六个场景：内存处理、文件I/O和CPU密集型哈希。每个类别都包括非批量版本和批量版本，所有基准测试每个操作运行超过10,000项。

<details class="example"><summary>显示基准文件</summary>

```go
package perf

import (
    "crypto/sha256"
    "encoding/hex"
    "fmt"
    "os"
    "strings"
    "testing"
)

var lines = make([]string, 10000)

func init() {
    for i := range lines {
        lines[i] = fmt.Sprintf("log entry %d %s", i, strings.Repeat("x", 100))
    }
}

// --- 1\. No I/O ---

func BenchmarkUnbatchedProcessing(b *testing.B) {
    for b.Loop() {
        for _, line := range lines {
            strings.ToUpper(line)
        }
    }
}

func BenchmarkBatchedProcessing(b *testing.B) {
    batchSize := 100
    for b.Loop() {
        for i := 0; i < len(lines); i += batchSize {
            end := i + batchSize
            if end > len(lines) {
                end = len(lines)
            }
            batch := strings.Join(lines[i:end], "|")
            strings.ToUpper(batch)
        }
    }
}

// --- 2\. With I/O ---

func BenchmarkUnbatchedIO(b *testing.B) {
    for b.Loop() {
        f, err := os.CreateTemp("", "unbatched")
        if err != nil {
            b.Fatal(err)
        }
        for _, line := range lines {
            _, _ = f.WriteString(line + "\n")
        }
        f.Close()
        os.Remove(f.Name())
    }
}

func BenchmarkBatchedIO(b *testing.B) {
    batchSize := 100
    for b.Loop() {
        f, err := os.CreateTemp("", "batched")
        if err != nil {
            b.Fatal(err)
        }
        for i := 0; i < len(lines); i += batchSize {
            end := i + batchSize
            if end > len(lines) {
                end = len(lines)
            }
            batch := strings.Join(lines[i:end], "\n") + "\n"
            _, _ = f.WriteString(batch)
        }
        f.Close()
        os.Remove(f.Name())
    }
}

// --- 3\. With Crypto ---

func hash(s string) string {
    h := sha256.Sum256([]byte(s))
    return hex.EncodeToString(h[:])
}

func BenchmarkUnbatchedCrypto(b *testing.B) {
    for b.Loop() {
        for _, line := range lines {
            hash(line)
        }
    }
}

func BenchmarkBatchedCrypto(b *testing.B) {
    batchSize := 100
    for b.Loop() {
        for i := 0; i < len(lines); i += batchSize {
            end := i + batchSize
            if end > len(lines) {
                end = len(lines)
            }
            joined := strings.Join(lines[i:end], "")
            hash(joined)
        }
    }
} 
```</details>

| Benchmark | 迭代次数 | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| BenchmarkUnbatchedProcessing-14 | 530 | 2,028,492 | 1,279,850 | 10,000 |
| BenchmarkBatchedProcessing-14 | 573 | 2,094,168 | 2,457,603 | 200 |

内存中的字符串操作显示了适度的性能差异。虽然批量变体将内存分配减少了50倍，但由于连接大型字符串的成本，执行时间仅略有减慢。这表明，批量并不总是能提高原始吞吐量，但它始终能减轻垃圾收集器的压力。

| Benchmark | 迭代次数 | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| BenchmarkUnbatchedIO-14 | 87 | 12,766,433 | 1,280,424 | 10,007 |
| BenchmarkBatchedIO-14 | 1324 | 993,912 | 2,458,026 | 207 |

文件I/O基准测试显示了最显著的增长。批量版本比非批量版本快12倍以上，系统调用次数少得多，执行时间也显著降低。将磁盘写入分组，分摊I/O成本，尽管暂时使用更多内存，但效率得到了大幅提升。

| Benchmark | 迭代次数 | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| BenchmarkUnbatchedCrypto-14 | 978 | 1,232,242 | 2,559,840 | 30,000 |
| BenchmarkBatchedCrypto-14 | 1760 | 675,303 | 2,470,406 | 400 |

密码学基准测试展示了在CPU密集型场景中批量处理的价值。批量哈希将总处理时间几乎减半，同时将分配计数减少了70多倍。这强化了批量作为一种有效策略，即使在CPU密集型工作负载中，减少操作数量也能带来更好的局部性和缓存行为。

## 当使用批量时

在以下情况下使用批量：

+   单个操作成本较高（例如，I/O、RPC、数据库写入）。将多个操作组合成一个批次可以减少重复调用的开销并提高效率。

+   系统从减少外部交互的频率中受益。更少的对外调用可以减轻下游系统的负担，并减少竞争或速率限制问题。

+   你对每个项目的延迟有一定的容忍度，以换取更高的吞吐量。批量处理会引入轻微的延迟，但可以显著提高整体系统吞吐量。

在以下情况下避免批量处理：

+   对于每个个体输入都需要立即采取行动。延迟处理以构建批量可能会违反对时间敏感的要求。

+   保留数据会引入风险（例如，在刷新前崩溃）。如果必须立即处理或持久化数据以避免丢失，批量处理可能是不安全的。

+   预测的延迟比吞吐量更重要。批量处理会增加时间上的变化性，这可能在对延迟有严格要求的系统中不可接受。
