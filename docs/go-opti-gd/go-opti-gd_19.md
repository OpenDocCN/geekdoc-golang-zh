# 网络化 Go 应用程序的基准测试和负载测试

> 原文：[`goperf.dev/02-networking/bench-and-load/`](https://goperf.dev/02-networking/bench-and-load/)

在你伸手去一个无锁队列或调整你的 goroutine 池之前，退一步。没有基线的优化只是猜测。在 Go 应用程序中，性能调整始于了解你的系统在压力下的行为，这意味着在负载下对其进行基准测试。

负载测试不仅仅是推动请求直到事物崩溃。它是模拟现实的使用模式以提取可测量、可重复的数据。这些数据是后续每个优化的基础。

## 测试应用：模拟快速/慢路径和 GC 压力

为了进行有意义的基准测试，我们需要反映不同工作负载特性的端点。

<details class="example"><summary>显示基准测试应用</summary>

```go
`package  main  // pprof-start import  ( // pprof-end   "flag"   "fmt"   "log"   "math/rand/v2"   "net/http" // pprof-start   _  "net/http/pprof" // pprof-end   "os"   "os/signal"   "time" // pprof-start ) // pprof-end  var  (   fastDelay  =  flag.Duration("fast-delay",  0,  "Fixed delay for fast handler (if any)")   slowMin  =  flag.Duration("slow-min",  1*time.Millisecond,  "Minimum delay for slow handler")   slowMax  =  flag.Duration("slow-max",  300*time.Millisecond,  "Maximum delay for slow handler")   gcMinAlloc  =  flag.Int("gc-min-alloc",  50,  "Minimum number of allocations in GC heavy handler")   gcMaxAlloc  =  flag.Int("gc-max-alloc",  1000,  "Maximum number of allocations in GC heavy handler") )  func  randRange(min,  max  int)  int  {   return  rand.IntN(max-min)  +  min }  func  fastHandler(w  http.ResponseWriter,  r  *http.Request)  {   if  *fastDelay  >  0  {   time.Sleep(*fastDelay)   }   fmt.Fprintln(w,  "fast response") }  func  slowHandler(w  http.ResponseWriter,  r  *http.Request)  {   delayRange  :=  int((*slowMax  -  *slowMin)  /  time.Millisecond)   delay  :=  time.Duration(randRange(1,  delayRange))  *  time.Millisecond   time.Sleep(delay)   fmt.Fprintf(w,  "slow response with delay %d ms\n",  delay.Milliseconds()) }  // heavy-start var  longLivedData  [][]byte  func  gcHeavyHandler(w  http.ResponseWriter,  r  *http.Request)  {   numAllocs  :=  randRange(*gcMinAlloc,  *gcMaxAlloc)   var  data  [][]byte   for  i  :=  0;  i  <  numAllocs;  i++  {   // Allocate 10KB slices. Occasionally retain a reference to simulate long-lived objects.   b  :=  make([]byte,  1024*10)   data  =  append(data,  b)   if  i%100  ==  0  {  // every 100 allocations, keep the data alive   longLivedData  =  append(longLivedData,  b)   }   }   fmt.Fprintf(w,  "allocated %d KB\n",  len(data)*10) } // heavy-end  func  main()  {   flag.Parse()    http.HandleFunc("/fast",  fastHandler)   http.HandleFunc("/slow",  slowHandler)   http.HandleFunc("/gc",  gcHeavyHandler)  // pprof-start // ...    // Start pprof in a separate goroutine.   go  func()  {   log.Println("pprof listening on :6060")   if  err  :=  http.ListenAndServe("localhost:6060",  nil);  err  !=  nil  {   log.Fatalf("pprof server error: %v",  err)   }   }() // pprof-end    // Create a server to allow for graceful shutdown.   server  :=  &http.Server{Addr:  ":8080"}    go  func()  {   log.Println("HTTP server listening on :8080")   if  err  :=  server.ListenAndServe();  err  !=  nil  &&  err  !=  http.ErrServerClosed  {   log.Fatalf("HTTP server error: %v",  err)   }   }()    // Graceful shutdown on interrupt signal.   sigCh  :=  make(chan  os.Signal,  1)   signal.Notify(sigCh,  os.Interrupt)   <-sigCh   log.Println("Shutting down server...")   if  err  :=  server.Shutdown(nil);  err  !=  nil  {   log.Fatalf("Server Shutdown Failed:%+v",  err)   }   log.Println("Server exited") }` 
```</details>

+   `/fast`：快速响应，非常适合吞吐量测试。

+   `/slow`：模拟延迟和竞争。

+   `/gc`：模拟 GC 重量级工作流程。

+   `net/http/pprof`：在 `localhost:6060` 上公开运行时分析。

运行方式：

```go
`go  run  main.go` 
```

## 模拟负载：反映现实情况的工具

### 何时使用什么

信息

这绝对不是一份详尽的列表。负载测试工具的生态系统很广泛，并且不断演变。像 Apache JMeter、Locust、Artillery 和 Gatling 这样的工具各自带来了自己的优势——从基于 UI 的测试设计到分布式执行或 JVM 基于的场景。正确的选择取决于你的堆栈、测试目标和团队工作流程。这里列出的工具针对基于 Go 的服务和本地优先基准测试进行了优化，但它们只是起点。

一眼望去，`vegeta`、`wrk` 和 `k6` 都在打击 HTTP 端点。但它们根据你测试的内容、所需的精度以及场景的复杂程度扮演着不同的角色。

| 工具 | 侧重 | 可脚本化 | 指标深度 | 理想用例 |
| --- | --- | --- | --- | --- |
| `vegeta` | 恒定速率负载生成 | 否（但可组合） | 高（直方图、百分位数） | 跟踪随时间变化的延迟百分位数；CI 基准测试 |
| `wrk` | 最大吞吐量压力测试 | 是（Lua） | 中等 | 测量原始服务器容量和并发限制 |
| `k6` | 基于场景的模拟 | 是（JavaScript） | 高（VU 指标、仪表板） | 模拟现实世界用户工作流程和节奏 |

使用 `vegeta` 的情况：

+   你需要一个一致的 RPS 负载（例如，60 秒内每秒 100 个请求）。

+   你在可控的压力下观察延迟下降。

+   你想要结构化的输出（直方图、百分位数）以进行性能分析。

+   你想在更深入的剖析之前验证本地更改。

使用 `wrk` 的情况：

+   你正在探索上限吞吐量。

+   你想要原始、快速的负载，设置最少。

+   你在高并发（例如，10k 个连接）下进行剖析。

使用 `k6` 的情况：

+   你必须模拟复杂的流程，如登录 → API 调用 → 等待 → 注销。

+   你正在将性能测试集成到 CI/CD 中。

+   你想要阈值、节奏和视觉反馈。

这些工具中的每一个都在您的基准测试工具包中都有其位置。选择正确的工具取决于您是在验证性能、探索扩展阈值还是模拟最终用户行为。

### Vegeta

[Vegeta](https://github.com/tsenart/vegeta) 是一个用 Go 编写的灵活的 HTTP 负载测试工具，用于生成恒定的请求速率。这使得它非常适合模拟稳定的、持续的流量模式，而不是突然的峰值。

当需要精确度时，我们会选择 Vegeta。它保持精确的请求速率并捕获详细的延迟分布，这有助于跟踪系统在负载下的行为变化。它轻量级，易于自动化，并且可以干净地集成到 CI 工作流程中——使其成为基准测试 Go 服务的可靠选择。

安装：

```go
`go  install  github.com/tsenart/vegeta@latest` 
```

我们将要测试哪个端点：

```go
`echo  "GET http://localhost:8080/slow"  >  targets.txt` 
```

运行：

```go
`vegeta  attack  -rate=100  -duration=30s  -targets=targets.txt  |  tee  results.bin  |  vegeta  report` 
```

<details class="example"><summary>潜在输出</summary>

```go
`>  vegeta  attack  -rate=100  -duration=30s  -targets=targets.txt  |  tee  results.bin  |  vegeta  report Requests  [total,  rate,  throughput]  3000,  100.04,  100.03 Duration  [total,  attack,  wait]  29.989635542s,  29.989108333s,  527.209µs Latencies  [mean,  50,  95,  99,  max]  524.563µs,  504.802µs,  793.997µs,  1.47362ms,  7.351541ms Bytes  In  [total,  mean]  42000,  14.00 Bytes  Out  [total,  mean]  0,  0.00 Success  [ratio]  100.00% Status  Codes  [code:count]  200:3000 Error  Set:` 
```</details>

查看百分位数：

```go
`vegeta  report  -type='hist[0,10ms,50ms,100ms,200ms,500ms,1s]'  <  results.bin` 
```

生成图表：

```go
`vegeta  plot  <  results.bin  >  plot.html` 
```

<details class="info"><summary>使用 Vegeta 测试多个端点</summary>

根据您的目标，有两种推荐的测试方法，可以在单次运行中测试 `/fast` 和 `/slow` 端点。

**选项 1：端点间的轮询**

创建一个包含两个端点的 `targets.txt` 文件：

```go
`cat  >  targets.txt  <<EOF GET http://localhost:8080/fast GET http://localhost:8080/slow EOF` 
```

运行测试：

```go
`vegeta  attack  -rate=50  -duration=30s  -targets=targets.txt  |  tee  mixed-results.bin  |  vegeta  report  -type='hist[0,50ms,100ms,200ms,500ms,1s,2s]'` 
```

+   请求在两个端点之间随机分布。

+   有助于观察混合流量的总体行为。

+   易于设置和分析组合性能。

**选项 2：使用多个 Vegeta 运行进行加权混合**

要模拟不同的流量比例（例如，80%快速，20%慢速）：

```go
`# Send 80% of requests to /fast vegeta  attack  -rate=40  -duration=30s  -targets=<(echo  "GET http://localhost:8080/fast")  >  fast.bin  &  # Send 20% of requests to /slow vegeta  attack  -rate=10  -duration=30s  -targets=<(echo  "GET http://localhost:8080/slow")  >  slow.bin  &  wait` 
```

然后将结果合并并生成报告：

```go
`vegeta  encode  fast.bin  slow.bin  >  combined.bin vegeta  report  -type='hist[0,50ms,100ms,200ms,500ms,1s,2s]'  <  combined.bin` 
```

+   提供对流量分布的精确控制。

+   更适合模拟真实的流量混合。

+   当单独分析时，允许每个端点的基准测试。

这两种方法都是有效的——根据您是否需要简单性或控制来选择。</details>

### wrk

[wrk](https://github.com/wg/wrk) 是一个用 C 编写的性能高效的 HTTP 基准测试工具。它旨在提供原始速度和并发性，使其非常适合压力测试服务器的吞吐量和连接处理能力。

当我们想要将系统推到其极限时，我们会使用 `wrk`。它擅长使用多个线程和连接向端点发送高请求量。虽然它不像 `vegeta` 那样提供详细的百分位数，但它非常适合快速饱和测试和测量 Go 服务器在开始丢弃请求或停滞之前可以处理多少流量。

安装：

```go
`brew  install  wrk  # or build from source` 
```

运行测试：

```go
`wrk  -t4  -c100  -d30s  http://localhost:8080/fast` 
```

<details class="example"><summary>潜在输出</summary>

```go
`>  wrk  -t4  -c100  -d30s  http://localhost:8080/fast Running  30s  test  @  http://localhost:8080/fast   4  threads  and  100  connections   Thread  Stats  Avg  Stdev  Max  +/-  Stdev   Latency  1.29ms  255.31us  5.24ms  84.86%   Req/Sec  19.30k  565.16  21.93k  77.92%   2304779  requests  in  30.00s,  287.94MB  read Requests/sec:  76823.88 Transfer/sec:  9.60MB` 
```</details>

### k6

[k6](https://k6.io) 是一个围绕在 JavaScript 中编写现实客户行为的现代负载测试工具。它旨在模拟基于时间的负载配置文件——上升、稳态、下降——并支持自定义流程、节奏和基于阈值的验证。

当原始吞吐量不足且需要模拟真实用户与系统交互时，我们使用 `k6`。它处理链式请求，模拟会话式流程，并支持开箱即用的基于阶段的测试。凭借丰富的指标和无缝的 CI/CD 集成，`k6` 有助于在生产之前揭示回归。

安装：

```go
`brew  install  k6` 
```

脚本：

```go
`// script.js import  http  from  'k6/http'; import  {  sleep  }  from  'k6';  export  const  options  =  {   stages:  [   {  duration:  '10s',  target:  50  },   {  duration:  '30s',  target:  50  },   {  duration:  '10s',  target:  0  },   ], };  export  default  function  ()  {   http.get('http://localhost:8080/fast');   sleep(1); }` 
```

运行：

```go
`k6  run  script.js` 
```

<details class="example"><summary>潜在输出</summary>

```go
`>  k6  run  script.js    /\  Grafana  /‾‾/   /\  /  \  |\  __  /  /   /  \/  \  |  |/  /  /  ‾‾\   /  \  |  (  |  (‾)  |   /  __________  \  |_|\_\  \_____/    execution:  local   script:  script.js   output:  -    scenarios:  (100.00%)  1  scenario,  50  max  VUs,  1m20s  max  duration  (incl.  graceful  stop):   *  default:  Up  to  50  looping  VUs  for  50s  over  3  stages  (gracefulRampDown:  30s,  gracefulStop:  30s)     █  TOTAL  RESULTS    HTTP   http_req_duration.......................................................:  avg=495.55µs  min=116µs  med=449µs  max=5.49ms  p(90)=705µs  p(95)=820.39µs   {  expected_response:true  }............................................:  avg=495.55µs  min=116µs  med=449µs  max=5.49ms  p(90)=705µs  p(95)=820.39µs   http_req_failed.........................................................:  0.00%  0  out  of  2027   http_reqs...............................................................:  2027  40.146806/s    EXECUTION   iteration_duration......................................................:  avg=1s  min=1s  med=1s  max=1.01s  p(90)=1s  p(95)=1s   iterations..............................................................:  2027  40.146806/s   vus.....................................................................:  3  min=3  max=50   vus_max.................................................................:  50  min=50  max=50    NETWORK   data_received...........................................................:  266  kB  5.3  kB/s   data_sent...............................................................:  176  kB  3.5  kB/s     running  (0m50.5s),  00/50  VUs,  2027  complete  and  0  interrupted  iterations default  ✓  [======================================]  00/50  VUs  50s` 
```</details>

## 使用 `pprof` 分析网络化 Go 应用程序

分析大量使用网络的 Go 应用程序至关重要，这有助于识别和解决在高流量场景下影响性能的瓶颈。Go 内置的 `net/http/pprof` 包提供了针对网络密集型操作特别有益的见解。通过启用 HTTP 端点设置持续分析：

```go
`import  (    _  "net/http/pprof"  )  // ...    // Start pprof in a separate goroutine.   go  func()  {   log.Println("pprof listening on :6060")   if  err  :=  http.ListenAndServe("localhost:6060",  nil);  err  !=  nil  {   log.Fatalf("pprof server error: %v",  err)   }   }()` 
```

可以在实时状态下检查应用程序，即使在重负载下也是如此。要捕获 CPU 配置文件：

```go
`go  tool  pprof  http://localhost:6060/debug/pprof/profile?seconds=30` 
```

这会捕获 CPU 使用情况的 30 秒快照。通过暴露 `pprof` HTTP 服务器，所有运行时数据——CPU、内存、goroutines、竞争、火焰图——都可在不暂停或重新启动应用程序的情况下获得。

```go
`go  tool  pprof  -http=:7070  cpu.prof  #(1)` 
```

1.  实际的 `cpu.prof` 路径可能类似于 `$HOME/pprof/pprof.net-app.samples.cpu.004.pb.gz`

### CPU 分析

在静态系统中进行性能分析很少能讲出全部故事。真正的瓶颈在压力下出现——当请求堆积，线程竞争，内存碎片增加时。在负载期间进行 CPU 分析揭示了执行时间集中的地方，通常暴露出缓慢的序列化、低效的处理逻辑或 goroutines 之间的竞争。这些路径在流量扩展时默默地限制了吞吐量并增加了延迟。

#### 要查找的内容

+   网络序列化热点：在网络响应生成期间频繁使用 `json.Marshal` 或类似的序列化方法。

+   系统调用开销：广泛的系统调用使用（例如，`syscall.Read`）表明不高效的套接字处理或过度的阻塞 I/O。

+   GC 活动：`runtime.gc` 高频率表明不高效的内存管理影响了响应延迟。

**为什么这很重要：**在联网环境中识别和优化 CPU 密集型操作可以减少延迟，提高吞吐量，并在流量高峰期间提高可靠性。

### 火焰图

火焰图通过聚合和折叠堆栈跟踪到一个单一视图中，提供了 CPU 时间花费的视觉摘要。它们使得在不深入原始性能分析数据的情况下轻松识别性能热点。在服务器应用程序中，这通常突出了请求处理、序列化或阻塞 I/O 中的问题。在负载下，火焰图特别有用于捕捉到微小的低效性，这些低效性在扩展到主要性能问题时变得明显。

#### 要查找的内容

+   在火焰图中表现为宽块的与网络 I/O 或数据传输相关的函数通常指向在序列化、缓冲或套接字操作上花费过多时间。

+   深层调用链 深层堆栈可能揭示不高效的中间件或网络请求处理中不必要的层。

+   意外路径 寻找意外的序列化、反射或路由效率低下。

**为什么这很重要**：火焰图简化了复杂效率问题的视觉诊断，从而加快了优化并减少了停机时间。

### 管理垃圾收集（GC）压力

内存分析有助于确定您的应用程序在哪里浪费堆空间，尤其是在网络密集型代码路径中。常见问题包括重复分配响应缓冲区、临时对象或过度使用切片和映射。这些模式通常直到触发 GC 压力或负载下的延迟才会被发现。使用 `pprof` 进行分析遵循与 CPU 分析相同的基本步骤，使其易于集成到现有的工作流程中。

```go
`go  tool  pprof  http://localhost:6060/debug/pprof/heap` 
```

然后，再次，您可以交互式地查看结果。

```go
`go  tool  pprof  -http=:7070  mem.prof  #(1)` 
```

1.  实际的 `mem.prof` 路径可能类似于 `$HOME/pprof/pprof.net-app.alloc_objects.alloc_space.inuse_objects.inuse_space.003.pb.gz`

#### 需要关注的内容

+   频繁的临时缓冲区：网络缓冲区中分配的高频率，例如为每个请求重复创建字节切片。

+   持久网络对象：长期存在的网络连接或会话的积累。

+   过度的序列化开销：由于重复编码/解码网络有效负载而导致的高对象创建率。

示例：通过使用 `sync.Pool` 优化缓冲区重用，在大量网络操作期间大大减少了 GC 压力。

**为什么这很重要**：减少网络活动引起的内存 churn 可以提高响应时间并最小化由 GC 引起的延迟峰值。

### 识别 CPU 瓶颈

网络应用程序在持续负载下通常首先遇到 CPU 限制。分析有助于揭示实际花费时间和阻碍的因素。

#### 需要关注的内容

+   当吞吐量停滞时延迟上升：这是 CPU 过载的迹象，通常来自请求处理或序列化开销。

+   调度开销（runtime.schedule, mcall）：过多的 goroutines 可能会压倒调度器，尤其是在每个连接都有自己的处理器时。

+   锁竞争：在共享网络状态或阻塞通道操作上重复加锁会降低吞吐量并限制并行性。

例如，如果分析显示在 TLS 握手例程中花费了过多时间，修复可能涉及将握手从热点路径移除或通过连接重用减少握手频率。

**为什么这很重要**：CPU 瓶颈限制了您的扩展能力。修复它们通常是处理 5K 客户端和 50K 客户端之间的区别。

### 使用 `pprof` 分析网络 Go 应用程序的实用示例

为了实际说明这些概念，我们的演示应用程序集成了性能分析和基准测试工具，并提供了全面的性能分析和负载测试场景。演示涵盖了识别性能瓶颈、分析火焰图以及在各种模拟网络条件下的基准测试。

由于其显著的大小，使用`prof`对网络 Go 应用程序进行性能分析的实用示例是一篇单独的文章。

## 作为反馈循环的基准测试

单独进行一次负载测试运行意义不大。但如果你将基准测试视为开发周期的一部分——在更改前后——你开始构建性能叙事。你可以确切地看到更改是如何影响吞吐量，或者它是如何以内存开销为代价换取延迟的。

Go 标准库提供了`testing.B`用于微基准测试。结合使用`Vegeta`和`k6`等工具，将性能分析与稳健的集成测试作为 CI/CD 管道的一部分，这确保了早期检测回归、持续验证性能改进，并在实际生产条件下可靠地维护应用程序性能。
