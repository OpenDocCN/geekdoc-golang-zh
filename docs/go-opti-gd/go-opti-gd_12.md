# 懒初始化

> 原文：[`goperf.dev/01-common-patterns/lazy-init/`](https://goperf.dev/01-common-patterns/lazy-init/)

## Go 中的懒初始化性能

在 Go 中，一些资源初始化成本高昂，或者除非某些代码路径被触发，否则根本不必要。这就是懒初始化变得有用的地方：它将值的构建推迟到实际需要的时候。这种模式可以提高性能、减少启动开销，并避免不必要的操作——尤其是在高并发应用程序中。

### 懒初始化的重要性

在启动时初始化像数据库连接、缓存或大型内存结构这样的重资源可能会减慢应用程序的启动速度，并在实际需要之前消耗内存。懒初始化将这项工作推迟到第一次使用资源时，以保持启动快速和内存使用精简。

这也是一种实用的模式，当你拥有可能被多次触发但只需运行一次的逻辑时——确保昂贵的操作不会被重复执行，并且初始化在并发调用中保持安全且幂等的。

### 使用`sync.Once`进行线程安全初始化

Go 提供了`sync.Once`类型，以在并发环境中安全地实现懒初始化：

```go
`var  (   resource  *MyResource   once  sync.Once )  func  getResource()  *MyResource  {   once.Do(func()  {   resource  =  expensiveInit()   })   return  resource }` 
```

在这个例子中，函数`expensiveInit()`只执行一次，无论有多少 goroutine 并发调用`getResource()`。这确保了线程安全的初始化，而无需额外的同步开销。

### 使用`sync.OnceValue`和`sync.OnceValues`进行带有输出值的初始化

自从 Go 1.21 以来，如果你的初始化逻辑返回一个值，你可能更喜欢使用`sync.OnceValue`（单个值）或`sync.OnceValues`（多个值）来编写更简单、更易于表达代码：

```go
`var  getResource  =  sync.OnceValue(func()  *MyResource  {   return  expensiveInit() })  func  processData()  {   res  :=  getResource()   // use res }` 
```

在这里，`sync.OnceValue`提供了一种简洁的方式来封装一次性初始化逻辑，并在需要时直接返回计算值，而无需手动管理标志或互斥锁。它通过直接返回计算值来简化懒加载。

对于初始化器返回多个值的情况——例如资源和一个错误——`sync.OnceValues`扩展了相同的概念。它确保函数只运行一次，并干净地解包结果，使代码可读且线程安全，无需样板代码。

```go
`var  getConfig  =  sync.OnceValues(func()  (*Config,  error)  {   return  loadConfig("config.yml") })  func  processData()  {   config,  err  :=  getConfig()   if  err  !=  nil  {   log.Fatal(err)   }   // use config }` 
```

选择`sync.OnceValue`或`sync.OnceValues`可以帮助你清晰地表达初始化逻辑，并通过直接返回值来实现，而`sync.Once`则最适合需要灵活初始化逻辑且不立即返回值的通用场景。

### 使用原子操作进行自定义懒初始化

是的，技术上可以使用低级原子操作，如`atomic.CompareAndSwap`或`atomic.Load/Store`，用自定义逻辑替换`sync.Once`、`sync.OnceValue`或`sync.OnceFunc`。在罕见且性能关键的路由中，这可以避免标准类型带来的小开销或分配。

然而，这种权衡是复杂性的。你失去了标准原语的安全保证和清晰性，并且更容易引入微妙的错误——尤其是在并发情况下。除非分析表明 sync.Once 是瓶颈，否则标准版本几乎总是更好的选择。

**尽管如此，这种权衡很少值得。**

手动基于原子的初始化更容易出错，更难阅读，也更易出错——尤其是在涉及并发和内存可见性保证时。在绝大多数情况下，`sync.Once*` 更安全、更清晰，且性能足够。

信息

如果你确信在您的案例中基于原子的懒加载初始化是合理的，这篇博客文章将详细介绍细节和注意事项：

[使用原子在 Go 中进行懒加载初始化](https://goperf.dev/blog/2025/04/03/lazy-initialization-in-go-using-atomics/)

### 性能考虑

虽然懒加载初始化可以提供明显的优势，但它也带来了额外的复杂性。小心处理初始化很重要，以避免像竞态条件或并发错误这样的微妙问题。使用内置工具如 `sync.Once` 或 `atomic` 操作通常可以轻松确保线程安全。然而，始终通过分析来衡量实际改进是一个好主意，以确认懒加载初始化确实提高了启动速度、减少了内存使用或提高了应用程序的响应速度。

## 基准测试影响

通常没有特定于懒加载初始化本身进行基准测试的内容，因为主要好处是延迟昂贵的资源创建。性能提升本质上与避免不必要的初始化的成本、启动速度的提高和内存消耗的减少有关，而不是直接的运行时吞吐量差异。

## 选择懒加载初始化的时机

+   当资源初始化成本高昂或涉及 I/O 时。延迟构建可以避免支付设置成本——如打开文件、查询数据库或加载大型结构——除非实际上需要。

+   为了提高启动性能和内存效率。将工作推迟到首次使用允许你的应用程序更快启动，并避免为可能永远不会使用的资源分配内存。

+   当在运行时不需要立即或根本不需要所有资源时。懒加载初始化可以帮助你避免初始化仅在特定代码路径中适用的字段或服务。

+   为了确保代码块在重复调用的情况下恰好执行一次。使用像 `sync.Once` 这样的工具确保在并发环境中进行线程安全的单次设置。
