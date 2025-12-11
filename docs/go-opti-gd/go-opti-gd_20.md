# 实际示例：使用 pprof 剖析网络 Go 应用程序

> 原文：[`goperf.dev/02-networking/gc-endpoint-profiling/`](https://goperf.dev/02-networking/gc-endpoint-profiling/)

本节通过一个使用基准测试工具和运行时剖析的演示应用程序，将剖析概念应用于实际场景。它涵盖了识别性能瓶颈、解释火焰图以及在各种模拟网络条件下的系统行为分析。

## 网络应用程序中的 CPU 剖析

演示应用程序被有意设计得尽可能简单，以突出关键剖析概念，而不引入不必要的复杂性。虽然演示中使用的代码和模式很基础，但在这里获得的剖析见解高度适用于更复杂、生产级的应用程序。

为了在负载下实现连续的剖析，我们通过一个专门的 HTTP 端点公开了`pprof`：

```go
`import  (    _  "net/http/pprof"  )  // ...    // Start pprof in a separate goroutine.   go  func()  {   log.Println("pprof listening on :6060")   if  err  :=  http.ListenAndServe("localhost:6060",  nil);  err  !=  nil  {   log.Fatalf("pprof server error: %v",  err)   }   }()` 
```

<details class="example"><summary>完整的`net-app`源代码</summary>

```go
`package  main  // pprof-start import  ( // pprof-end   "flag"   "fmt"   "log"   "math/rand/v2"   "net/http" // pprof-start   _  "net/http/pprof" // pprof-end   "os"   "os/signal"   "time" // pprof-start ) // pprof-end  var  (   fastDelay  =  flag.Duration("fast-delay",  0,  "Fixed delay for fast handler (if any)")   slowMin  =  flag.Duration("slow-min",  1*time.Millisecond,  "Minimum delay for slow handler")   slowMax  =  flag.Duration("slow-max",  300*time.Millisecond,  "Maximum delay for slow handler")   gcMinAlloc  =  flag.Int("gc-min-alloc",  50,  "Minimum number of allocations in GC heavy handler")   gcMaxAlloc  =  flag.Int("gc-max-alloc",  1000,  "Maximum number of allocations in GC heavy handler") )  func  randRange(min,  max  int)  int  {   return  rand.IntN(max-min)  +  min }  func  fastHandler(w  http.ResponseWriter,  r  *http.Request)  {   if  *fastDelay  >  0  {   time.Sleep(*fastDelay)   }   fmt.Fprintln(w,  "fast response") }  func  slowHandler(w  http.ResponseWriter,  r  *http.Request)  {   delayRange  :=  int((*slowMax  -  *slowMin)  /  time.Millisecond)   delay  :=  time.Duration(randRange(1,  delayRange))  *  time.Millisecond   time.Sleep(delay)   fmt.Fprintf(w,  "slow response with delay %d ms\n",  delay.Milliseconds()) }  // heavy-start var  longLivedData  [][]byte  func  gcHeavyHandler(w  http.ResponseWriter,  r  *http.Request)  {   numAllocs  :=  randRange(*gcMinAlloc,  *gcMaxAlloc)   var  data  [][]byte   for  i  :=  0;  i  <  numAllocs;  i++  {   // Allocate 10KB slices. Occasionally retain a reference to simulate long-lived objects.   b  :=  make([]byte,  1024*10)   data  =  append(data,  b)   if  i%100  ==  0  {  // every 100 allocations, keep the data alive   longLivedData  =  append(longLivedData,  b)   }   }   fmt.Fprintf(w,  "allocated %d KB\n",  len(data)*10) } // heavy-end  func  main()  {   flag.Parse()    http.HandleFunc("/fast",  fastHandler)   http.HandleFunc("/slow",  slowHandler)   http.HandleFunc("/gc",  gcHeavyHandler)  // pprof-start // ...    // Start pprof in a separate goroutine.   go  func()  {   log.Println("pprof listening on :6060")   if  err  :=  http.ListenAndServe("localhost:6060",  nil);  err  !=  nil  {   log.Fatalf("pprof server error: %v",  err)   }   }() // pprof-end    // Create a server to allow for graceful shutdown.   server  :=  &http.Server{Addr:  ":8080"}    go  func()  {   log.Println("HTTP server listening on :8080")   if  err  :=  server.ListenAndServe();  err  !=  nil  &&  err  !=  http.ErrServerClosed  {   log.Fatalf("HTTP server error: %v",  err)   }   }()    // Graceful shutdown on interrupt signal.   sigCh  :=  make(chan  os.Signal,  1)   signal.Notify(sigCh,  os.Interrupt)   <-sigCh   log.Println("Shutting down server...")   if  err  :=  server.Shutdown(nil);  err  !=  nil  {   log.Fatalf("Server Shutdown Failed:%+v",  err)   }   log.Println("Server exited") }` 
```</details>

下一步将是与被剖析的应用程序建立连接并收集样本：

```go
`go  tool  pprof  http://localhost:6060/debug/pprof/profile?seconds=30` 
```

交互式查看结果：

```go
`go  tool  pprof  -http=:7070  cpu.prof  # (1)` 
```

1.  实际的`cpu.prof`路径可能类似于`$HOME/pprof/pprof.net-app.samples.cpu.004.pb.gz`

或者您可以将剖析图保存为`svg`图像。

## CPU 剖析流程：对`/gc`端点的负载

我们在针对`/gc`端点的 30 秒负载测试期间对应用程序进行了剖析，以查看在内存压力下的情况。这个处理器被有意设计为触发分配并强制进行垃圾收集，这使得它成为观察压力下运行时行为的理想候选。

我们使用了 Go 的内置剖析器来捕获 CPU 跟踪：

<details class="example"><summary>`/gc`端点的 CPU 剖析跟踪</summary>

![](img/cpu.prof.png)</details>

这为我们提供了 3.02 秒的样本 CPU 活动，占用了 30 秒的墙钟时间——这是一个了解在压力下运行时和应用程序所做事情的有用窗口。

## 时间都花在哪里了

### HTTP 堆栈主导了表面

如预期，大部分 CPU 时间都花在了请求处理上：

+   `http.(*conn).serve`占用了近 58%的样本时间

+   `http.serverHandler.ServeHTTP`也显著出现

这与我们的持续流量是一致的。Go HTTP 堆栈正在做大部分工作，管理连接和分发请求。

### 垃圾收集开销明显可见

大部分 CPU 时间都花在了垃圾收集器内部：

+   `runtime.gcDrain`、`runtime.scanobject`和`runtime.gcBgMarkWorker`都处于活跃状态

+   结合与内存相关的功能，如`runtime.mallocgc`，这些大约占用了总 CPU 时间的 20%

这证实了`gcHeavyHandler`达到了其目标。我们关心的是这种分配压力是否会泄漏到现实世界的处理器中。如果确实如此，我们将在延迟和 CPU 轮换中付出代价。

### I/O 和系统调用占据了很大一部分

我们还看到了很高的系统调用活动——特别是来自：

+   `syscall.syscall`（与`poll`、`Read`和`Write`相关联）

+   `bufio.Writer.Flush`和`http.response.finishRequest`

这些函数反映了将响应写回客户端的成本。对于简单的处理程序，这是预期的。但如果你的处理逻辑很轻量级，大部分时间都花在通过 TCP 刷新数据上，那么优化有效载荷或缓冲区策略就值得考虑。

### 调度器活动非同小可

`runtime.schedule`、`mcall`和`findRunnable`等函数也出现在列表中。这些是 Go 运行时内部负责管理 goroutines 的函数。在高并发测试期间看到它们并不罕见——但如果它们占主导地位，通常表明 goroutine churn 或阻塞行为过多。

## 内存分析：来自`/gc`端点的保留堆

我们还捕获了一个内存分析，以补充对`/gc`端点的 CPU 视图。此分析使用了`inuse_space`指标，它显示了在捕获时每个函数实际保留的堆内存量。

我们通过以下方式触发了分析：

```go
`go  tool  pprof  -http=:7070  http://localhost:6060/debug/pprof/heap` 
```

<details class="example"><summary>`/gc`端点的内存分析</summary>

![](img/mem.prof.png)</details>

在捕获时，应用程序保留了 649MB 的堆内存，其中几乎全部——99.46%——归因于单个函数：`gcHeavyHandler`。这是预期的。处理程序通过在紧密循环中创建 10KB 的切片来模拟分配压力。每 100 个切片被添加到一个全局变量中，以模拟长期内存。

下面是处理程序所做的工作：

```go
`var  longLivedData  [][]byte  func  gcHeavyHandler(w  http.ResponseWriter,  r  *http.Request)  {   numAllocs  :=  randRange(*gcMinAlloc,  *gcMaxAlloc)   var  data  [][]byte   for  i  :=  0;  i  <  numAllocs;  i++  {   // Allocate 10KB slices. Occasionally retain a reference to simulate long-lived objects.   b  :=  make([]byte,  1024*10)   data  =  append(data,  b)   if  i%100  ==  0  {  // every 100 allocations, keep the data alive   longLivedData  =  append(longLivedData,  b)   }   }   fmt.Fprintf(w,  "allocated %d KB\n",  len(data)*10) }` 
```

火焰图证实了我们的预期：

+   `gcHeavyHandler`占用了几乎所有在用的内存。

+   跟踪的路径从 HTTP 连接开始，经过 Go 路由器堆栈，进入处理逻辑。

+   没有显著的分配来自其他地方——这是一个集中、受控的内存压力场景。

这种类型的分析很有价值，因为它揭示了内存中仍然保留的内容，而不仅仅是已分配的内容。这种视图通常对于诊断泄漏、保留缓冲区或遗忘的引用最有揭示性。

## 摘要：`/gc`端点的 CPU 和内存分析

`/gc`端点是故意构建来模拟高分配压力和 GC 活动的。在负载下对处理程序进行分析，为我们提供了一个清晰、集中的视角，展示了 Go 运行时在内存极限被推到时的行为。

从**CPU 分析**中，我们看到了以下情况：

+   如预期的那样，在持续负载期间，大部分时间都花在了 HTTP 处理路径上。

+   近 20%的 CPU 样本归因于内存分配和垃圾回收。

+   系统调用活动很高，主要来自写入响应。

+   Go 调度器适度活跃，管理处理流量的并发 goroutines。

从 **内存配置文件** 中，我们捕捉到了 649MB 的活跃堆使用量，其中 **99.46% 被由 `gcHeavyHandler` 保留**。这符合我们的预期：处理程序故意保留每 100 个 10KB 分配，以模拟长期数据。

这些配置文件共同让我们有信心，在合成压力下 `/gc` 端点表现如预期：

+   它创造了有意义的 CPU 和内存负载。

+   它揭示了持续分配和垃圾回收周期的成本。

+   它提供了一个可预测的环境来测试优化或垃圾回收调整策略。
