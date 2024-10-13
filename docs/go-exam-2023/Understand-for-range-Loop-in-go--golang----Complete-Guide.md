<!--yml

类别：未分类

日期：2024-10-13 06:18:22

-->

# 在 go (golang) 中理解 for-range 循环 – 完整指南

> 来源：[`golangbyexample.com/understand-for-range-loop-golang/`](https://golangbyexample.com/understand-for-range-loop-golang/)

这是 golang 综合教程系列的第十一章。请参考此链接获取该系列的其他章节 – [Golang 综合教程系列](https://golangbyexample.com/golang-comprehensive-tutorial/)

**下一个教程** – [如果-否则](https://golangbyexample.com/understand-if-else-statement-golang/)

**上一个教程** – [for 循环](https://golangbyexample.com/for-loop-in-golang/)

现在让我们查看当前的教程。以下是当前教程的目录。

# **概述**

说到循环，golang 有：

+   for 循环

+   for-range 循环

我们在上一个教程中看到了 for 循环。在本教程中，我们将学习 **for-range** 循环。

**for-range** 循环用于遍历 golang 中不同的集合数据结构，如

+   数组或切片

+   字符串

+   映射

+   通道

现在让我们来看一些示例

# **示例**

## **数组/切片的 for-range 循环**

这是 **for-range** 与数组/切片一起使用时的格式

```go
for index, value := range array/slice {
    //Do something with index and value
}
```

这就是 **for-range** 循环在数组/切片情况下的工作方式。它从索引零开始遍历给定的数组/切片，并为每个存在于索引的值执行 for-range 循环体。在与数组/切片一起使用时，for-range 中的索引和值都是可选的。

下面的示例展示了如何对切片使用 **for-range** 循环

+   带索引和值

+   仅包含值

+   仅带索引

+   无索引和值

```go
package main

import "fmt"

func main() {
    letters := []string{"a", "b", "c"}

    //With index and value
    fmt.Println("Both Index and Value")
    for i, letter := range letters {
        fmt.Printf("Index: %d Value:%s\n", i, letter)
    }

    //Only value
    fmt.Println("\nOnly value")
    for _, letter := range letters {
        fmt.Printf("Value: %s\n", letter)
    }

    //Only index
    fmt.Println("\nOnly Index")
    for i := range letters {
        fmt.Printf("Index: %d\n", i)
    }

    //Without index and value. Just print array values
    fmt.Println("\nWithout Index and Value")
    i := 0
    for range letters {
        fmt.Printf("Index: %d Value: %s\n", i, letters[i])
        i++
    }
}
```

**输出：**

```go
Both Index and Value
Index: 0 Value:a
Index: 1 Value:b
Index: 2 Value:c

Only value
Value: a
Value: b
Value: c

Only Index
Index: 0
Index: 1
Index: 2

Without Index and Value
Index: 0 Value: a
Index: 1 Value: b
Index: 2 Value: c
```

## **带字符串的 for-range 循环**

在 Golang 中，字符串是字节序列。字符串字面量实际上表示 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节的，对应前 128 个 Unicode 字符。所有其他字符则在 1 - 4 字节之间。要更好地理解，请考虑下面的字符串

```go
sample := "a£c"
```

在上面的字符串中

+   ‘a’ 按照 UTF-8 占用一个字节

+   ‘£’ 按照 UTF-8 占用两个字节

+   ‘b’ 按照 UTF-8 占用一个字节

上述字符串总共有 1+2+1 = 4 字节。因此，当我们尝试使用标准 **len()** 函数打印字符串的长度时，它将输出 4，而不是 3，因为 **len()** 函数返回字符串中的字节数。

```go
fmt.Printf("Length is %d\n", len(sample))
```

因此，**独立的 for** 循环不能用于遍历字符串中的所有字符，因为它会遍历字节而不是字符。因此，下面的 **for** 循环将遍历四次，并打印出与该索引处的字节对应的值。

```go
 for i := 0; i < len(sample); i++ {
    fmt.Printf("%c\n", sample[i])
 }
```

它将输出下面的字符串，与 **"a£c"** 字符串不同

```go
aÂ£b
```

上述输出不是我们想要的。这就是 **for-range** 循环在字符串中的应用。它遍历字符串中的 Unicode 点（在 golang 中也称为 rune），并正确输出 a, £, b。以下是与字符串一起使用 **for-range** 的格式

```go
for index, character := range string {
    //Do something with index and character
}
```

在我们进入代码示例之前需要注意的一些要点

+   索引是字符串中 Unicode 字符的起始点。例如在字符串 "a£c" 中，字符 "a" 的起始索引为 0，字符 "£" 的起始索引为 1，而字符 "c" 的起始索引为 2。

+   值是 Unicode 点，基本上是字符串中的每个字符，而不是字节。它也被称为 rune。golang 中的 rune 表示一个 Unicode 代码点。

+   索引和值都是可选的。

现在让我们看一个代码示例。

```go
package main

import "fmt"

func main() {
    sample := "a£b"

    //With index and value
    fmt.Println("Both Index and Value")
    for i, letter := range sample {
        fmt.Printf("Start Index: %d Value:%s\n", i, string(letter))
    }

    //Only value
    fmt.Println("\nOnly value")
    for _, letter := range sample {
        fmt.Printf("Value:%s\n", string(letter))
    }

    //Only index
    fmt.Println("\nOnly Index")
    for i := range sample {
        fmt.Printf("Start Index: %d\n", i)
    }
}
```

**输出：**

```go
Both Index and Value
Start Index: 0 Value:a
Start Index: 1 Value:£
Start Index: 3 Value:b

Only value
Value:a
Value:£
Value:b

Only Index
Start Index: 0
Start Index: 1
Start Index: 3
```

## **带有映射的 for-range 循环**

在映射的情况下，**for-range** 遍历映射的键和值。以下是与映射一起使用时的 **for-range** 格式。

```go
for key, value := range map {
    //Do something with key and value
}
```

值得注意的是，在使用 **for-range** 与映射时，键和值都是可选的。我们来看一个简单的代码示例。

```go
package main

import "fmt"

func main() {
    sample := map[string]string{
        "a": "x",
        "b": "y",
    }

    //Iterating over all keys and values
    fmt.Println("Both Key and Value")
    for k, v := range sample {
        fmt.Printf("key :%s value: %s\n", k, v)
    }

    //Iterating over only keys
    fmt.Println("\nOnly keys")
    for k := range sample {
        fmt.Printf("key :%s\n", k)
    }

    //Iterating over only values
    fmt.Println("\nOnly values")
    for _, v := range sample {
        fmt.Printf("value :%s\n", v)
    }
}
```

**输出**

```go
Both Key and Value
key :a value: x
key :b value: y

Only keys
key :a
key :b

Only values
value :x
value :y
```

## **带有通道的 for-range 循环**

**for-range** 循环在通道中也有不同的工作方式。对于通道，索引没有任何意义，因为通道类似于一个管道，值从一端进入，从另一端退出。

因此在通道的情况下，**for-range** 循环将遍历当前在通道中的值。在遍历完所有当前存在的值（如果有）后，**for-range** 循环不会退出，而是等待可能被推送到通道的下一个值，只有在通道关闭时才会退出。

使用 **for-range** 与通道时的格式如下。

```go
for value := range channel {
    //Do something value
}
```

让我们看一个代码示例。

```go
package main

import "fmt"

func main() {
    ch := make(chan string)
    go pushToChannel(ch)
    for val := range ch {
        fmt.Println(val)
    }
}
func pushToChannel(ch chan<- string) {
    ch <- "a"
    ch <- "b"
    ch <- "c"
    close(ch)
}
```

**输出：**

```go
a
b
c
```

# **结论**

这就是 golang 中 **for-range** 循环的全部内容。希望你喜欢它。请在评论中分享反馈/改进/错误。

**下一个教程** – [如果否则](https://golangbyexample.com/understand-if-else-statement-golang/)

**上一个教程** – [For 循环](https://golangbyexample.com/for-loop-in-golang/)

+   [go](https://golangbyexample.com/tag/go/) * [golang](https://golangbyexample.com/tag/golang/)
