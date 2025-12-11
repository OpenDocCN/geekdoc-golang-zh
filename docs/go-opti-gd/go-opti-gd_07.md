# 零拷贝技术

> 原文：[`goperf.dev/01-common-patterns/zero-copy/`](https://goperf.dev/01-common-patterns/zero-copy/)

当编写性能关键型的 Go 代码时，内存管理的影响往往比最初看起来更大。零拷贝技术是加强这种控制的有效方法之一。这些技术不是在缓冲区之间移动字节，而是直接在现有内存上工作——完全避免复制。这意味着对 CPU 的压力更小，缓存行为更好，以及更少的 GC 触发的暂停。对于 I/O 密集型系统——无论是流式传输文件、处理网络流量还是解析大型数据集——这可以转化为更高的吞吐量和更低的延迟，而不会增加复杂性。

## 理解零拷贝

在通常的 I/O 路径中，数据在用户空间和内核空间之间来回移动——首先复制到一个内核缓冲区，然后到你的应用程序的缓冲区，或者反过来。它有效，但效率低下。每次复制都会消耗 CPU 周期并阻塞内存带宽。零拷贝改变了这一点。它不是在缓冲区之间弹跳数据，而是让应用程序直接与现有的内容工作——没有绕路，没有额外的复制。结果是？更低的 CPU 负载，更好的内存使用，以及更快的 I/O，尤其是在吞吐量或延迟真正重要的时候。

## Go 中的常见零拷贝技术

### 使用`io.Reader`和`io.Writer`接口

使用像`io.Reader`和`io.Writer`这样的接口可以让你对数据流动有更精细的控制。不必每次都启动新的缓冲区，你可以重用现有的缓冲区并保持内存使用稳定。在实践中，这可以避免不必要的垃圾回收压力，并保持你的 I/O 路径清洁高效——尤其是在处理高吞吐量或流式工作负载时。

```go
`func  StreamData(src  io.Reader,  dst  io.Writer)  error  {   buf  :=  make([]byte,  4096)  // Reusable buffer   _,  err  :=  io.CopyBuffer(dst,  src,  buf)   return  err }` 
```

`io.CopyBuffer`重用提供的缓冲区，避免重复分配和中间复制。关于`io.CopyBuffer`的深入解释[可在 SO 上找到](https://stackoverflow.com/questions/71082021/what-exactly-is-buffer-last-parameter-in-io-copybuffer)。

### 高效数据访问的切片

将大型字节数组或缓冲区切片而不是将数据复制到新的切片中是一种强大的零拷贝策略：

```go
`func  process(buffer  []byte)  []byte  {   return  buffer[128:256]  // returns a slice reference without copying }` 
```

Go 中的切片天生就是零拷贝，因为它们引用了底层的数组。

### 内存映射（`mmap`）

使用内存映射可以在不进行显式读取操作的情况下直接访问文件内容：

```go
`import  "golang.org/x/exp/mmap"  func  ReadFileZeroCopy(path  string)  ([]byte,  error)  {   r,  err  :=  mmap.Open(path)   if  err  !=  nil  {   return  nil,  err   }   defer  r.Close()    data  :=  make([]byte,  r.Len())   _,  err  =  r.ReadAt(data,  0)   return  data,  err }` 
```

这种方法直接将文件内容映射到内存中，完全消除了内核空间和用户空间之间的复制。

## 基准测试影响

下面是一个基本的基准测试，展示了显式复制和零拷贝切片之间的性能差异：

```go
`func  BenchmarkCopy(b  *testing.B)  {   data  :=  make([]byte,  64*1024)   for  b.Loop()  {   buf  :=  make([]byte,  len(data))   copy(buf,  data)   } }  func  BenchmarkSlice(b  *testing.B)  {   data  :=  make([]byte,  64*1024)   for  b.Loop()  {   _  =  data[:]   } }` 
```

在 `BenchmarkCopy` 中，每次迭代将 64KB 缓冲区拷贝到一个新的切片中——每次都分配内存和复制数据。这种成本很快就会累积。另一方面，`BenchmarkSlice` 只是对相同的缓冲区进行切片——没有分配，没有拷贝，只是对相同数据的新的视图。这种差异是巨大的。当性能很重要时，避免拷贝不仅仅是一个微优化——这是基础。

Info

这两个函数在行为上并不等价——`BenchmarkCopy` 对缓冲区进行实际的深度拷贝，而 `BenchmarkSlice` 只创建一个新的切片头，指向相同的基本数据。这个基准测试并不是比较功能正确性，而是有意对比性能特性，以突出不必要的拷贝成本。

| Benchmark | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- |
| BenchmarkCopy | 4,246 | 65536 | 1 |
| BenchmarkSlice | 0.592 | 0 | 0 |

### 文件 I/O：内存映射与标准读取

我们还使用 `os.ReadAt` 与 `mmap.Open` 对一个 4MB 的二进制文件进行了文件读取性能的基准测试。

```go
`func  BenchmarkReadWithCopy(b  *testing.B)  {   f,  err  :=  os.Open("testdata/largefile.bin")   if  err  !=  nil  {   b.Fatalf("failed to open file: %v",  err)   }   defer  f.Close()    buf  :=  make([]byte,  4*1024*1024)  // 4MB buffer   for  b.Loop()  {   _,  err  :=  f.ReadAt(buf,  0)   if  err  !=  nil  &&  err  !=  io.EOF  {   b.Fatal(err)   }   } }  func  BenchmarkReadWithMmap(b  *testing.B)  {   r,  err  :=  mmap.Open("testdata/largefile.bin")   if  err  !=  nil  {   b.Fatalf("failed to mmap file: %v",  err)   }   defer  r.Close()    buf  :=  make([]byte,  r.Len())   for  b.Loop()  {   _,  err  :=  r.ReadAt(buf,  0)   if  err  !=  nil  &&  err  !=  io.EOF  {   b.Fatal(err)   }   } }` 
```

<details class="info"><summary>如何运行基准测试</summary>

要运行涉及 `mmap` 的基准测试，你需要安装所需的包并创建一个测试文件：

```go
`go  get  golang.org/x/exp/mmap mkdir  -p  testdata dd  if=/dev/urandom  of=./testdata/largefile.bin  bs=1M  count=4` 
```</details>

基准测试结果

| Benchmark | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- |
| ReadWithCopy | 94,650 | 0 | 0 |
| ReadWithMmap | 50,082 | 0 | 0 |

内存映射版本（`mmap`）比标准读取调用快近 2 倍。这说明了通过内存映射实现的零拷贝访问如何显著降低大文件的读取延迟和 CPU 使用率。

<details class="example"><summary>显示完整的基准测试文件</summary>

```go
`package  perf  import  "testing"   // interface-start  type  Worker  interface  {   Work() }  type  LargeJob  struct  {   payload  [4096]byte }  func  (LargeJob)  Work()  {} // interface-end  // bench-slice-start func  BenchmarkBoxedLargeSlice(b  *testing.B)  {   jobs  :=  make([]Worker,  0,  1000)   for  b.Loop()  {   jobs  =  jobs[:0]   for  j  :=  0;  j  <  1000;  j++  {   var  job  LargeJob   jobs  =  append(jobs,  job)   }   } }  func  BenchmarkPointerLargeSlice(b  *testing.B)  {   jobs  :=  make([]Worker,  0,  1000)   for  b.Loop()  {   jobs  :=  jobs[:0]   for  j  :=  0;  j  <  1000;  j++  {   job  :=  &LargeJob{}   jobs  =  append(jobs,  job)   }   } } // bench-slice-end  // bench-call-start var  sink  Worker  func  call(w  Worker)  {   sink  =  w }  func  BenchmarkCallWithValue(b  *testing.B)  {   for  b.Loop()  {   var  j  LargeJob   call(j)   } }  func  BenchmarkCallWithPointer(b  *testing.B)  {   for  b.Loop()  {   j  :=  &LargeJob{}   call(j)   } } // bench-call-end` 
```</details>

## 何时使用零拷贝

零拷贝技术在以下方面高度有益：

+   网络服务器处理大量并发数据流。避免不必要的内存拷贝有助于降低 CPU 使用率和延迟，尤其是在高负载下。

+   拥有大量 I/O 操作的应用程序，如文件流或实时数据处理。零拷贝允许数据在系统中高效地移动，无需冗余分配或拷贝。

Warning

零拷贝并非免费的胜利。切片共享基本内存，因此重用它们意味着你也在共享状态。如果你的代码的一部分在另一部分仍在读取数据时更改数据，你将为自己设置微妙且难以追踪的 bug。这种共享内存需要纪律——清晰的拥有权和严格控制。它还增加了复杂性，除非性能提升是真实且可衡量的，否则可能不值得。在做出承诺之前，始终进行基准测试。

### 实际应用案例和库

零拷贝策略不仅停留在理论层面——它们被性能关键型的 Go 系统在生产环境中使用：

+   [fasthttp](https://github.com/valyala/fasthttp)：一个设计用来避免分配的高性能 HTTP 服务器。它直接返回切片，并避免`string`转换以最小化拷贝。

+   [gRPC-Go](https://github.com/grpc/grpc-go)：使用内部缓冲池，避免对大型请求/响应消息进行深度复制，以减少 GC 压力。

+   [MinIO](https://github.com/minio/minio): 一个对象存储系统，它使用 `io.Reader` 直接在磁盘和网络之间传输数据，无需不必要的缓冲区复制。

+   [Protobuf](https://github.com/protocolbuffers/protobuf) 和 [MsgPack](https://github.com/vmihailenco/msgpack) 库：高效的序列化框架，如 `google.golang.org/protobuf` 和 `vmihailenco/msgpack`，支持直接解码到用户管理的缓冲区。

+   [InfluxDB](https://github.com/influxdata/influxdb) 和 [Badger](https://github.com/hypermodeinc/badger)：这些存储引擎广泛使用 `mmap` 以实现快速、零拷贝访问数据库文件。

这些库展示了零拷贝技术如何帮助减少分配、GC 开销和系统调用频率，同时提高吞吐量。
