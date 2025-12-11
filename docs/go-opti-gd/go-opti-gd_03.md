# 对象池化

> 原文：[`goperf.dev/01-common-patterns/object-pooling/`](https://goperf.dev/01-common-patterns/object-pooling/)

通过重用对象而不是每次都分配新的对象，对象池化有助于减少高吞吐量 Go 程序中的分配 churn。这避免了分配器的重复工作，并减轻了垃圾收集器的压力，尤其是在处理短生命周期或频繁重用的结构时。

Go 的 `sync.Pool` 提供了一种使用最少代码实现池化的内置方式。它特别适用于分配成本高昂或可能导致频繁垃圾收集周期的对象。虽然不是万能的，但它是一个低摩擦的工具，可以在持续负载下带来延迟和 CPU 效率的明显提升。

## 对象池化是如何工作的

对象池化允许程序通过回收先前分配的对象而不是在每次使用时创建新对象来重用内存。而不是每次都击中堆，对象从共享池中检索，并在不再需要时返回。这减少了分配的数量，减少了垃圾收集的工作量，并导致更可预测的性能——特别是在具有高对象 churn 或严格延迟要求的工作负载中。

### 使用 `sync.Pool` 进行对象重用

#### 无对象池化（低效内存使用）

```go
`package  main  import  (   "fmt" )  type  Data  struct  {   Value  int }  func  createData()  *Data  {   return  &Data{Value:  42} }  func  main()  {   for  i  :=  0;  i  <  1000000;  i++  {   obj  :=  createData()  // Allocating a new object every time   _  =  obj  // Simulate usage   }   fmt.Println("Done") }` 
```

在上述示例中，每个迭代都会创建一个新的 `Data` 实例，导致不必要的分配和垃圾收集压力的增加。

#### 通过对象池化（优化内存使用）

```go
`package  main  import  (   "fmt"   "sync" )  type  Data  struct  {   Value  int }  var  dataPool  =  sync.Pool{   New:  func()  any  {   return  &Data{}   }, }  func  main()  {   for  i  :=  0;  i  <  1000000;  i++  {   obj  :=  dataPool.Get().(*Data)  // Retrieve from pool   obj.Value  =  42  // Use the object   dataPool.Put(obj)  // Return object to pool for reuse   }   fmt.Println("Done") }` 
```

### 为高效 I/O 池化字节缓冲区

当处理可能导致高分配和垃圾收集开销的大型字节切片时，对象池化特别有效。

```go
`package  main  import  (   "bytes"   "fmt"   "sync" )  var  bufferPool  =  sync.Pool{   New:  func()  any  {   return  new(bytes.Buffer)   }, }  func  main()  {   buf  :=  bufferPool.Get().(*bytes.Buffer)   buf.Reset()   buf.WriteString("Hello, pooled world!")   fmt.Println(buf.String())   bufferPool.Put(buf)  // Return buffer to pool for reuse }` 
```

使用 `sync.Pool` 对字节缓冲区进行池化，在处理高频 I/O 操作时显著减少了内存压力。

## 基准测试影响

为了证明对象池实际上减少了分配并提高了速度，我们可以使用 Go 的内置内存分析工具（`pprof`）并比较非池化和池化版本之间的内存分配。模拟一个全面使用内存进行基准测试的应用程序具有挑战性，因此我们需要一个受控的测试来评估直接堆分配与池化分配之间的差异。

<details class="example"><summary>显示基准文件</summary>

```go
`package  perf  import  (   "sync"   "testing" )  // Data is a struct with a large fixed-size array to simulate a memory-intensive object. type  Data  struct  {   Values  [1024]int }  // BenchmarkWithoutPooling measures the performance of direct heap allocations. func  BenchmarkWithoutPooling(b  *testing.B)  {   for  b.Loop()  {   data  :=  &Data{}  // Allocating a new object each time   data.Values[0]  =  42  // Simulating some memory activity   } }  // dataPool is a sync.Pool that reuses instances of Data to reduce memory allocations. var  dataPool  =  sync.Pool{   New:  func()  any  {   return  &Data{}   }, }  // BenchmarkWithPooling measures the performance of using sync.Pool to reuse objects. func  BenchmarkWithPooling(b  *testing.B)  {   for  b.Loop()  {   obj  :=  dataPool.Get().(*Data)  // Retrieve from pool   obj.Values[0]  =  42  // Simulate memory usage   dataPool.Put(obj)  // Return object to pool for reuse   } }` 
```</details>

| 基准 | 迭代次数 | 每次操作时间（ns） | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| 无池化基准-14 | 1,692,014 | 705.4 | 8,192 | 1 |
| 池化基准-14 | 160,440,506 | 7.455 | 0 | 0 |

基准测试结果突出了直接分配和对象池化在性能和内存使用上的对比。在 `BenchmarkWithoutPooling` 中，每次迭代都会在堆上创建一个新的对象，导致执行时间更长，内存消耗增加。这种持续的分配压力会触发更频繁的垃圾回收，增加延迟并降低吞吐量。每个操作的非零分配计数确认了每次迭代都会对 GC 负载做出贡献，这使得这种方法在高吞吐量场景中效率较低。

## 你应该在何时使用 `sync.Pool`？

在以下情况下使用 sync.Pool：

+   你有短暂的生命周期且可重用的对象（例如，缓冲区、临时内存、请求状态）。池化避免了重复分配，并允许你高效地回收内存。

+   分配开销或 GC（垃圾回收）的波动是可测量且显著的。重用对象减少了堆分配的数量，从而降低了垃圾回收的频率和暂停时间。

+   对象的生命周期是局部的，可以在使用之间重置。当对象不需要复杂的拆解，并且在简单重置后可以安全重用时，池化简单且有效。

+   你想在高吞吐量系统中减少对垃圾回收器的压力。在每秒处理数千个请求的系统中，池化有助于保持一致的性能并最小化 GC 相关的延迟峰值。

避免使用 sync.Pool：

+   对象是长期存在或跨多个 goroutine 共享的。`sync.Pool`针对短暂的单次使用对象进行了优化，不管理共享所有权或协调。

+   重用率低且池化对象不常被访问。如果对象在池中闲置，你获得的收益很少，甚至可能浪费内存。

+   可预测性或生命周期控制比分配速度更重要。池化使得生命周期跟踪更加困难，可能不值得这种权衡。

+   内存节省微不足道或代码复杂性显著增加。如果池化没有提供明显的优势，它可能会在不复杂的代码中增加不必要的复杂性。
