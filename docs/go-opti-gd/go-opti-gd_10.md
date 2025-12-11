# Go 中的 Goroutine 工作池

> [原文：https://goperf.dev/01-common-patterns/worker-pool/](https://goperf.dev/01-common-patterns/worker-pool/)

Go 的并发模型使得启动成千上万的 goroutines 变得容易，但这种容易可能会带来代价。每个 goroutine 开始时很小，但在负载下，无界的并发可能会导致内存使用激增，上下文切换堆积，整体性能变得不可预测。

工作池通过限制活动 goroutines 的数量来帮助应用背压。而不是为每个任务启动一个，一个固定的池以受控的并行方式处理工作——保持内存使用可预测并避免过载。这使得即使在需求扩大的情况下，也能更容易地维持稳定的性能。

## 为什么工作池很重要

虽然为每个任务启动一个 goroutine 是惯例并且通常有效，但在规模上这样做会带来权衡。每个 goroutine 都需要堆栈空间并引入调度开销。当活动 goroutine 的数量增加时，性能可能会急剧下降，尤其是在处理无界输入的系统（如 HTTP 请求、队列中的作业或通道中的任务）时。

工作池维护一定数量的 goroutines，它们从共享的工作队列中提取任务。这创建了一个背压机制，确保系统不会同时处理超过其处理能力的更多工作。当每个任务的成本可预测，并且整个系统的吞吐量需要稳定时，工作池特别有价值。

## 基本工作池实现

这里是一个工作池的最小实现：

```go
`func  worker(id  int,  jobs  <-chan  int,  results  chan<-  [32]byte)  {   for  j  :=  range  jobs  {   results  <-  doWork(j)   } }  func  doWork(n  int)  [32]byte  {   data  :=  []byte(fmt.Sprintf("payload-%d",  n))   return  sha256.Sum256(data)  // (1) }  func  main()  {   jobs  :=  make(chan  int,  100)   results  :=  make(chan  [32]byte,  100)    for  w  :=  1;  w  <=  5;  w++  {   go  worker(w,  jobs,  results)   }    for  j  :=  1;  j  <=  10;  j++  {   jobs  <-  j   }   close(jobs)    for  a  :=  1;  a  <=  10;  a++  {   <-results   } }` 
```

1.  密码学用于说明 CPU 密集型代码

在这个例子中，五个工作者从`jobs`通道中提取任务，并将结果推送到`results`通道。工作池将并发限制为每次五项任务，无论发送了多少任务。

### 工作者数量和 CPU 核心

池中最佳的工作者数量与 CPU 核心数紧密相关，您可以在 Go 中使用`runtime.NumCPU()`或`runtime.GOMAXPROCS(0)`获取 CPU 核心数。对于每个工作者消耗大量 CPU 时间的 CPU 密集型任务——您通常希望工作者的数量等于或略少于逻辑 CPU 核心数。这确保了最大核心利用率而没有过多的开销。

如果你的任务是 I/O 密集型（例如，网络调用、磁盘 I/O、数据库查询），池的大小可以大于核心数。这是因为工作者将花费大量时间处于阻塞状态，允许其他人运行。相比之下，CPU 密集型工作负载受益于更小、紧密限定的池，这避免了竞争和上下文切换。

### 为什么过多的工作者会损害性能

添加更多的工作者似乎是一种提高吞吐量的简单方法，但超过某个点后，其好处会迅速减少。一旦超过系统的最佳并发水平，性能通常会下降而不是提高。

+   随着 Go 运行时需要处理比逻辑 CPU 更多的可运行 goroutine，调度器竞争增加。

+   上下文切换变得更加频繁，消耗 CPU 周期而不进行实际工作。

+   由于每个 goroutine 在空闲时也持有自己的栈，因此内存压力上升。

+   随着 goroutine 在核心之间弹跳，缓存冲突的可能性增加，这会破坏局部性并降低 CPU 缓存性能。

结果：更高的延迟，增加的垃圾收集活动，以及降低的吞吐量——这与正确调优的工作池应该提供的效果正好相反。

## 基准测试影响

在工作负载是 CPU 密集型或必须限制并发以避免耗尽共享资源（例如数据库连接或文件描述符）的场景中，工作池表现出色。比较无界 goroutine 启动与工作池的基准测试通常显示：

+   降低峰值内存使用

+   在负载下的更稳定响应时间

+   改进的 CPU 缓存局部性

<details class="example"><summary>显示基准文件</summary>

```go
`package  perf  import  (   // "log"   "fmt"   // "os"   "runtime"   "sync"   "testing"   "crypto/sha256" )  const  (   numJobs  =  10000   workerCount  =  10 )  func  doWork(n  int)  [32]byte  {   data  :=  []byte(fmt.Sprintf("payload-%d",  n))   return  sha256.Sum256(data) }  func  BenchmarkUnboundedGoroutines(b  *testing.B)  {   for  b.Loop()  {   var  wg  sync.WaitGroup   wg.Add(numJobs)    for  j  :=  0;  j  <  numJobs;  j++  {   go  func(job  int)  {   _  =  doWork(job)   wg.Done()   }(j)   }   wg.Wait()   } }  func  worker(jobs  <-chan  int,  wg  *sync.WaitGroup)  {   for  job  :=  range  jobs  {   _  =  doWork(job)   wg.Done()   } }  func  BenchmarkWorkerPool(b  *testing.B)  {   for  b.Loop()  {   var  wg  sync.WaitGroup   wg.Add(numJobs)    jobs  :=  make(chan  int,  numJobs)   for  w  :=  0;  w  <  workerCount;  w++  {   go  worker(jobs,  &wg)   }    for  j  :=  0;  j  <  numJobs;  j++  {   jobs  <-  j   }    close(jobs)   wg.Wait()   } }` 
```</details>

结果：

| Benchmark | Iterations | Time per op (ns) | Bytes per op | Allocs per op |
| --- | --- | --- | --- | --- |
| BenchmarkUnboundedGoroutines-14 | 2,274 | 2,499,213 ns | 639,350 | 39,754 |
| BenchmarkWorkerPool-14 | 3,325 | 1,791,772 ns | 320,707 | 19,762 |

在我们的基准测试中，每个任务执行了 CPU 密集型操作（例如，加密散列、数学或序列化）。在 Apple M3 Max 机器上，`workerCount = 10`时，工作池比无界 goroutine 模型显著优于，使用更少的资源并更快地完成工作。将工作计数增加到可用核心数量以上会导致由于竞争而性能变差。

## 何时使用工作池

在以下情况下使用 goroutine 工作池：

+   工作负载是无界的或高容量的。池可以防止 goroutine 无控制地增长，这可能导致内存耗尽、垃圾收集压力和不可预测的性能。

+   无界并发可能导致资源饱和。限制并发工作者的数量有助于避免 CPU、网络、数据库或磁盘 I/O（尤其是在负载下）过载。

+   您需要可预测的并行性以保持稳定性。限制并发可以平滑性能峰值并保持系统行为的一致性，即使在流量激增期间也是如此。

+   任务相对均匀且适合排队。当任务成本一致时，固定池大小可以提供高效的调度，最小化开销，确保良好的吞吐量而无需复杂的协调。

在以下情况下避免使用工作池：

+   每个任务都必须立即处理，以最小化延迟。在工作池中排队会引入延迟。对于延迟关键的任务，直接创建 goroutine 可以避免调度开销。

+   在低负载场景下，您可以依赖 Go 的调度器进行自然负载平衡。在轻负载中，管理池的开销可能超过了其带来的好处。Go 的调度器通常可以独立高效地处理轻量级并行。

+   工作负载量小且有限。对于有限且可预测的工作负载，直接启动 goroutines 可以使代码更简单，而不会冒着无序增长的风险。
