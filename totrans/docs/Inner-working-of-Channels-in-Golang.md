<!--yml
category: 未分类
date: 2024-10-13 06:05:13
-->

# Inner working of Channels in Golang

> 来源：[https://golangbyexample.com/inner-working-of-channels-in-golang/](https://golangbyexample.com/inner-working-of-channels-in-golang/)

# **Introduction**

The purpose of this article is to give an idea of the inner working of channels. Golang has two  concurrency primitives:

1.  **Goroutine** – lightweight independent execution to achieve concurrency/parallelism.
2.  **Channels** – provides synchronization and communication between goroutines.

Channels are goroutine safe and manage communication between goroutine in a FIFO way. A goroutine can block on a channel while doing send or receive of some data and it is the responsibility of the channel to wake up blocked goroutine

# **Types of Channels**

## **Buffered Channel**

*   Send on a buffer channel only blocks if the buffer is full.
*   Receive is blocked if the channel is empty.

## **Unbuffered Channel**

*   Send on a channel is block unless there is another goroutine to receive.
*   Receive is block until there is another goroutine on the other side to send.

# **HCHAN struct**

Let’s understand what happens internally when you make channels. A channel is internally represented by a **hchan** struct whose main elements are:

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

The struct **sudog** main elements are represented as below:

```
type sudog struct {
   g     *g             //goroutine
   elem  unsafe.Pointer // data element 
   ...
}
```

Let’s understand what happens during send and receive of a channel

# **Send on a channel**

1.  **No receiver/receivers waitin**g: Unbuffered Channel or the Buffer is Full in case of Buffered Channel.
2.  **Receiver/receivers waiting**: Unbuffered Channel or the Buffer is empty in case of Buffered Channel
3.  **Buffer empty**: In case of buffered channel
4.  **Channel closed**

## **1\. No Receiver/Receivers Waiting:**

For below two scenarios the behavior will be the same when there are no receivers waiting.

*   Buffered Channel: Buffer is Full
*   Unbuffered Channel

The goroutine **G1** which is trying to send to the channel, its execution is paused and is resumed only after a receive. Let’s see how that happens.

*   It creates a **sudog** object with **g** i.e, goroutine as itself and **elem** pointing to the data it wants to put into the buffer
*   It then adds that sudog struct to the sending queue sendq .
*   The goroutine calls “GOPARK” to the Go RunTime. In response, the Go RunTime changes the status of that G1 to WAITING.

## **2\. Receivers waiting**

For below two scenarios the behavior will be the same when there are receivers waiting

*   Buffered Channel: Buffer is Empty
*   Unbuffered Channel

Let’s see how that happens.

*   The goroutine G1 dequeues from the **receq** and then pass the data directly to the receiver goroutine.
*   Set the status of the receiver goroutine to **RUNNABLE**

## **3\. Buffer not full:**

Only applicable for buffered channels: Buffer has at least one empty space

*   Writes the data to the buffer

## **4\. Channel closed:**

*   Panics

# **Receive From a channel**

1.  **No sender/senders waiting:** Unbuffered Channel or buffer is empty in case of Buffered Channel
2.  **Sender/senders waiting:** Unbuffered Channel or the Buffer is empty in case of Buffered Channel.
3.  **Non-empty Buffer:** In case of buffered channel. Channel has at least 1 item.
4.  **Channel closed**

## **1\. No sender/senders waiting:**

For below two scenarios, the behavior will be the same when there are no receivers waiting.

*   Buffered Channel: Buffer is Empty
*   Unbuffered Channel

The goroutine G1 which is trying to receive, its execution is paused and is resumed only after a send. Let’s see how that happens.

*   The goroutine creates a **sudog** object with goroutine as itself and element being empty
*   It then adds that **sudog** struct to the waiting sending queue recvq **.**
*   The gorutine calls “GOPARK” to the Go RunTime. In response, the Go RunTime changes the status of that Goroutine to **WAITING**.

## **2\. Sender/Senders waiting:**

*   It dequeues the element from the buffer and copies to it to itself
*   Dequeues from the **sendq** and then copy the data directly to the buffer.
*   Set the status of the sender goroutine to **RUNNABLE**

## **3\. Non-Empty Buffer:**

Only applicable for buffered channels:

*   The goroutine reads the data from the buffer

## **4\. Channel closed:**

*   Receives the default value of data type from the channel

*   [sidetoc](https://golangbyexample.com/tag/sidetoc/)