# 零拷贝技术

> 原文：[`goperf.dev/01-common-patterns/zero-copy/`](https://goperf.dev/01-common-patterns/zero-copy/)

在编写性能关键型的 Go 代码时，内存管理的影响往往比最初看起来更大。零拷贝技术是加强这种控制的有效方法之一。这些技术不是在缓冲区之间移动字节，而是直接在现有内存上工作——完全避免拷贝。这意味着对 CPU 的压力更小，缓存行为更好，以及更少的由 GC 触发的暂停。对于 I/O 密集型系统——无论是流式传输文件、处理网络流量还是解析大型数据集——这可以转化为更高的吞吐量和更低的延迟，而不会增加复杂性。

## 理解零拷贝

在常规 I/O 路径中，数据在用户空间和内核空间之间来回移动——首先复制到内核缓冲区，然后到你的应用程序的缓冲区，或者反过来。这虽然可行，但很浪费。每次拷贝都会消耗 CPU 周期并阻塞内存带宽。零拷贝技术改变了这一点。它不是在缓冲区之间弹跳数据，而是让应用程序直接与现有的内容工作——没有绕路，没有额外的拷贝。结果？更低的 CPU 负载，更好的内存使用，以及更快的 I/O，尤其是在吞吐量或延迟真正重要的时候。

## Go 中的常见零拷贝技术

### 使用 `io.Reader` 和 `io.Writer` 接口

使用 `io.Reader` 和 `io.Writer` 等接口可以让你对数据流有更精细的控制。不必每次都启动新的缓冲区，你可以重用现有的缓冲区，并保持内存使用稳定。在实践中，这避免了不必要的垃圾收集压力，并保持了你的 I/O 路径清洁高效——尤其是在处理高吞吐量或流式工作负载时。

```go
func StreamData(src io.Reader, dst io.Writer) error {
    buf := make([]byte, 4096) // Reusable buffer
    _, err := io.CopyBuffer(dst, src, buf)
    return err
} 
```

`io.CopyBuffer` 重新使用提供的缓冲区，避免了重复分配和中间拷贝。关于 `io.CopyBuffer` 的深入解释可以在 SO 上找到：[什么是 io.CopyBuffer 中的 buffer 最后一个参数？](https://stackoverflow.com/questions/71082021/what-exactly-is-buffer-last-parameter-in-io-copybuffer)。

### 为高效数据访问进行切片

与其将数据复制到新的切片中，不如切片大型字节数组或缓冲区是一种强大的零拷贝策略：

```go
func process(buffer []byte) []byte {
    return buffer[128:256] // returns a slice reference without copying
} 
```

Go 中的切片天生就是零拷贝的，因为它们引用了底层的数组。

### 内存映射 (`mmap`)

使用内存映射可以直接访问文件内容，而无需显式读取操作：

```go
import "golang.org/x/exp/mmap"

func ReadFileZeroCopy(path string) ([]byte, error) {
    r, err := mmap.Open(path)
    if err != nil {
        return nil, err
    }
    defer r.Close()

    data := make([]byte, r.Len())
    _, err = r.ReadAt(data, 0)
    return data, err
} 
```

这种方法直接将文件内容映射到内存中，完全消除了内核和用户空间之间的拷贝。

## 基准测试影响

这里有一个基本的基准测试，说明了显式拷贝和零拷贝切片之间的性能差异：

```go
func BenchmarkCopy(b *testing.B) {
    data := make([]byte, 64*1024)
    for b.Loop() {
        buf := make([]byte, len(data))
        copy(buf, data)
    }
}

func BenchmarkSlice(b *testing.B) {
    data := make([]byte, 64*1024)
    for b.Loop() {
        _ = data[:]
    }
} 
```

在 `BenchmarkCopy` 中，每次迭代都会将 64KB 缓冲区复制到一个新的切片中——每次都会分配内存和复制数据。这种成本很快就会累积起来。另一方面，`BenchmarkSlice` 只是对相同的缓冲区进行重新切片——没有分配，没有复制，只是对相同数据的新的视图。这种差异是巨大的。当性能至关重要时，避免复制不仅仅是一个微优化——这是基础。

信息

这两个函数在行为上并不相同——`BenchmarkCopy` 对缓冲区进行了实际的深度复制，而 `BenchmarkSlice` 只创建了一个指向相同底层数据的新切片头。这个基准测试并不是比较功能正确性，而是有意对比性能特性，以突出不必要的复制的成本。

| 基准测试 | 每次操作时间 (ns) | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- |
| BenchmarkCopy | 4,246 | 65536 | 1 |
| BenchmarkSlice | 0.592 | 0 | 0 |

### 文件 I/O：内存映射与标准读取

我们还使用 `os.ReadAt` 与 `mmap.Open` 对 4MB 二进制文件的文件读取性能进行了基准测试。

```go
func BenchmarkReadWithCopy(b *testing.B) {
    f, err := os.Open("testdata/largefile.bin")
    if err != nil {
        b.Fatalf("failed to open file: %v", err)
    }
    defer f.Close()

    buf := make([]byte, 4*1024*1024) // 4MB buffer
    for b.Loop() {
        _, err := f.ReadAt(buf, 0)
        if err != nil && err != io.EOF {
            b.Fatal(err)
        }
    }
}

func BenchmarkReadWithMmap(b *testing.B) {
    r, err := mmap.Open("testdata/largefile.bin")
    if err != nil {
        b.Fatalf("failed to mmap file: %v", err)
    }
    defer r.Close()

    buf := make([]byte, r.Len())
    for b.Loop() {
        _, err := r.ReadAt(buf, 0)
        if err != nil && err != io.EOF {
            b.Fatal(err)
        }
    }
} 
```

<details class="info"><summary>如何运行基准测试</summary>

要运行涉及 `mmap` 的基准测试，您需要安装所需的软件包并创建一个测试文件：

```go
go get golang.org/x/exp/mmap
mkdir -p testdata
dd if=/dev/urandom of=./testdata/largefile.bin bs=1M count=4 
```</details>

基准测试结果

| 基准测试 | 每次操作时间 (ns) | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- |
| ReadWithCopy | 94,650 | 0 | 0 |
| ReadWithMmap | 50,082 | 0 | 0 |

内存映射版本（`mmap`）比标准读取调用快近 2 倍。这说明了通过内存映射实现的零拷贝访问如何显著减少大文件的读取延迟和 CPU 使用率。

<details class="example"><summary>显示完整的基准测试文件</summary>

```go
package perf

import "testing"

// interface-start

type Worker interface {
    Work()
}

type LargeJob struct {
    payload [4096]byte
}

func (LargeJob) Work() {}
// interface-end

// bench-slice-start
func BenchmarkBoxedLargeSlice(b *testing.B) {
    jobs := make([]Worker, 0, 1000)
    for b.Loop() {
        jobs = jobs[:0]
        for j := 0; j < 1000; j++ {
            var job LargeJob
            jobs = append(jobs, job)
        }
    }
}

func BenchmarkPointerLargeSlice(b *testing.B) {
    jobs := make([]Worker, 0, 1000)
    for b.Loop() {
        jobs := jobs[:0]
        for j := 0; j < 1000; j++ {
            job := &LargeJob{}
            jobs = append(jobs, job)
        }
    }
}
// bench-slice-end

// bench-call-start
var sink Worker

func call(w Worker) {
    sink = w
}

func BenchmarkCallWithValue(b *testing.B) {
    for b.Loop() {
        var j LargeJob
        call(j)
    }
}

func BenchmarkCallWithPointer(b *testing.B) {
    for b.Loop() {
        j := &LargeJob{}
        call(j)
    }
}
// bench-call-end 
```</details>

## 何时使用零拷贝

零拷贝技术对于以下方面高度有益：

+   处理大量并发数据流的网络服务器。避免不必要的内存复制有助于减少 CPU 使用率和延迟，尤其是在高负载下。

+   具有大量 I/O 操作的应用程序，如文件流或实时数据处理。零拷贝允许数据在系统中高效地移动，无需冗余分配或复制。

警告

零拷贝并非免费的胜利。切片共享底层内存，因此重用它们意味着您也在共享状态。如果代码的一部分在另一部分仍在读取数据时更改了数据，您就会为自己设置微妙且难以追踪的 bug。这种共享内存需要纪律——清晰的拥有权和严格控制。它还增加了复杂性，除非性能提升是真实且可衡量的，否则可能不值得。始终在做出决定之前进行基准测试。

### 实际应用案例和库

零拷贝策略不仅仅是理论上的——它们被性能关键型 Go 系统在生产中实际使用：

+   [fasthttp](https://github.com/valyala/fasthttp)：一个高性能的 HTTP 服务器，旨在避免分配。它直接返回切片，并避免 `string` 转换来最小化复制。

+   [gRPC-Go](https://github.com/grpc/grpc-go)：使用内部缓冲池，避免对大型请求/响应消息进行深度复制，以减少垃圾回收压力。

+   [MinIO](https://github.com/minio/minio)：一个对象存储系统，它使用 `io.Reader` 在磁盘和网络之间直接传输数据，而不进行不必要的缓冲区复制。

+   [Protobuf](https://github.com/protocolbuffers/protobuf) 和 [MsgPack](https://github.com/vmihailenco/msgpack) 库：高效的序列化框架，如 `google.golang.org/protobuf` 和 `vmihailenco/msgpack`，支持直接解码到用户管理的缓冲区。

+   [InfluxDB](https://github.com/influxdata/influxdb) 和 [Badger](https://github.com/hypermodeinc/badger)：这些存储引擎广泛使用 `mmap` 以实现快速、零拷贝访问数据库文件。

这些库展示了零拷贝技术如何帮助减少分配、垃圾回收开销和系统调用频率，同时提高吞吐量。
