<!--yml

分类：未分类

日期：2024-10-13 06:23:23

-->

# Go中的选择语句（Golang）

> 来源：[https://golangbyexample.com/select-statement-golang/](https://golangbyexample.com/select-statement-golang/)

这是Golang综合教程系列的第25章。有关该系列其他章节，请参考此链接 – [Golang综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一教程** – [错误](https://golangbyexample.com/error-in-golang/)

**上一教程** – [通道](https://golangbyexample.com/channel-golang/)

现在让我们查看当前的教程。以下是当前教程的目录。

目录

**   [概述](#Overview "Overview")

+   [选择语句的使用](#Use_of_select_statement "Use of  select statement")

+   [带发送操作的选择示例](#Select_with_send_operation_example "Select with send operation example")

+   [带默认情况的选择](#Select_with_default_case "Select with default case")

+   [带阻塞超时的选择](#Select_with_blocking_timeout "Select with blocking timeout")

+   [空选择](#Empty_select "Empty select")

+   [带无限for循环的选择语句](#Select_statement_with_an_infinite_for_loop_outside "Select statement with an infinite for loop outside")

+   [带nil通道的选择语句](#Select_statement_with_a_nil_channel "Select statement with a nil channel")

+   [选择中的中断关键字](#Break_keyword_in_Select "Break keyword in Select")

+   [结论](#Conclusion "Conclusion")*  *# **概述**

选择类似于switch语句，区别在于在选择中，每个案例语句等待来自通道的发送或接收操作。选择语句将等待，直到任何一个案例语句上的发送或接收操作完成。它与switch语句的不同之处在于，每个案例语句要么在通道上发送或接收操作，而在switch中，每个案例语句是一个表达式。因此，选择语句允许你等待来自不同通道的多个发送和接收操作。需要注意的关于选择语句的两个重要点是

+   选择会阻塞，直到任何一个案例语句准备就绪。

+   如果多个案例语句都准备就绪，则随机选择一个并继续。

以下是**选择**的格式

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

选择选择未被阻塞且准备执行的通道上的发送或接收操作的案例。如果多个案例准备执行，则随机选择一个。

让我们看一个简单的例子。我们将在本教程中稍后学习默认情况。

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

在上述程序中，我们创建了两个通道，分别传递给两个不同的 goroutine。然后每个 goroutine 向通道发送一个值。在选择中，我们有两个 case 语句。每个 case 语句都在等待对其中一个通道的接收操作完成。一旦任何通道的接收操作完成，就会执行相应的语句，**选择**退出。因此，从输出中可以看到，以上程序打印了从某个通道接收到的值并退出。

在上述程序中，由于无法确定哪个发送操作会更早完成，因此如果多次运行程序，您会看到不同的输出。让我们看另一个程序，其中在向 **ch2** 通道发送值之前，我们会在 goroutine goTwo 中设置超时。这将确保对 **ch1** 的发送操作会比对 **ch2** 的发送操作先执行。

```go
package main

import (
    "fmt"
    "time"
)

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
    time.Sleep(time.Second * 1)
    ch <- "From goTwo goroutine"
}
```

**输出**

```go
From goOne goroutine
```

在上述程序中，选择语句对 **ch1** 的接收操作较早完成，因此选择将始终执行该 case 语句，这也从输出中可以明显看出。

通过在选择语句中使用循环，也可以等待两个通道上的接收操作完成。我们来看一个程序示例。

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go goOne(ch1)
    go goTwo(ch2)
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        }
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
From goTwo goroutine
```

在上述程序中，我们在选择语句中设置了长度为二的循环。因此，选择语句执行了两次，并打印了每个 case 语句接收到的值。

我们之前提到，如果任何一个 case 语句没有准备好，选择可能会阻塞。让我们看一个示例。

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    }
}
```

**输出**

```go
fatal error: all goroutines are asleep - deadlock!
```

在上述程序中，我们创建了一个名为 **ch1** 的通道。然后我们在选择语句中从这个通道接收。由于没有 goroutine 向该通道发送数据，因此会导致死锁，选择语句无限期阻塞。这就是它输出以下内容的原因。

```go
fatal error: all goroutines are asleep - deadlock!
```

# **选择语句的使用**

当有多个 goroutine 同时向多个通道发送数据时，选择语句非常有用。选择语句可以同时从任何一个 goroutine 接收数据，并执行已准备好的语句。因此，选择结合通道和 goroutine 成为管理同步和并发的强大工具。

# **带发送操作的选择示例**

到目前为止，我们已经看到了选择案例语句中接收操作的示例。现在我们来看一个发送操作的示例。

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
    case ch2 <- "To goTwo goroutine":
    }
}

func goOne(ch chan string) {
    ch <- "From goOne goroutine"
}

func goTwo(ch chan string) {
    msg := <-ch
    fmt.Println(msg)
}
```

**输出**

```go
To goTwo goroutine
```

# **带默认案例的选择**

类似于switch，选择也可以有一个默认案例。这个默认案例将在没有任何发送或接收操作准备就绪时执行。因此，从某种意义上说，默认语句防止选择永远阻塞。因此，非常重要的一点是，默认语句使选择变为非阻塞。如果选择语句不包含默认案例，则可能会永远阻塞，直到某个案例语句上的发送或接收操作准备就绪。让我们看一个例子以完全理解它。

```go
package main

import "fmt"

func main() {
    ch1 := make(chan string)
    select {
    case msg := <-ch1:
        fmt.Println(msg)
    default:
        fmt.Println("Default statement executed")
    }
}
```

**输出**

```go
Default statement executed
```

在上面的程序中，有一个选择语句正在等待在**ch1**上的接收操作和一个默认语句。由于没有goroutine向通道**ch1**发送数据，因此执行了默认案例，选择退出。如果没有默认案例，选择将会阻塞。

# **带有阻塞超时的选择语句**

在选择中实现阻塞超时可以通过使用**time**包的**After()**函数来完成。下面是**After()**函数的签名。

```go
func After(d Duration) <-chan Time
```

**After**函数等待d持续时间完成，然后在一个通道上返回当前时间。

[https://golang.org/pkg/time/#Time.After](https://golang.org/pkg/time/#Time.After)

让我们看看一个带有超时的选择程序。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan string)
	go goOne(ch1)

	select {
	case msg := <-ch1:
		fmt.Println(msg)
	case <-time.After(time.Second * 1):
		fmt.Println("Timeout")
	}
}

func goOne(ch chan string) {
	time.Sleep(time.Second * 2)
	ch <- "From goOne goroutine"
}
```

**输出**

```go
Timeout
```

在上述选择语句中，我们在等待在**ch1**上的接收操作完成。在其他案例语句中，我们有**time.After**，持续时间为1秒。因此，本质上这个选择语句将至少等待1秒以完成**ch1**上的接收操作，之后**time.After**案例语句将被执行。我们在**goOne**函数中设置了超过1秒的超时，因此我们看到了**time.After**语句被执行。

```go
Timeout
```

被打印为输出。

# **空选择**

没有任何案例语句的选择块是空选择。空选择将永远阻塞，因为没有案例语句可以执行。这也是goroutine无限期等待的一种方式。但如果这个空选择放在主goroutine中，则会导致死锁。让我们来看一个程序。

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

在上面的程序中，我们有一个空的选择语句，因此导致了死锁，这就是你看到如下输出的原因。

```go
fatal error: all goroutines are asleep - deadlock!
```

# **外部无限循环的选择语句**

我们可以在选择语句外部有一个无限循环。这将导致选择语句无限次执行。因此，当使用在选择语句外部的无限循环的for语句时，我们需要有一种方式来跳出for循环。在选择语句外部使用无限循环的一个用例可能是你在等待多个操作在同一通道上接收一段时间。请看下面的例子。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	news := make(chan string)
	go newsFeed(news)

	printAllNews(news)
}

func printAllNews(news chan string) {
	for {
		select {
		case n := <-news:
			fmt.Println(n)
		case <-time.After(time.Second * 1):
			fmt.Println("Timeout: News feed finished")
			return
		}
	}
}

func newsFeed(ch chan string) {
	for i := 0; i < 2; i++ {
		time.Sleep(time.Millisecond * 400)
		ch <- fmt.Sprintf("News: %d", i+1)
	}
}
```

**输出**

```go
News: 1
News: 2
Timeout: News feed finished
```

在上述程序中，我们创建了一个名为 **news** 的通道，用于存储字符串类型的数据。然后，我们将此通道传递给 **newsfeed** 函数，该函数将新闻推送到此通道。在选择语句中，我们从 **news** 通道接收新闻。这条选择语句位于无限循环中，因此选择语句将被多次执行，直到我们退出循环。我们还使用了 **time.After**，其持续时间为1秒，作为其中一个案例语句。因此，这个设置将在1秒内接收所有来自 **news** 通道的新闻，然后退出。

# **选择语句与空通道**

对空通道的发送或接收操作会永远阻塞。因此，在选择语句中使用空通道的用例是，在该案例语句上的发送或接收操作完成后禁用该案例语句。然后可以将通道简单设置为nil。该案例语句在再次执行选择语句时将被忽略，接收或发送操作将等待另一个案例语句。因此，目的是忽略该案例语句并执行其他案例语句。

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    news := make(chan string)
    go newsFeed(news)
    printAllNews(news)
}

func printAllNews(news chan string) {
    for {
        select {
        case n := <-news:
            fmt.Println(n)
            news = nil
        case <-time.After(time.Second * 1):
            fmt.Println("Timeout: News feed finished")
            return
        }
    }
}

func newsFeed(ch chan string) {
    for i := 0; i < 2; i++ {
        time.Sleep(time.Millisecond * 400)
        ch <- fmt.Sprintf("News: %d", i+1)
    }
}
```

**输出**

```go
News: 1
Timeout: News feed finished
```

上述程序与我们研究的程序非常相似，都是在无限循环中包含选择语句。唯一的变化是，在接收到第一条新闻后，我们通过将新闻通道设置为nil来禁用案例语句。

```go
case n := <-news:
   fmt.Println(n)
   news = nil
```

因此，我们只接收到第一条新闻，之后它将超时。这是选择语句中空通道的用例。

# **选择中的 Break 关键字**

下面是 **break** 关键字的示例。

```go
import "fmt"

func main() {
	ch := make(chan string, 1)
	ch <- "Before break"

	select {
	case msg := <-ch:
		fmt.Println(msg)
		break
		fmt.Println("After break")
	default:
		fmt.Println("Default case")
	}
}
```

**输出**

```go
Before break
```

**break** 语句将终止最内层语句的执行，下面的行将永远不会被执行

```go
fmt.Println("After break")
```

# **结论**

这就是golang中选择语句的全部内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一教程** – [错误](https://golangbyexample.com/error-in-golang/)

**上一教程** – [通道](https://golangbyexample.com/channel-golang/)

+   [go](https://golangbyexample.com/tag/go/) *   [golang](https://golangbyexample.com/tag/golang/)*
