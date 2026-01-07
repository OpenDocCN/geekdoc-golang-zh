# 避免接口封装[]

> 原文：[https://goperf.dev/01-common-patterns/interface-boxing/](https://goperf.dev/01-common-patterns/interface-boxing/)

Go 的接口使得编写灵活、解耦的代码变得容易。但在这便利的背后，有一个可能会影响性能的细节：当一个具体值被赋给接口时，Go 会将其封装在一个隐藏的结构体中——这个过程称为接口封装。

在许多情况下，封装是无害的。但在性能敏感的代码中——如紧密循环、热点路径或高吞吐量服务——它可能会引入隐藏的堆分配、额外的内存复制，并增加垃圾收集器的压力。这些影响在开发过程中往往不易察觉，只有在出现延迟峰值或内存膨胀时才会显现。

## 什么是接口封装?

接口封装是指将具体值转换为接口类型的过程。在 Go 中，接口值在内部表示为两个词：

+   一个 **类型描述符**，它包含有关具体类型的信息（其身份和方法集）。

+   一个 **数据指针**，它指向实际存储的值。

当你将值赋给接口变量时，Go 会创建这个两部分的结构。如果值是一个非指针类型——比如结构体或原始类型——并且尚未在堆上，Go **可能**会在堆上分配它的副本以满足接口赋值。这种行为在处理大值或存储在接口切片中的项目时尤其相关，其中每个元素都会单独封装。这些隐式分配可能会累积起来，并且是 Go 程序中隐藏内存压力的常见来源。

这里有一个简单的例子：

```go
var i interface{}
i = 42 
```

在这种情况下，整数 `42` 被封装在一个接口中：Go 存储了类型信息（`int`）和值 `42` 的副本。对于像 `int` 这样的小值来说，这并不昂贵，但对于大型结构体来说，成本就变得相当可观了。

另一个例子：

```go
type Shape interface {
    Area() float64
}

type Square struct {
    Size float64
}

func (s Square) Area() float64 { return s.Size * s.Size }

func main() {
    var shapes []Shape
    for i := 0; i < 1000; i++ {
        s := Square{Size: float64(i)}
        shapes = append(shapes, s) // boxing occurs here
    }
} 
```

警告

**请注意此代码！** 在这个例子中，尽管 `shapes` 是一个接口切片，但每个 `Square` 值在追加到 `shapes` 时都会被复制到接口中。如果 `Square` 是一个大型结构体，这将引入 1000 个分配和大量的内存复制。

为了避免这种情况，你可以传递指针：

```go
 shapes = append(shapes, &s) // avoids large struct copy 
```

这样，接口中只存储了一个 8 字节的指针，减少了分配大小和复制开销。

## 为什么这很重要

在紧密循环或高吞吐量路径中，例如反序列化 JSON、渲染模板或处理大型集合时，接口封装可能会通过触发不必要的堆分配和增加垃圾收集压力来降低性能。这种开销在高并发或实时响应约束的系统中的成本尤其高昂。

封装也可能使分析和基准测试产生误导，因为归因于看似无害的行的分配实际上可能源于对接口的隐式转换。

## 基准测试影响

对于基准测试，我们将定义一个接口和一个实现该接口的具有显著负载的结构体。

```go
type Worker interface {
    Work()
}

type LargeJob struct {
    payload [4096]byte
}

func (LargeJob) Work() {} 
```

### 装箱大结构体

为了展示装箱大值与指针的实际影响，我们基准测试了将1000个大结构体赋值给接口切片的成本：

```go
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
```

基准测试结果

| Benchmark | 每操作时间（ns） | 每操作字节（B） | 每操作分配（Allocs） |
| --- | --- | --- | --- |
| BoxedLargeSliceGrowth | 404,649 | ~4.13 MB | 1011 |
| PointerLargeSliceGrowth | 340,549 | ~4.13 MB | 1011 |

装箱大值要慢得多——在这个例子中大约19%——因为每个接口赋值都要复制整个4KB的结构体。然而，装箱指针避免了这种成本，并保持复制小（仅8字节）。虽然两种方法分配的总内存相同（因为所有值都逃逸到堆上），但在压力下指针装箱具有明显的性能优势。

### 将值传递给接受接口的函数时

另一个常见的装箱来源是当将大值直接传递给接受接口的函数时。即使没有存储到切片中，在调用位置也会发生装箱。

```go
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
```

基准测试结果

| Benchmark | ns/op | B/op | allocs/op |
| --- | --- | --- | --- |
| CallWithValue | 422.5 | 4096 | 1 |
| CallWithPointer | 379.9 | 4096 | 1 |

将值传递给期望接口的函数会导致装箱，复制整个结构体并在堆上分配。在我们的基准测试中，这导致与使用指针相比大约11%的CPU成本增加。传递指针避免了复制结构体，减少了内存移动，并导致更小、更缓存友好的接口值，使其在性能敏感的场景中成为更有效的选择。

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

## 当接口装箱可接受时

尽管在某些情况下接口装箱对性能有影响，但通常它是完全合理的——有时甚至是首选的。

### 当抽象比性能更重要时

接口可以实现解耦和模块化。如果你正在设计一个干净、可测试的API，装箱的成本与抽象的好处相比可以忽略不计。

```go
type Storage interface {
    Save([]byte) error
}
func Process(s Storage) { /* ... */ } 
```

### 当值很小且装箱不涉及分配时

将小的、可复制的值如 `int`、`float64` 或小的结构体进行装箱通常不会引起内存分配。

```go
var i interface{}
i = 123 // safe and cheap 
```

### 当值是短生命期时

如果装箱值被短暂使用（例如，用于日志记录或基于接口的排序），开销很小。

```go
fmt.Println("value:", someStruct) // implicit boxing is fine 
```

### 当需要动态行为时

接口允许运行时多态。如果你需要不同类型实现相同的行为，装箱是必要的且是惯用的。

```go
for _, s := range []Shape{Circle{}, Square{}} {
    fmt.Println(s.Area())
} 
```

当支持清晰度、可重用性或设计目标时使用装箱，仅在性能关键路径的代码中避免使用。

## 如何避免接口装箱

+   在赋值给接口时使用指针。如果方法集需要指针接收者或值较大，应显式传递指针以避免重复复制和堆分配。

    ```go
    for i := range tasks {
       result = append(result, &tasks[i]) // Avoids boxing copies
    } 
    ```

+   避免在热点路径中使用接口。如果已知具体的类型且稳定，应完全避免接口间接引用——尤其是在计算密集型或分配敏感的函数中。

+   使用特定类型的容器。在可行的情况下，而不是使用`[]interface{}`，更倾向于使用泛型切片或类型化集合。这保留了静态类型并减少了不必要的分配。

+   使用pprof进行基准测试和检查。使用`go test -bench`和`pprof`来观察分配发生的位置。如果分配点在`runtime.convT2E`（将T转换为接口），你很可能是进行了装箱。
