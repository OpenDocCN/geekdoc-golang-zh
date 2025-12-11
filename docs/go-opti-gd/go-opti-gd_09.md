# 栈分配和逃逸分析

> 原文：[`goperf.dev/01-common-patterns/stack-alloc/`](https://goperf.dev/01-common-patterns/stack-alloc/)

当编写性能关键型 Go 应用程序时，您可以进行的微妙但重要的优化之一是鼓励值在栈上而不是在堆上进行分配。栈分配更便宜、更快且无垃圾回收——但 Go 并不总是自动将变量放在那里。这个决定是由 Go 编译器在 **逃逸分析** 期间做出的。

在本文中，我们将探讨逃逸分析是什么，如何阅读编译器的逃逸诊断，什么原因会导致值逃逸，以及如何构建代码以最小化不必要的堆分配。我们还将基准测试不同的场景以展示其对现实世界的影响。

## 逃逸分析是什么？

逃逸分析是 Go 编译器执行的一种静态分析，用于确定变量是否可以安全地分配在栈上，或者是否必须移动（“逃逸”）到堆上。

### 这为什么很重要？

+   **栈分配** 便宜：当函数返回时，内存会自动释放。

+   **堆分配** 更昂贵：它们涉及垃圾回收开销。

编译器根据变量的使用方式来决定其位置。如果一个变量可以保证不会超出其声明函数的作用域，它就可以留在栈上。如果不能，它就会逃逸到堆上。

### 示例：栈与堆

```go
`func  allocate()  *int  {   x  :=  42   return  &x  // x escapes to the heap }  func  noEscape()  int  {   x  :=  42   return  x  // x stays on the stack }` 
```

在 `allocate` 中，`x` 作为指针返回。由于指针逃出了函数，Go 编译器将 `x` 放在堆上。在 `noEscape` 中，`x` 是一个普通值，没有逃逸。

## 如何查看逃逸分析输出

您可以使用 `-gcflags` 编译器选项来检查逃逸分析：

```go
`go  build  -gcflags="-m"  ./path/to/pkg` 
```

或者对于特定的文件：

```go
`go  run  -gcflags="-m"  main.go` 
```

这将打印出类似以下内容的行：

```go
`main.go:10:6: moved to heap: x main.go:14:6: can inline noEscape` 
```

寻找类似 `moved to heap` 的消息来识别逃逸点。

## 导致变量逃逸的原因是什么？

这里是一些强制进行堆分配的常见场景：

### 返回局部变量的指针

```go
`func  escape()  *int  {   x  :=  10   return  &x  // escapes }` 
```

### 在闭包中捕获变量

```go
`func  closureEscape()  func()  int  {   x  :=  5   return  func()  int  {  return  x  }  // x escapes }` 
```

### 接口转换

当一个值存储在接口中时，它可能会逃逸：

```go
`func  toInterface(i  int)  interface{}  {   return  i  // escapes if type info needed at runtime }` 
```

### 将值赋给全局变量或结构体字段

```go
`var  global  *int  func  assignGlobal()  {   x  :=  7   global  =  &x  // escapes }` 
```

### 大型复合字面量

即使它们没有严格逃逸，Go 也可能在堆上分配大的结构体或切片。

```go
`func  makeLargeSlice()  []int  {   s  :=  make([]int,  10000)  // may escape due to size   return  s }` 
```

## 基准测试栈与堆分配

让我们运行一个基准测试来探索堆分配实际上何时发生——以及即使我们返回一个指针，它们何时不会发生。

```go
`func  StackAlloc()  Data  {   return  Data{1,  2,  3}  // stays on stack }  func  HeapAlloc()  *Data  {   return  &Data{1,  2,  3}  // escapes to heap }  func  BenchmarkStackAlloc(b  *testing.B)  {   for  b.Loop()  {   _  =  StackAlloc()   } }  func  BenchmarkHeapAlloc(b  *testing.B)  {   for  b.Loop()  {   _  =  HeapAlloc()   } }` 
```

基准测试结果

| Benchmark | 迭代次数 | 每次操作时间 (ns) | 每次操作字节数 | 每次操作分配数 |
| --- | --- | --- | --- | --- |
| BenchmarkStackAlloc-14 | 1,000,000,000 | 0.2604 ns | 0 B | 0 |
| BenchmarkHeapAlloc-14 | 1,000,000,000 | 0.2692 ns | 0 B | 0 |

您可能期望 `HeapAlloc` 总是分配内存到堆上——但在这里不是。这是因为编译器很聪明：在这个隔离的基准测试中，`HeapAlloc` 返回的指针以任何有意义的方式都没有逃出函数。编译器可以看到它只用于基准测试，并且是短命的，因此它也安全地将它放在栈上。

### 强制堆分配

```go
`var  sink  *Data  func  HeapAllocEscape()  {   d  :=  &Data{1,  2,  3}   sink  =  d  // d escapes to heap }  func  BenchmarkHeapAllocEscape(b  *testing.B)  {   for  b.Loop()  {   HeapAllocEscape()   } }` 
```

| 基准 | 迭代次数 | 每次操作时间（ns） | 每次操作字节 | 每次操作分配 |
| --- | --- | --- | --- | --- |
| 基准 HeapAllocEscape-14 | 331,469,049 | 10.55 ns | 24 B | 1 |

如`BenchmarkHeapAllocEscape`所示，将指针分配给全局变量会导致真正的堆逃逸。这引入了真正的开销：40 倍更慢的调用、24 字节的分配以及每次调用一个垃圾回收对象。

<details class="example"><summary>显示基准文件</summary>

```go
`package  main  import  "testing"  type  Data  struct  {   A,  B,  C  int }  // heap-alloc-start func  StackAlloc()  Data  {   return  Data{1,  2,  3}  // stays on stack }  func  HeapAlloc()  *Data  {   return  &Data{1,  2,  3}  // escapes to heap }  func  BenchmarkStackAlloc(b  *testing.B)  {   for  b.Loop()  {   _  =  StackAlloc()   } }  func  BenchmarkHeapAlloc(b  *testing.B)  {   for  b.Loop()  {   _  =  HeapAlloc()   } } // heap-alloc-end  // escape-start var  sink  *Data  func  HeapAllocEscape()  {   d  :=  &Data{1,  2,  3}   sink  =  d  // d escapes to heap }  func  BenchmarkHeapAllocEscape(b  *testing.B)  {   for  b.Loop()  {   HeapAllocEscape()   } } // escape-end` 
```</details>

## 当优化栈分配时

并非所有的逃逸都值得防止。以下是在关注栈分配时合理的情况，以及何时最好让值逃逸。

避免逃逸的时机

+   在性能关键路径上。在紧密循环或延迟敏感的代码中减少堆使用可以降低垃圾回收压力并加快执行速度。

+   对于短暂存在的小对象。这些对象可以有效地进行栈分配，而不涉及垃圾回收器，从而减少内存碎片。

+   当你控制完整的调用链。如果对象保持在你的代码中，并且你可以重构它以避免逃逸，那么进行小的重构通常是值得的。

+   如果分析显示存在垃圾回收瓶颈。逃逸分析可以帮助你针对并缩小在现实世界跟踪中识别出的内存密集型分配。

当允许值逃逸是可行的

+   当从构造函数或工厂返回值时。从`NewThing()`返回指针是 Go 的惯用做法——即使它会导致逃逸，但它提高了清晰度和可用性。

+   当对象必须超出函数的生命周期。如果你在全局存储数据、发送到 goroutine 或将其保存在结构体中，逃逸是必要且正确的。

+   当分配大小小且不频繁。如果堆分配不在热点路径上，避免它的好处通常可以忽略不计。

+   当防止逃逸损害可读性。为了微优化而编写不自然的代码，将所有内容保持在栈上，可能会降低可维护性，而这种微优化可能并不重要。
