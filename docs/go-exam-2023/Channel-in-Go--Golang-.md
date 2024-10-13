<!--yml

类别：未分类

日期：2024-10-13 06:24:19

-->

# Go语言中的通道（Channel）

> 来源：[https://golangbyexample.com/channel-golang/](https://golangbyexample.com/channel-golang/)

这是Go语言综合教程系列的第24章。有关该系列其他章节，请参考此链接 – [Go语言综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [选择语句](https://golangbyexample.com/select-statement-golang/)

**上一个教程** – [协程](https://golangbyexample.com/goroutines-golang/)

现在让我们来看一下当前教程。以下是当前教程的目录

目录

**   [概述](#Overview "Overview")

+   [声明通道](#Declaring_Channels "Declaring Channels")

+   [通道上的操作](#Operations_on_Channel "Operations on Channel")

    +   [发送操作](#Send_Operation "Send Operation")

    +   [接收操作](#Receive_Operation "Receive Operation")

+   [通道方向](#Channel_Direction "Channel Direction")

    +   [仅发送通道](#Only_Send_Channel "Only Send Channel")

    +   [仅接收通道](#Only_Receive_Channel "Only Receive Channel")

+   [使用cap()函数获取通道的容量](#Capacity_of_a_channel_using_cap_function "Capacity of a channel using cap() function")

+   [使用len()函数获取通道的长度](#Length_of_a_channel_using_len_function "Length of a channel using len() function")

+   [关闭通道的操作](#Close_operation_on_a_channel "Close operation on a channel")

+   [在通道上的范围循环](#For_range_loop_on_a_channel "For range loop on a channel")

+   [空通道](#Nil_channel "Nil channel")

+   [摘要表](#Summary_table "Summary table")

+   [结论](#Conclusion "Conclusion")*  *# **概述**

通道是Go中的一种数据类型，提供了协程之间的同步和通信。可以将其视为管道，供协程之间进行通信。协程之间的这种通信不需要任何显式锁定，锁由通道自身内部管理。通道与协程一起使Go编程语言具有并发性。因此我们可以说，Go语言有两个并发原语：

+   **协程** – 轻量级独立执行以实现并发/并行。

+   **通道** – 提供协程之间的同步和通信。

# **声明通道**

每个通道变量只能容纳特定类型的数据。Go在声明通道时使用特殊关键字**chan**。下面是声明通道的格式

```
var variable_name chan type
```

这仅仅声明了一个可以容纳**<type>**类型数据的通道，并且默认值为nil，因此创建了一个空通道。让我们看一个程序来确认这一点。

```
package main

import "fmt"

func main() {
    var a chan int
    fmt.Println(a)
}
```

**输出**

```
{nil}
```

为了定义通道，我们可以使用内置函数**make.**

```
package main

import "fmt"

func main() {
    var a chan int
    a = make(chan int)
    fmt.Println(a)
}
```

**输出**

```
0xc0000240c0
```

在你的机器上，输出的地址可能会不同。

那么这里的make是干什么的呢？通道在内部由**hchan**结构表示，其主要元素是：

```
type hchan struct {
    qcount   uint           // total data in the queue
    dataqsiz uint           // size of the circular queue
    buf      unsafe.Pointer // points to an array of dataqsiz elements
    elemsize uint16
    closed   uint32         // denotes weather channel is closed or not
    elemtype *_type         // element type
    sendx    uint           // send index
    recvx    uint           // receive index
    recvq    waitq          // list of recv waiters
    sendq    waitq          // list of send waiters
    lock     mutex
}
```

使用**make**时，会创建一个**hchan**结构的实例，所有字段都初始化为其默认值。

# **通道上的操作**

通道上可以执行两个主要操作。

+   发送

+   接收

让我们逐一查看每个部分。

## **发送操作**

发送操作用于将数据发送到通道。以下是向通道发送的格式。

```
ch <- val
```

在这里

+   **ch**是通道变量

+   **val**是发送到通道的数据。

请注意**val**的数据类型和通道的数据类型应该匹配。

## **接收操作**

接收操作用于从通道读取数据。以下是从通道接收的格式。

```
val := <- ch 
```

在这里

+   **ch**是通道变量

+   **val**是一个变量，用于存储从通道读取的数据。

让我们看一个示例，在这个示例中，我们将在一个goroutine中发送数据，并在另一个goroutine中接收该数据。

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)

    fmt.Println("Sending value to channel")
    go send(ch)

    fmt.Println("Receiving from channel")
    go receive(ch)

    time.Sleep(time.Second * 1)
}

func send(ch chan int) {
    ch <- 1
}

func receive(ch chan int) {
    val := <-ch
    fmt.Printf("Value Received=%d in receive function\n", val)
}
```

**输出**

```
Sending value to channel
Receiving from channel
Value Received=1 in receive function
```

在上面的程序中，我们创建了一个名为**ch**的通道，其数据类型为**int**，这意味着它只能传输类型为**int**的数据。函数**send()**和**receive()**作为一个goroutine启动。我们在send()的goroutine中向通道**ch**发送数据，并在receive()的goroutine中从**ch**接收数据。

关于接收操作要注意一个非常重要的点是，发送到通道的特定值在任何goroutine中只能接收一次。正如你所见，在发送和接收通道时，goroutine中没有使用锁。锁是由通道内部管理的，代码中不需要使用显式锁。

默认情况下，当我们使用make创建通道时，它会创建一个无缓冲的通道，这意味着创建的通道不能存储任何数据。因此，任何对通道的发送在另一个goroutine接收之前都是阻塞的。因此在**send()**函数中，这一行将会被阻塞。

```
ch <- 1
```

直到在**receive()**函数中接收到值。

```
val := <-ch
```

我们还在主函数中设置了一个超时，以允许发送和接收函数都完成。如果在主函数结束时没有超时，程序将退出，两个goroutine可能无法调度。

为了说明发送时的阻塞，让我们在**send()**函数中向通道**ch**发送值后添加一个日志，并在从**ch**接收值之前在**receive()**函数中设置一个超时。

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int)
	go send(ch)
	go receive(ch)
	time.Sleep(time.Second * 2)
}

func send(ch chan int) {
	ch <- 1
	fmt.Println("Sending value to channel complete")
}

func receive(ch chan int) {
	time.Sleep(time.Second * 1)
	fmt.Println("Timeout finished")
	_ = <-ch
	return
}
```

**输出**

```
Timeout finished
Sending value to channel complete
```

日志

```
Timeout finished
```

将始终位于之前

```
Sending value to channel complete
```

尝试在接收函数中更改不同的超时值。你会发现上述顺序始终存在。这表明，在无缓冲通道上发送时会阻塞，直到在其他goroutine上发生接收。接收通道也是阻塞的，除非有另一个goroutine向该通道发送数据。

为了说明接收时的阻塞，让我们在receive()函数中接收值后添加一个日志，并在send()函数中发送值之前设置一个超时。

```
package main

import (
	"fmt"
	"time"
)

func main() {
  ch := make(chan int)
  go send(ch)

  go receive(ch)
  time.Sleep(time.Second * 2)
}

func send(ch chan int) {
  time.Sleep(time.Second * 1)
  fmt.Println("Timeout finished")
  ch <- 1
}

func receive(ch chan int) {
  val := <-ch
  fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**输出**

```
Timeout finished
Receiving Value from channel finished. Value received: 1
```

在上面的程序中，我们在发送到通道之前添加了一个超时。

日志

```
Timeout finished
```

将始终在之前。

```
Receiving Value from channel finished. Value received: 1
```

尝试在send()函数中更改不同的超时值。你将注意到上述顺序始终存在。这说明在无缓冲通道上的接收会被阻塞，直到其他goroutine在该通道上进行发送。

我们也可以在主函数中接收这个值。

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)
    fmt.Println("Sending value to channel start")
    go send(ch)
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}

func send(ch chan int) {
    ch <- 1
}
```

**输出**

```
Sending value to channel start
Receiving Value from channel finished. Value received: 1
```

目前为止，我们已经看到无缓冲通道的例子。无缓冲通道没有任何存储，因此，对于无缓冲通道。

+   除非有其他goroutine来接收，否则在通道上发送会被阻塞。

+   接收会被阻塞，直到另一侧有其他goroutine发送。

在Go中，你也可以创建一个缓冲通道。缓冲通道有一定的容量来保存数据，因此，对于缓冲通道：

+   只有在缓冲区满时，发送到缓冲通道才会被阻塞。

+   接收仅在通道为空时被阻塞。

这是使用make函数创建缓冲通道的语法。

```
a = make(chan <type>, capacity)</type>
```

第二个参数指定通道的容量。无缓冲通道的容量为零。这就是为什么如果没有接收者，发送会被阻塞；如果没有发送者，接收会被阻塞。

让我们看一个缓冲通道的程序。

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

在上面的程序中，我们创建了一个长度为1的缓冲通道，如此。

```
ch := make(chan int, 1)
```

我们在主goroutine中发送一个值并接收同一个值。这是可能的，因为如果缓冲通道没有满，发送到缓冲通道不会被阻塞。因此，下面的行对于缓冲通道不会阻塞。

```
ch <- 1
```

该通道的容量为一。因此，向通道发送数据不会被阻塞，值会存储在通道的缓冲区中。因此，在同一个goroutine中发送和接收仅对缓冲通道有效。让我们深入探讨上述提到的两个重要点。

+   当通道已满时，发送到通道会被阻塞。

+   当通道为空时，接收会被阻塞。

让我们看看每个程序。

**当通道满时，发送被阻塞**

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**输出**

```
fatal error: all goroutines are asleep - deadlock!
```

在上面的程序中，我们创建了一个容量为一的通道。之后，我们向通道发送一个值，然后再向通道发送另一个值。

```
ch <- 1
ch <- 1
```

发送到通道的第二个请求被阻塞，因为缓冲区已满，因此导致了死锁情况，因为程序无法继续，这就是为什么你会看到输出为。

```
fatal error: all goroutines are asleep - deadlock!
```

**当通道为空时，接收被阻塞**

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 1)
    ch <- 1
    fmt.Println("Sending value to channnel complete")
    val := <-ch
    val = <-ch
    fmt.Printf("Receiving Value from channel finished. Value received: %d\n", val)
}
```

**输出**

```
fatal error: all goroutines are asleep - deadlock!
```

在上面的程序中，我们创建了一个容量为一的通道，之后我们向通道发送一个值，然后从通道接收一个值。接着，我们尝试第二次接收通道中的值，结果导致了死锁，因为程序无法继续，因为通道为空，无法接收。这就是为什么你会看到输出为。

```
fatal error: all goroutines are asleep - deadlock!
```

这说明如果通道缓冲区为空，接收将被阻塞。

# **通道方向**

到目前为止，我们已经看到双向通道，既可以发送又可以接收数据。在golang中，也可以创建单向通道。可以创建一个只能发送数据的通道，也可以创建一个只能接收数据的通道。这由通道的箭头方向决定。

+   只能发送数据的通道。

这是这种通道的语法

```
chan<- int
```

+   只能发送数据的通道

这是这种通道的语法

```
<-chan in
```

现在的问题是，为什么你想创建一个只能发送数据或只能接收数据的通道。这在将通道传递给函数时非常有用，因为我们希望限制函数只能发送数据或接收数据。

有多种方法可以将通道作为函数参数传递。通道的箭头方向指定数据流的方向。

+   **chan**  :双向通道（可读可写）

+   **chan <-**  :仅向通道写入

+   **<- chan**  :仅从通道读取（输入通道）

## **仅发送通道**

+   这种通道的签名只能发送数据，传递给函数作为参数时会如下所示。

```
func process(ch chan<- int){ //doSomething }
```

+   尝试从这样的通道接收数据将出现以下错误。

```
invalid operation: <-ch (receive from send-only type chan<- int)
```

尝试取消注释代码中的以下行以查看上述错误

```
s := <-ch
```

**代码：**

```
package main
import "fmt"
func main() {
    ch := make(chan int, 3)
    process(ch)
    fmt.Println(<-ch)
}
func process(ch chan<- int) {
    ch <- 2
    //s := <-ch
}
```

**输出：** 2

## **仅接收通道**

+   这种通道的签名只能接收数据，传递给函数作为参数时会如下所示。

```
func process(ch <-chan int){ //doSomething }
```

+   尝试向这样的通道发送数据将出现以下错误。

```
invalid operation: ch <- 2 (send to receive-only type <-chan int)
```

尝试取消注释代码中的以下行以查看上述错误

```
ch <- 2
```

**代码：**

```
package main
import "fmt"
func main() {
    ch := make(chan int, 3)
    ch <- 2
    process(ch)
    fmt.Println()
}
func process(ch <-chan int) {
    s := <-ch
    fmt.Println(s)
    //ch <- 2
}
```

**输出：** 2

# **使用cap()函数获取通道容量**

缓冲通道的容量是该通道可以容纳的元素数量。容量指通道的缓冲区大小。创建通道时可以在使用make函数时指定通道的容量。第二个参数是容量。

无缓冲通道的容量始终为零

```
package main

import "fmt"

func main() {
    ch := make(chan int, 3)
    fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**输出**

```
Capacity: 3
```

在上述程序中，我们在make函数中指定了容量为3

```
make(chan int, 3)
```

# **使用len()函数获取通道长度**

内置的**len()**函数可用于获取通道的长度。通道的长度是通道中已存在的元素数量。因此，长度实际上表示通道缓冲区中排队的元素数量。通道的长度始终小于或等于通道的容量。

无缓冲通道的长度始终为零

```
package main

import "fmt"

func main() {
	ch := make(chan int, 3)
	ch <- 5
	fmt.Printf("Len: %d\n", len(ch))

	ch <- 6
	fmt.Printf("Len: %d\n", len(ch))
	ch <- 7
	fmt.Printf("Len: %d\n", len(ch))
}
```

**输出**

```
Len: 1
Len: 2
Len: 3
```

在上述代码中，首先创建了一个容量为3的通道。之后，我们不断向通道发送一些值。正如你从输出中注意到的那样，每次发送操作后，通道的长度增加1，因为长度表示通道缓冲区中的项目数量。

# **通道的关闭操作**

关闭是一个内置函数，可用于关闭通道。关闭通道意味着无法再向通道发送数据。通道通常在所有数据已发送且没有更多数据可发送时关闭。让我们看看一个程序。

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)
    go sum(ch, 3)
    ch <- 2
    ch <- 2
    ch <- 2
    close(ch)
    time.Sleep(time.Second * 1)
}

func sum(ch chan int, len int) {
    sum := 0
    for i := 0; i < len; i++ {
        sum += <-ch
    }
    fmt.Printf("Sum: %d\n", sum)
}
```

**输出**

```
Sum: 6
```

在上面的程序中，我们创建了一个通道。然后我们在一个goroutine中调用**sum**函数。在主函数中，我们向通道发送3个值，之后关闭通道，表示无法再向通道发送值。**sum**函数使用for循环遍历通道并计算总值。

在关闭的通道上发送将导致恐慌。

请看下面的程序。

```
package main
func main() {
    ch := make(chan int)
    close(ch)
    ch <- 2
}
```

**输出**

```
panic: send on closed channel
```

关闭已经关闭的通道也会导致恐慌。

在接收一个通道时，我们也可以使用一个额外的变量来确定通道是否已关闭。下面是相应的语法。

```
val,ok <- ch
```

ok的值将是

+   如果通道未关闭则为真。

+   如果通道已关闭则为假。

```
package main
import (
    "fmt"
)
func main() {
    ch := make(chan int, 1)
    ch <- 2
    val, ok := <-ch
    fmt.Printf("Val: %d OK: %t\n", val, ok)

    close(ch)
    val, ok = <-ch
    fmt.Printf("Val: %d OK: %t\n", val, ok)
}
```

**输出**

```
Val: 2 OK: true
Val: 0 OK: false
```

在上面的程序中创建了一个容量为1的通道。然后我们向通道发送了一个值。第一次接收中的**ok**变量为真，因为通道未关闭。第二次接收中的ok变量为假，因为通道已关闭。

# **通道上的范围循环**

可以使用范围循环从通道接收数据，直到其关闭。

```
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 3)
	ch <- 2
	ch <- 2
	ch <- 2
	close(ch)
	go sum(ch)
	time.Sleep(time.Second * 1)
}

func sum(ch chan int) {
	sum := 0
	for val := range ch {
		sum += val
	}
	fmt.Printf("Sum: %d\n", sum)
}
```

**输出**

```
Sum: 6
```

在上面的程序中，我们创建了一个通道。在主函数中向通道发送了三个值，之后关闭了通道。然后我们调用求和函数，并将通道传递给该函数。在求和函数中，我们对通道进行了范围循环。在遍历完通道中的所有值后，范围循环将退出，因为通道已关闭。

现在脑海中浮现的问题是，如果在主函数中不关闭通道，会发生什么。尝试注释掉关闭通道的那一行。然后运行程序。它也会输出死锁，因为范围循环在求和函数中将永远不会完成。

```
fatal error: all goroutines are asleep - deadlock!
```

# **零通道**

通道的零值为零。因此，仅声明一个通道会创建一个默认的零通道，因为通道的零值为零。让我们看看一个演示程序。

```
package main

import "fmt"

func main() {
    var a chan int
    fmt.Print("Default zero value of channel: ")
    fmt.Println(a)
}
```

**输出**

```
nil
```

关于零通道需要注意的一些要点。

+   向零通道发送将永远阻塞。

+   从零通道接收将永远阻塞。

+   关闭零通道会导致恐慌。

# **摘要表**

到目前为止，我们已经看到了5个关于通道的操作。

+   发送

+   接收

+   关闭

+   长度

+   容量

让我们看看一个摘要表，展示不同类型通道上每个操作的结果。

| **命令** | **无缓冲通道**（未关闭且非零） | **缓冲通道**（未关闭且非零） | **已关闭通道** | **零通道** |
| --- | --- | --- | --- | --- |
| 发送 | 如果没有对应接收者则阻塞，否则成功 | 如果通道已满则阻塞，否则成功 | 恐慌 | 永远阻塞 |
| 接收 | 如果没有相应的发送者则阻塞，否则成功 | 如果通道为空则阻塞，否则成功 | 如果通道为空则接收数据类型的默认值，否则接收实际值 | 永远阻塞 |
| 关闭 | 成功 | 成功 | 恐慌 | 恐慌 |
| 长度 | 0 | 通道缓冲区中排队的元素数量 | -0如果是无缓冲通道-如果是有缓冲通道则为缓冲区中排队的元素数量 | 0 |
| 容量 | 0 | 通道缓冲区的大小 | -0如果是无缓冲通道-如果是有缓冲通道则为缓冲区的大小 | 0 |

# **结论**

这就是关于golang中通道的所有内容。希望你喜欢这篇文章。请在评论中分享反馈/改进/错误。

**下一篇教程** – [选择语句](https://golangbyexample.com/select-statement-golang/)

**上一篇教程** – [协程](https://golangbyexample.com/goroutines-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
