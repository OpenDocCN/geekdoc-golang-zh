<!--yml

类别：未分类

日期：2024-10-13 06:23:48

-->

# Go 中的 Select 与 Switch (Golang)

> 来源：[`golangbyexample.com/select-versus-switch-in-golang/`](https://golangbyexample.com/select-versus-switch-in-golang/)

目录

**概述**

+   开关示例

+   选择示例 *  * # **概述**

以下是 **switch** 和 **select** 语句之间的一些差异。

+   在 switch 中，每个 case 语句都是一个表达式，而在 select 中，每个 case 语句都是通道上的发送或接收操作。

+   **开关的格式**

```go
switch statement; expression {
case expression1:
     //Dosomething
case expression2:
     //Dosomething
default:
     //Dosomething
}
```

**选择**的格式

```go
select {
case channel_send_or_receive:
     //Dosomething
case channel_send_or_receive:
     //Dosomething
default:
     //Dosomething
}
```

这就是 switch 的工作方式。给定一个**switch 表达式**，它会遍历所有 case，尝试找到第一个匹配的**case 表达式**，否则执行默认 case（如果存在）。匹配的顺序是从上到下。而在 select 语句中，它选择发送或接收操作在通道上未被阻塞并且准备执行的 case。如果多个 case 准备执行，则随机选择一个。

+   Switch 语句是非阻塞的。它选择匹配的 case，否则执行默认 case。即使默认块不存在且没有任何 case 匹配，switch 块也会结束，程序继续。Select 语句可以阻塞，因为它与通道一起使用，而通道在发送或接收操作时可能会阻塞。如果在所有 case 语句中发送和接收操作都被阻塞，并且默认块不存在，则 select 语句将阻塞。默认块使 select 非阻塞，因为如果所有其他 case 都被阻塞，则将执行默认 case。

+   Switch 将按顺序选择匹配的 case，因此 switch 是确定性的。你可以通过查看 switch 语句和表达式，知道哪个 case 将匹配。Select 将随机执行一个 case，没有顺序，因此 select 是非确定性的。它会随机选择一个准备好的 case 语句来执行。

+   Switch 允许使用 fallthrough 关键字选择多个匹配的 case，而 select 不允许使用 fallthrough 关键字选择多个 case。将随机选择准备好的 case 中的一个。

+   Switch 有两种形式：表达式开关和类型开关，而 select 只有一种形式。

请参考全面的教程

+   Switch – [`golangbyexample.com/switch-statement-golang/`](https://golangbyexample.com/switch-statement-golang/)

+   Select – [`golangbyexample.com/select-statement-golang/`](https://golangbyexample.com/select-statement-golang/)

# **开关示例**

```go
package main

import "fmt"

func main() {
    switch ch := "b"; ch {
    case "a":
        fmt.Println("a")
    case "b":
        fmt.Println("b")    
    default:
        fmt.Println("No matching character")    
    }

    //fmt.Println(ch)

} 
```

**输出：**

```go
b
```

在上面的示例中，switch case 按顺序进行，并匹配此处的 switch 表达式“b”。

# **选择示例**

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go goOne(ch1)
    go goTwo(ch2)

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    }
}
func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}
func goTwo(ch chan string) {
    ch <- "From goTwo goroutine"
}
```

**输出**

```go
From goOne goroutine
```

在上述程序中，我们创建了两个通道，并将它们传递给两个不同的 goroutine。然后，每个 goroutine 向通道发送一个值。在 select 中，我们有两个 case 语句。两个 case 语句都在等待某个通道的接收操作完成。一旦任何通道的接收操作完成，它就会执行并退出 select。因此，从输出可以看出，在上述程序中，它打印了从某个通道接收到的值并退出。

因此，在上述程序中，由于哪个发送操作会先完成是不可确定的，这就是为什么你在不同时间运行程序时会看到不同输出的原因。

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
