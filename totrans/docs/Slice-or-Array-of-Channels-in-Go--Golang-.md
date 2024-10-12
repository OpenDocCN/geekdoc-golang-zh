<!--yml
category: 未分类
date: 2024-10-13 06:39:56
-->

# Slice or Array of Channels in Go (Golang)

> 来源：[https://golangbyexample.com/slice-array-channel-golang/](https://golangbyexample.com/slice-array-channel-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Slice of Channels](#Slice_of_Channels "Slice of Channels")
*   [Array of Channels](#Array_of_Channels "Array of Channels")*  *## **Overview**

It is possible to create a slice or array of channel data type in Golang as well. In fact, a slice or array can be created of any data type in Go. This tutorial contains simple examples to create a slice or array of channels in golang.

Just to add here that in golang array is of fixed size and slice can have variable size.

More details here

Array – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

Slice – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **Slice of Channels**

```
package main

import "fmt"

func main() {

	channel1 := make(chan int)
	channel2 := make(chan int)
	channel3 := make(chan int)

	//First Way
	var channels_first []chan int
	channels_first = append(channels_first, channel1)
	channels_first = append(channels_first, channel2)
	channels_first = append(channels_first, channel3)

	fmt.Println("\nOutput for First slice of channels")
	for _, c := range channels_first {
		fmt.Println(c)
	}

	//Second Way
	channels_second := make([]chan int, 3)
	channels_second[0] = channel1
	channels_second[1] = channel2
	channels_second[2] = channel3

	fmt.Println("\nOutput for Second slice of channels")
	for _, c := range channels_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First slice of channels
0xc000118000
0xc000118060
0xc0001180c0

Output for Second slice of channels
0xc000118000
0xc000118060
0xc0001180c0
```

The output will be different on your machine as it is an address.

In the above program, we created three channels having data type int

```
channel1 := make(chan int)
channel2 := make(chan int)
channel3 := make(chan int)
```

Then we have two ways of creating a slice of channels. The first way is

```
var channels_first []chan int
channels_first = append(channels_first, channel1)
channels_first = append(channels_first, channel2)
channels_first = append(channels_first, channel3)
```

In the second way, we use **make** command to create a slice of channel

```
channels_second := make([]chan int, 3)
channels_second[0] = channel1
channels_second[1] = channel2
channels_second[2] = channel3
```

Either way works. This is how we can create a slice of channels

## **Array of Channels**

```
package main

import "fmt"

func main() {
	channel1 := make(chan int)
	channel2 := make(chan int)
	channel3 := make(chan int)

	var channels_first [3]chan int

	channels_first[0] = channel1
	channels_first[1] = channel2
	channels_first[2] = channel3

	fmt.Println("Output for First Array of channels")
	for _, c := range channels_first {
		fmt.Println(c)
	}

	channel_second := [3]chan int{
		channel1,
		channel2,
		channel3,
	}

	fmt.Println("\nOutput for Second Array of channels")
	for _, c := range channel_second {
		fmt.Println(c)
	}
}
```

**Output**

```
Output for First Array of channels
0xc00008c060
0xc00008c0c0
0xc00008c120

Output for Second Array of channels
0xc00008c060
0xc00008c0c0
0xc00008c120
```

The output will be different on your machine as it is an address.

In the above program, we created three channels having data type int

```
channel1 := make(chan int)
channel2 := make(chan int)
channel3 := make(chan int)
```

Then we have two ways of creating an array. The first way is

```
var channels_first [3]chan int
channels_first[0] = channel1
channels_first[1] = channel2
channels_first[2] = channel3
```

In second way we directly initialize the array with created channels

```
channel_second := [3]chan int{
	channel1,
	channel2,
	channel3,
}
```

Check out our Golang Advanced Tutorial. The tutorials in this series are elaborative and we have tried to cover all concepts with examples. This tutorial is for those who are looking to gain expertise and a solid understanding of golang – [Golang Advance Tutorial](https://golangbyexample.com/golang-comprehensive-tutorial/)

Also if you are interested in understanding how all design patterns can be implemented in Golang. If yes, then this post is for you –[All Design Patterns Golang](https://golangbyexample.com/all-design-patterns-golang/)

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*