<!--yml

类别：未分类

日期：2024-10-13 06:05:13

-->

# Golang中通道的内部工作原理

> 来源：[https://golangbyexample.com/inner-working-of-channels-in-golang/](https://golangbyexample.com/inner-working-of-channels-in-golang/)

# **介绍**

本文的目的是让人们了解通道的内部工作原理。Golang有两种并发原语：

1.  **协程** – 轻量级独立执行以实现并发/并行。

1.  **通道** – 提供协程之间的同步和通信。

通道是协程安全的，并以FIFO方式管理协程之间的通信。协程可以在通道上阻塞，进行数据的发送或接收，唤醒被阻塞的协程是通道的责任。

# **通道类型**

## **缓冲通道**

+   仅在缓冲区已满时，发送到缓冲通道才会阻塞。

+   如果通道为空，接收将被阻塞。

## **未缓冲通道**

+   在通道上发送将被阻塞，除非有另一个协程来接收。

+   接收将被阻塞，直到另一协程在另一侧发送数据。

# **HCHAN结构**

让我们了解创建通道时内部发生的事情。通道在内部由**hchan**结构表示，其主要元素为：

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

type waitq struct {
   first *sudog
   last  *sudog
}
```

结构体**sudog**的主要元素如下所示：

```
type sudog struct {
   g     *g             //goroutine
   elem  unsafe.Pointer // data element 
   ...
}
```

让我们深入了解通道发送和接收时发生的事情

# **在通道上发送**

1.  **没有接收者/接收者等待**：未缓冲通道或在缓冲通道的情况下缓冲区已满。

1.  **接收器/接收者等待**：未缓冲通道或在缓冲通道的情况下缓冲区为空

1.  **缓冲区为空**：在缓冲通道的情况下

1.  **通道已关闭**

## **1. 没有接收者/接收者等待：**

在以下两种情况下，当没有接收者等待时，行为将是相同的。

+   缓冲通道：缓冲区已满

+   未缓冲通道

尝试向通道发送的协程**G1**，其执行被暂停，仅在接收后恢复。让我们看看这如何发生。

+   它创建一个**sudog**对象，**g**即协程本身，**elem**指向它想放入缓冲区的数据。

+   然后将该sudog结构添加到发送队列sendq中。

+   协程调用“GOPARK”到Go运行时。作为响应，Go运行时将该G1的状态更改为等待。

## **2. 接收者等待**

在以下两种情况下，当有接收者等待时，行为将是相同的

+   缓冲通道：缓冲区为空

+   未缓冲通道

让我们看看这如何发生。

+   协程G1从**receq**中出队，然后将数据直接传递给接收协程。

+   将接收协程的状态设置为**可运行**

## **3. 缓冲区未满：**

仅适用于缓冲通道：缓冲区至少有一个空位

+   将数据写入缓冲区

## **4. 通道已关闭：**

+   恐慌

# **从通道接收**

1.  **没有发送者/发送者等待**：未缓冲通道或在缓冲通道的情况下缓冲区为空

1.  **发送者等待：** 未缓冲通道或缓冲通道的缓冲区为空。

1.  **非空缓冲区：** 在缓冲通道的情况下，通道至少有1个项目。

1.  **通道关闭**

## **1\. 没有发送者等待：**

对于下面两种情况，当没有接收者等待时，行为将是相同的。

+   缓冲通道：缓冲区为空

+   无缓冲通道

尝试接收的goroutine G1，其执行被暂停，仅在发送后恢复。让我们看看这是如何发生的。

+   该goroutine创建一个**sudog**对象，自己作为goroutine，元素为空

+   然后将该**sudog**结构添加到等待发送队列recvq中**。**

+   该goroutine调用“GOPARK”到Go运行时。作为回应，Go运行时将该goroutine的状态更改为**等待**。

## **2\. 发送者/发送者等待：**

+   从缓冲区出队列元素并复制到自身

+   从**sendq**出队列，然后将数据直接复制到缓冲区。

+   将发送goroutine的状态设置为**可运行**

## **3\. 非空缓冲区：**

仅适用于缓冲通道：

+   该goroutine从缓冲区读取数据

## **4\. 通道关闭：**

+   从通道接收数据类型的默认值

+   [sidetoc](https://golangbyexample.com/tag/sidetoc/)
