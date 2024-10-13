<!--yml

分类：未分类

日期：2024-10-13 06:24:08

-->

# Go（Golang）中的空选择或没有案例的选择

> 来源：[https://golangbyexample.com/empty-select-golang/](https://golangbyexample.com/empty-select-golang/)

目录

**   [概述](#Overview "Overview")

+   [代码](#Code "Code")*  *# **概述**

没有任何案例语句的选择块称为空选择。空选择将永远阻塞，因为没有案例语句可以执行。我们知道选择语句会被阻塞，直到任何案例语句可以被执行。这也是一个goroutine无限期等待的方式。但如果这个空选择放在主goroutine中，它将导致死锁。

让我们来看一个程序

# **代码**

```go
package main

func main() {
    select {}
}
```

**输出**

```go
fatal error: all goroutines are asleep - deadlock!
```

在上面的程序中，我们有一个空的选择语句，因此它导致了死锁，这就是你看到输出的原因。

```go
fatal error: all goroutines are asleep - deadlock!
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
