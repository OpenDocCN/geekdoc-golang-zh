<!--yml

类别：未分类

日期：2024-10-13 06:03:30

-->

# Go（Golang）中的单例设计模式

> 来源：[https://golangbyexample.com/singleton-design-pattern-go/](https://golangbyexample.com/singleton-design-pattern-go/)

注意：有兴趣了解如何在GO中实现所有其他设计模式。请查看这个完整参考 - [Go中的所有设计模式（Golang）](https://golangbyexample.com/all-design-patterns-golang/)

**简介：**

单例设计模式是一种创建型设计模式，也是最常用的设计模式之一。当只应存在一个结构体实例时，使用此模式。这个单一实例称为单例对象。单例对象适用的一些情况：

1.  **DB实例** - 我们只想创建一个DB对象的实例，并且该实例将在整个应用程序中使用。

1.  **日志实例** - 仍然应该只创建一个日志实例，并在整个应用程序中使用。

当结构体首次初始化时，创建单例实例。通常，在结构体上定义一个getInstance()方法，只需要创建一个实例。一旦创建，每次调用**getInstance()**时返回的都是同一单例实例。

在GO中我们有goroutines。因此，单例结构体应该在多个goroutines尝试访问该实例时返回相同的实例。实现单例设计模式很容易出错。下面的代码说明了创建单例对象的正确方法。

```go
var lock = &sync.Mutex{}

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        lock.Lock()
        defer lock.Unlock()
        if singleInstance == nil {
            fmt.Println("Creting Single Instance Now")
            singleInstance = &single{}
        } else {
            fmt.Println("Single Instance already created-1")
        }
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

上述代码确保只创建一个单一结构体的实例。有几点值得注意。

1.  开始时有一个对nil **singleInstance**的检查。这是为了防止每次调用**getInstance()**方法时进行昂贵的锁操作。如果这个检查失败，则意味着**singleInstance**已经创建。

1.  **singleInstance**是在锁内创建的。

1.  锁定后还有一个对nil **singleInstance**的检查。这是为了确保如果多个goroutine绕过第一次检查，那么只有一个goroutine能够创建单例实例，否则每个goroutine将创建自己的**single**结构体实例。

这是完整代码

**single.go**

```go
package main

import (
    "fmt"
    "sync"
)

var lock = &sync.Mutex{}

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        lock.Lock()
        defer lock.Unlock()
        if singleInstance == nil {
            fmt.Println("Creting Single Instance Now")
            singleInstance = &single{}
        } else {
            fmt.Println("Single Instance already created-1")
        }
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

**main.go**

```go
package main

import (
    "fmt"
)

func main() {
    for i := 0; i < 100; i++ {
        go getInstance()
    }
    // Scanln is similar to Scan, but stops scanning at a newline and
    // after the final item there must be a newline or EOF.
    fmt.Scanln()
}
```

**输出：**

```go
Creting Single Instance Now
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-2
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-2
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
Single Instance already created-1
```

**注意：**

+   有一个输出为**"现在创建单实例"**，这意味着只有一个goroutine能够创建单一实例。

+   有几个输出为**"单实例已创建-1"**，这意味着一些goroutines在第一次检查中发现singleInstance的值为nil并绕过了它。

+   有几个输出为**"单实例已创建-2"**，这意味着当它们到达时，单实例已经创建，它们无法绕过第一次if检查。

**在Go中创建单例对象的其他方法**

+   **init()函数**

我们可以在init函数内创建一个单实例。这仅适用于对象的提前初始化是可以的。init函数在一个包的每个文件中只会被调用一次，因此我们可以确保只创建一个单实例。

+   **sync.Once**

sync.Once只会执行一次操作。请参见下面的代码。

**single.go**

```go
var once sync.Once

type single struct {
}

var singleInstance *single

func getInstance() *single {
    if singleInstance == nil {
        once.Do(
            func() {
                fmt.Println("Creting Single Instance Now")
                singleInstance = &single{}
            })
    } else {
        fmt.Println("Single Instance already created-2")
    }
    return singleInstance
}
```

**输出：**

```go
Creting Single Instance Now
Single Instance already created-2
Single Instance already created-2
```

+   有一个输出为**“现在创建单实例”**，这意味着只有一个goroutine能够创建单实例。

+   有几个输出为**“单实例已创建-2”**，这意味着在到达时单实例已经被创建，他们无法绕过第一次if检查。

+   [单例](https://golangbyexample.com/tag/singleton/) *   [Golang中的单例设计模式](https://golangbyexample.com/tag/singleton-design-pattern-in-golang/)
