<!--yml

category: 未分类

date: 2024-10-13 06:20:46

-->

# 在 Go（Golang）中遍历字符串

> 来源：[https://golangbyexample.com/iterate-over-a-string-golang/](https://golangbyexample.com/iterate-over-a-string-golang/)

在 Golang 中，字符串是字节的序列。字符串字面量实际上表示一个 UTF-8 字节序列。在 UTF-8 中，ASCII 字符是单字节，对应于前 128 个 Unicode 字符。所有其他字符占用 1 到 4 个字节。为了更好地理解，考虑以下字符串：

```
sameple := "a£c"
```

在上述字符串中

+   根据 UTF-8，‘a’占用一个字节。

+   根据 UTF-8，‘£’占用两个字节。

+   根据 UTF-8，‘b’占用一个字节。

上述字符串总共有 1+2+1 = 4 个字节。因此，当我们尝试使用标准的**len()** 函数打印字符串的长度时，它将输出 4，而不是 3，因为 **len()** 函数返回的是字符串中的字节数。

```
fmt.Printf("Length is %d\n", len(sample))
```

因此，独立的 for 循环不能用于遍历字符串中的所有字符，因为它会遍历字节而不是字符。因此，下面的 **for** 循环将循环四次，并打印对应于该索引的字节值。

```
 for i := 0; i < len(sample); i++ {
    fmt.Printf("%c\n", sample[i])
 }
```

它将输出以下字符串，与 **sample** 字符串不同。

```
aÂ£b
```

现在我们提到了使用 `len()` 函数和 `for` 循环的上述限制，让我们来看两种计算字符串长度的方法。

+   使用 for-range 循环

+   通过将字符串转换为 rune 数组。

**使用 for-range 循环**

for-range 遍历字符串中的 Unicode 点（在 golang 中也称为 rune），并将正确输出 a, £, b。因此，它也可以用于计算字符串的长度。下面是使用 for-range 和字符串时的格式。

```
for index, character := range string {
    //Do something with index and character
}
```

示例代码

```
package main

import "fmt"

func main() {
    sample := "a£b"

    for i, letter := range sample {
        fmt.Printf("Start Index: %d Value:%s\n", i, string(letter))
    }
}
```

**输出**

```
Start Index: 0 Value:a
Start Index: 1 Value:£
Start Index: 3 Value:b
```

**通过将字符串转换为 rune 数组**

一个 rune 表示一个 Unicode 点。通过将字符串转换为 rune 数组，基本上就是创建该字符串的 Unicode 点数组。因此，一旦字符串转换为 rune 数组，就可以用于遍历字符串中的所有字符。

```
package main

import "fmt"

func main() {
    sample := "a£b"
    runeSample := []rune(sample)
    fmt.Printf("Length of given string is %d\n", len(runeSample))
    //Iterate
    for i := 0; i < len(runeSample); i++ {
        fmt.Println(string(runeSample[i]))
    }
}
```

**输出**

```
Length of given string is 3
a
£
b
```

+   [go](https://golangbyexample.com/tag/go/)*   [iterate](https://golangbyexample.com/tag/iterate/)*   [string](https://golangbyexample.com/tag/string/)
