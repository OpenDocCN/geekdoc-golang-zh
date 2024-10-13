<!--yml

类别：未分类。

日期：2024-10-13 06:24:41。

-->

# Go（Golang）中通道的所有操作/函数。

> 来源：[https://golangbyexample.com/all-operations-channel-golang/](https://golangbyexample.com/all-operations-channel-golang/)

目录

**   [概述](#Overview "概述")

+   [发送操作](#Send_Operation "发送操作")

+   [接收操作](#Receive_Operation "接收操作")

+   [通道的关闭操作](#Close_operation_on_a_channel "通道的关闭操作")

+   [使用 len() 函数的通道长度](#Length_of_a_channel_using_len_function "使用 len() 函数的通道长度")

+   [使用 cap() 函数的通道容量](#Capacity_of_a_channel_using_cap_function "使用 cap() 函数的通道容量")*  *# **概述**

下面的操作/函数适用于 Golang 中的通道。

+   将数据发送到通道。

+   从通道接收数据。

+   关闭通道。

+   使用 len() 函数的通道长度。

+   使用 cap() 函数的通道容量。

让我们逐一看一下通道上的每个操作/函数。

# **发送操作**

发送操作用于将数据发送到通道。以下是发送到通道的格式。

```
ch <- data
```

在哪里

+   ch 是通道变量。

+   数据是发送到通道的内容。

请注意，数据类型和通道类型应匹配。

发送操作对于缓冲或无缓冲通道可以以下列方式阻塞。

+   **缓冲通道 -** 仅在缓冲区满时，发送到缓冲通道才会阻塞。

+   **无缓冲通道** - 发送到通道将阻塞，除非有另一个 goroutine 来接收。

在我们理解接收操作后，让我们看看发送操作的程序。

# **接收操作**

接收操作用于从通道读取数据。以下是从通道接收的格式。

```
data := <- ch 
```

在哪里

+   ch 是通道变量。

+   data 是一个变量，用于存储从通道读取的数据。

让我们看一个示例，其中我们将从一个 goroutine 发送数据，并在另一个 goroutine 中接收这些数据。

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch := make(chan int)

    fmt.Println("Sending value to channnel")
    go send(ch)

    fmt.Println("Receiving from channnel")
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

在上面的程序中，我们创建了一个只能传输 int 类型数据的通道。函数 **send()** 和 **receive()** 作为 goroutine 启动。我们在 send() goroutine 中向通道发送数据，并在 receive() goroutine 中接收数据。

接收操作对于缓冲或无缓冲通道可以以下列方式阻塞。

+   **缓冲通道 -** 仅在通道为空时接收才会阻塞。

+   **无缓冲通道 -** 接收会阻塞，直到另一侧有另一个 goroutine 发送。

# **通道的关闭操作**

Close 是一个内置函数，可用于关闭通道。关闭通道意味着不能再向通道发送数据。当所有数据发送完毕且没有更多数据可发送时，通道通常会关闭。让我们看看一个程序。

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

在上面的程序中，我们创建了一个通道。然后我们在一个 goroutine 中调用了**sum**函数。在主函数中，我们向通道发送了3个值，之后关闭了通道，表示不能再向通道发送更多的值。**sum**函数使用for循环迭代通道并计算总和。

在关闭的通道上发送将导致恐慌。请参见下面的程序。

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

关闭一个已关闭的通道将导致恐慌

在从通道接收时，我们还可以使用一个额外的变量来确定通道是否已关闭。以下是相应的语法。

```
val,ok <- ch
```

ok的值将是

+   如果通道未关闭则为真

+   每个通道都已关闭为假

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

在上面的程序中创建了一个容量为1的通道。然后我们向通道发送了一个值。在第一次接收中，**ok**变量为true，因为通道未关闭。在第二次接收中，ok变量为false，因为通道已关闭。

# **使用len()函数获取通道的长度**

内置的**len()**函数可用于获取通道的长度。通道的长度是通道中已有元素的数量。因此，长度实际上表示通道缓冲区中排队的元素数量。通道的长度始终小于或等于通道的容量。

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

在上面的代码中，首先创建了一个容量为3的通道。之后，我们持续向通道发送一些值。从你的输出中可以注意到，在每次发送操作后，通道的长度增加了1，因为长度表示通道缓冲区中的项目数量。

# **使用cap()函数获取通道的容量**

缓冲通道的容量是通道可以容纳的元素数量。容量指的是通道缓冲区的大小。通道的容量可以在创建通道时通过使用make函数指定。第二个参数是容量。

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

在上面的程序中，我们在make函数中指定了容量为3。

```
make(chan int, 3)
```

让我们来看一下总结表，展示了对不同类型通道的每个操作结果

| **命令** | **无缓冲通道**（未关闭且不为nil） | **缓冲通道**（未关闭且不为nil） | **已关闭通道** | **空通道** |
| --- | --- | --- | --- | --- |
| 发送 | 如果没有相应的接收者则阻塞，否则成功 | 如果通道已满则阻塞，否则成功 | 恐慌 | 永久阻塞 |
| 接收 | 如果没有相应的发送者则阻塞，否则成功 | 如果通道为空则阻塞，否则成功 | 如果通道为空则接收数据类型的默认值，否则接收实际值 | 永久阻塞 |
| 关闭 | 成功 | 成功 | 恐慌 | 恐慌 |
| 长度 | 0 | 在通道缓冲区中排队的元素数量 | - 如果是无缓冲通道则为0 - 如果是有缓冲通道则为缓冲区中的排队元素数量 | 0 |
| 容量 | 0 | 通道缓冲区的大小 | - 如果是无缓冲通道则为0 - 如果是有缓冲通道则为缓冲区大小 | 0 |

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
