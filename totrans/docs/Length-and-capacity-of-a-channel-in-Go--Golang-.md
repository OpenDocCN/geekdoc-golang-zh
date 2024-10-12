<!--yml
category: 未分类
date: 2024-10-13 06:24:37
-->

# Length and capacity of a channel in Go (Golang)

> 来源：[https://golangbyexample.com/length-and-capacity-channel-golang/](https://golangbyexample.com/length-and-capacity-channel-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Length and capacity of a buffered channel](#Length_and_capacity_of_a_buffered_channel "Length and capacity of a buffered channel")
*   [Length and capacity of a unbuffered channel](#Length_and_capacity_of_a_unbuffered_channel "Length and capacity of a unbuffered channel")
*   [Length and capacity of nil channel](#Length_and_capacity_of_nil_channel "Length and capacity of nil channel")*  *# **Overview**

Length, as well as capacity only, applies to the buffered channel.  The length of a channel is the number of elements that are already there in the channel whereas the capacity of a buffered channel is the number of elements which that channel can hold.  So length actually represents the number of elements queued in the buffer of the channel while capacity refers to the size of the buffer of the channel. Hence the length of a channel is always less than or equal to the capacity of the channel.

*   len() function can be used to get the length of the channel

*   cap() function can be used to get the capacity of the channel

Length and capacity of unbuffered channel is always zero

# **Length and capacity of a buffered channel**

```
package main

import "fmt"

func main() {
	ch := make(chan int, 3)
	ch <- 5
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))

	ch <- 6
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))

	ch <- 7
	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**Output**

```
Len: 1
Capacity: 3
Len: 2
Capacity: 3
Len: 3
Capacity: 3
```

In the above code, the first created a channel of capacity 3.  After that, we keep sending some value to the channel. As you can notice from your output that after each send operation to the length of channel increases by one while capacity is always the same which is 3.

# **Length and capacity of a unbuffered channel**

Length and capacity of unbuffered channel is always zero

```
package main

import "fmt"

func main() {
    ch := make(chan int)
    fmt.Printf("Len: %d\n", len(ch))
    fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**Output**

```
Len: 0
Capacity: 0
```

# **Length and capacity of nil channel**

Length and capacity of nil channel is always zero

```
package main

import "fmt"

func main() {
	var ch chan int

	fmt.Printf("Len: %d\n", len(ch))
	fmt.Printf("Capacity: %d\n", cap(ch))
}
```

**Output**

```
Len: 0
Capacity: 0
```

Below is the summary table of result of **len()** and **cap()** on different types of channel

| **Command** | **Unbuffered Channel****(Not Closed and not nil)** | **Buffered Channel****(Not Closed and not nil)** | **Closed Channel** | **Nil Channel** |
| Length | 0 | Number of elements queued in the buffer of the channel | -0 if unbuffered channel-Number of elements queued in the buffer if buffered channel | 0 |
| Capacity | 0 | Size of the buffer of the channel | -0 if unbuffered channel-Size of the buffer if buffered channel | 0 |

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*