<!--yml

分类：未分类

日期：2024-10-13 06:39:56

-->

# Go（Golang）中的通道切片或数组

> 来源：[https://golangbyexample.com/slice-array-channel-golang/](https://golangbyexample.com/slice-array-channel-golang/)

目录

**[概述](#Overview "Overview")**

+   [通道切片](#Slice_of_Channels "Slice of Channels")

+   [通道数组](#Array_of_Channels "Array of Channels")*  *## **概述**

在Golang中也可以创建通道数据类型的切片或数组。事实上，可以创建任何数据类型的切片或数组。本教程包含了创建Golang中通道切片或数组的简单示例。

在此补充，Golang中的数组是固定大小的，而切片可以是可变大小的。

更多细节见此处

数组 – [https://golangbyexample.com/understanding-array-golang-complete-guide/](https://golangbyexample.com/understanding-array-golang-complete-guide/)

切片 – [https://golangbyexample.com/slice-in-golang/](https://golangbyexample.com/slice-in-golang/)

## **通道切片**

```go
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

**输出**

```go
Output for First slice of channels
0xc000118000
0xc000118060
0xc0001180c0

Output for Second slice of channels
0xc000118000
0xc000118060
0xc0001180c0
```

输出在你的机器上会有所不同，因为这是一个地址。

在上面的程序中，我们创建了三个数据类型为int的通道。

```go
channel1 := make(chan int)
channel2 := make(chan int)
channel3 := make(chan int)
```

我们有两种创建通道切片的方法。第一种方式是

```go
var channels_first []chan int
channels_first = append(channels_first, channel1)
channels_first = append(channels_first, channel2)
channels_first = append(channels_first, channel3)
```

在第二种方法中，我们使用**make**命令来创建通道切片。

```go
channels_second := make([]chan int, 3)
channels_second[0] = channel1
channels_second[1] = channel2
channels_second[2] = channel3
```

无论哪种方式都可以。这就是我们如何创建通道切片的方法。

## **通道数组**

```go
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

**输出**

```go
Output for First Array of channels
0xc00008c060
0xc00008c0c0
0xc00008c120

Output for Second Array of channels
0xc00008c060
0xc00008c0c0
0xc00008c120
```

输出在你的机器上会有所不同，因为这是一个地址。

在上面的程序中，我们创建了三个数据类型为int的通道。

```go
channel1 := make(chan int)
channel2 := make(chan int)
channel3 := make(chan int)
```

我们有两种创建数组的方法。第一种方式是

```go
var channels_first [3]chan int
channels_first[0] = channel1
channels_first[1] = channel2
channels_first[2] = channel3
```

在第二种方法中，我们直接用创建的通道初始化数组。

```go
channel_second := [3]chan int{
	channel1,
	channel2,
	channel3,
}
```

查看我们的Golang高级教程。本系列教程详细而全面，我们努力覆盖所有概念并附有示例。本教程适合那些希望获得专业知识和深入理解Golang的人——[Golang高级教程](https://golangbyexample.com/golang-comprehensive-tutorial/)

如果你对如何在Golang中实现所有设计模式感兴趣，如果是的话，那么这篇文章适合你——[所有设计模式Golang](https://golangbyexample.com/all-design-patterns-golang/)

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
