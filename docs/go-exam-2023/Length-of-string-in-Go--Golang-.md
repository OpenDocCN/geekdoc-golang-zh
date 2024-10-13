<!--yml

类别: 未分类

日期: 2024-10-13 06:20:41

-->

# Go语言中的字符串长度

> 来源：[https://golangbyexample.com/length-of-string-golang/](https://golangbyexample.com/length-of-string-golang/)

在Golang中，字符串是字节序列。字符串字面量实际上表示的是UTF-8字节序列。在UTF-8中，ASCII字符是单字节，对应于前128个Unicode字符。所有其他字符占用1到4个字节。为了更好地理解，考虑以下字符串

```go
sample := "a£c"
```

在上述字符串中

+   ‘a’根据UTF-8占用一个字节

+   ‘£’根据UTF-8占用两个字节

+   ‘b’根据UTF-8占用一个字节

上述字符串总共有1+2+1 = 4个字节。因此，当我们尝试使用标准的**len()**函数打印字符串的长度时，它会输出4，而不是3，因为**len()**函数返回的是字符串中的字节数。

```go
fmt.Printf("Length is %d\n", len(sample))
```

因此，独立的for循环不能用于遍历字符串的所有字符，因为它将遍历字节而不是字符。因此，下面的**for**循环将迭代四次，并打印对应于该索引的字节值。

```go
for i := 0; i < len(sample); i++ {
    fmt.Printf("%c\n", sample[i])
 }
```

它将输出下面的字符串，这与**示例**字符串不同

```go
aÂ£b
```

现在我们提到了使用len()函数和for循环的上述限制，让我们看看两种计算字符串长度的方法。

+   使用**utf8包的RuneCountInString**方法

+   使用for-range循环

+   通过将字符串转换为rune数组。

**使用utf8包的**RuneCountInString**方法**

**golang的utf8包提供了一个RuneCountInString方法，可以用来获取字符串的长度。它正确地计算字符串中的rune数量。**

[https://golang.org/pkg/unicode/utf8/#RuneCountInString](https://golang.org/pkg/unicode/utf8/#RuneCountInString)

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	sample := "a£b"

	fmt.Printf("Length of given string is %d\n", utf8.RuneCountInString(sample))
}
```

**输出**

```go
Length of given string is 3
```

**使用for-range循环**

for-range遍历字符串中的Unicode点（在golang中也称为rune），并将正确输出a, £, b。因此，它也可以用于计算字符串的长度。以下是使用for-range与字符串时的格式

```go
for index, character := range string {
    //Do something with index and character
}
```

示例代码

```go
package main

import "fmt"

func main() {
	sample := "a£b"

	len := 0
	//With index and value
	fmt.Println("Both Index and Value")
	for i, letter := range sample {
		len++
		fmt.Printf("Start Index: %d Value:%s\n", i, string(letter))
	}

	fmt.Printf("Length of given string is %d\n", len)
}
```

**输出**

```go
Start Index: 0 Value:a
Start Index: 1 Value:£
Start Index: 3 Value:b
Length of given string is 3
```

**通过将字符串转换为rune数组**

一个rune表示一个Unicode点。通过将字符串转换为rune数组，基本上就是创建该字符串的Unicode点数组。因此，一旦字符串被转换为rune数组，它就可以用来遍历字符串的所有字符。

```go
package main

import "fmt"

func main() {
	sample := "a£b"

	runeSample := []rune(sample)

	fmt.Printf("Length of given string is %d\n", len(runeSample))
}
```

**输出**

```go
Length of given string is 3
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [length](https://golangbyexample.com/tag/length/)*   [string](https://golangbyexample.com/tag/string/)
