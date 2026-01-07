# 不可变数据共享

> 原文：[https://goperf.dev/01-common-patterns/immutable-data/](https://goperf.dev/01-common-patterns/immutable-data/)

高性能 Go 程序中常见的性能瓶颈之一是并发下对共享数据的访问方式。常用的工具——互斥锁和通道——效果很好，但它们不是免费的。如果许多 goroutine 尝试获取相同的锁，互斥锁可能会成为瓶颈。通道虽然优雅地用于协调，但可能会引入阻塞，使控制流更难推理。两者都需要谨慎使用：如果同步不紧密，很容易引入微妙的错误或意外的性能问题。

一个强大的替代方案是不可变数据共享。不是用锁来保护数据，而是设计你的系统，使得创建后的共享数据永远不会被修改。这最小化了竞争并简化了程序推理。

## 为什么使用不可变数据？

不可变性为并发程序带来了几个优点：

+   无需锁：多个 goroutine 可以安全地读取不可变数据，无需同步。

+   更容易推理：如果数据不能改变，你可以避免整个类别的竞态条件。

+   写时复制优化：你可以创建结构的新版本而不改变原始版本，这对于配置重新加载或版本化状态很有用。

## 实际示例：共享配置

想象你有一个长时间运行的服务，它定期从磁盘或远程源重新加载其配置。多个 goroutine 读取此配置以做出决策。

以下是不可变数据如何帮助的示例：

### 第 1 步：定义 Config 结构体

```go
// config.go
type Config struct {
    LogLevel string
    Timeout  time.Duration
    Features map[string]bool // This needs attention!
} 
```

### 第 2 步：确保深度不可变性

Go 中的 Maps 和 Slices 是引用类型。即使 Config 结构体没有改变，有人也可能意外地修改共享的 Map。为了防止这种情况，我们创建防御性副本：

```go
func NewConfig(logLevel string, timeout time.Duration, features map[string]bool) *Config {
    copiedFeatures := make(map[string]bool, len(features))
    for k, v := range features {
        copiedFeatures[k] = v
    }

    return &Config{
        LogLevel: logLevel,
        Timeout:  timeout,
        Features: copiedFeatures,
    }
} 
```

现在，每个配置实例都是自包含的，可以安全地共享。

### 第 3 步：原子交换

使用 `atomic.Value` 存储并安全地更新当前配置。

```go
var currentConfig atomic.Pointer[Config]

func LoadInitialConfig() {
    cfg := NewConfig("info", 5*time.Second, map[string]bool{"beta": true})
    currentConfig.Store(cfg)
}

func GetConfig() *Config {
    return currentConfig.Load()
} 
```

现在，所有 goroutine 都可以安全地调用 `GetConfig()` 而无需加锁。当配置重新加载时，只需 `Store` 一个新的不可变副本。

### 第 4 步：在处理器中使用它

```go
func handler(w http.ResponseWriter, r *http.Request) {
    cfg := GetConfig()
    if cfg.Features["beta"] {
        // Enable beta path
    }
    // Use cfg.Timeout, cfg.LogLevel, etc.
} 
```

## 实际示例：不可变路由表

假设你正在构建一个轻量级的反向代理或 API 网关，必须根据路径或主机路由传入的请求。路由表每秒被读取数千次，但只偶尔更新（例如，从配置文件或服务发现）。

### 第 1 步：定义路由结构体

```go
type Route struct {
    Path    string
    Backend string
}

type RoutingTable struct {
    Routes []Route
} 
```

### 第 2 步：构建不可变版本

为了确保不可变性，我们在构建新的路由表时深度复制路由片段。

```go
func NewRoutingTable(routes []Route) *RoutingTable {
    copied := make([]Route, len(routes))
    copy(copied, routes)
    return &RoutingTable{Routes: copied}
} 
```

### 步骤 3：原子存储

```go
var currentRoutes atomic.Pointer[RoutingTable]

func LoadInitialRoutes() {
    table := NewRoutingTable([]Route{
        {Path: "/api", Backend: "http://api.internal"},
        {Path: "/admin", Backend: "http://admin.internal"},
    })
    currentRoutes.Store(table)
}

func GetRoutingTable() *RoutingTable {
    return currentRoutes.Load()
} 
```

### 步骤 4：并发路由请求

```go
func routeRequest(path string) string {
    table := GetRoutingTable()
    for _, route := range table.Routes {
        if strings.HasPrefix(path, route.Path) {
            return route.Backend
        }
    }
    return ""
} 
```

现在，你的路由逻辑可以在负载下安全地扩展，且没有锁定开销。

## 可扩展不可变路由表

随着系统的发展，路由表可以扩展到数百甚至数千个条目。虽然不可变性带来了明显的优势——安全并发访问、可预测的行为——但如果每次更新都意味着复制整个结构，这就会变得昂贵。在某个点上，为每个小变化重建整个表不再可扩展。

为了在不每次更新时都支付完整重建的费用的情况下保持不可变性，设计需要进化。有几种方法可以实现这一点——每种方法都保留了核心优势，同时减少了开销。

### 场景 1：分段路由

想象一个多租户系统，其中每个客户都有自己的路由规则集。你不必有一个巨大的路由片段，可以将它们拆分为一个映射：

```go
type MultiTable struct {
    Tables map[string]RoutingTable // key = tenant ID
} 
```

如果只有客户 "acme" 更新了他们的规则，你只需克隆那个片段并更新映射。然后你原子性地交换完整映射的新版本。所有其他租户继续使用他们现有的、未受影响的路由表。

这种方法减少了内存压力，加快了更新速度，同时保持了不可变性。它还隔离了爆炸半径：一个段中的损坏规则集不会影响其他段。

### 场景 2：索引路由表

假设你的路由器通过精确路径匹配，并且查找速度至关重要。你可以使用 `map[string]RouteHandler` 作为索引：

```go
type RouteIndex map[string]RouteHandler 
```

当添加新路径时，克隆当前映射，添加新路由，并发布新版本。因为映射很浅，所以对于中等数量的路由来说，这很快。读取是常数时间，更新效率高，因为只有结构的一小部分发生变化。

### 场景 3：混合预演和发布

假设你正在进行批量更新——可能从数据库中读取数百个路由。你不必重建实时数据，而是保持一个可变的预演区域：

```go
var mu sync.Mutex
var stagingRoutes []Route 
```

你在互斥锁下加载和操作数据，然后将其转换为不可变的 `RoutingTable` 并原子性地存储。这让你可以在不锁定读取者或影响实时流量的情况下安全地准备复杂更改。

## 基准测试影响

在现实世界系统中以通用、有意义的方式对不可变数据共享进行基准测试是困难的。结构大小、读写比和内存布局等因素都会严重影响结果。

而不是在这里提供人工基准，我们建议回顾[原子操作和同步原语](../atomic-ops/#benchmarking-impact)文章中的结果。这些基准明确说明了使用atomic.Value而不是传统的同步原语（如sync.RWMutex）在高度并发读取场景中可能带来的性能优势。

## 何时使用此模式

当以下情况时，不可变数据共享是理想的：

+   数据读取密集而写入轻（例如，配置、功能标志、全局映射）。这效果很好，因为创建新不可变版本的成本被分摊到许多读取中，避免加锁提供了性能提升。

+   你希望在不妨碍安全性的情况下最小化加锁。通过共享只读数据，你消除了对互斥锁或协调的需求，减少了死锁或竞态条件发生的可能性。

+   你可以容忍更新和读取之间的轻微延迟（最终一致性）。由于数据更新没有与读取者协调，所有goroutines看到新版本之前可能会有短暂的延迟。如果精确时间不是关键，这种权衡可以简化你的并发模型。

当更新必须在多个数据项之间进行事务性操作或频繁发生时，则不太适用。在这些情况下，重复复制或缺乏协调的成本可能会超过其带来的好处。
