<!--yml

category: 未分类

date: 2024-10-13 06:01:42

-->

# 在 Go(Golang)中逐字读取大型文件

> 来源：[`golangbyexample.com/read-large-file-word-by-word-go/`](https://golangbyexample.com/read-large-file-word-by-word-go/)

处理大型文件时，显然我们不想将整个文件加载到内存中。在 Golang 中，bufio 包在读取大型文件时提供了帮助。假设我们有一个名为 sample.txt 的文件，内容如下：

```go
This is an example
to show how
to read file
word by word.
```

## **逐字扫描**

```go
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)
func main() {
    WordbyWordScan()
}
func WordbyWordScan() {
    file, err := os.Open("./scanner/sample.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    scanner := bufio.NewScanner(file)
    scanner.Split(bufio.ScanWords)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
    if err := scanner.Err(); err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

```go
This
is
an
example
to
show
how
to
read
file
line
by
line
and
word
by
word.
```

请注意，在上面的程序中，我们设置了**scanner.Split(bufio.ScanWords)**，这有助于我们逐字读取文件。不过需要注意的是，bufio.Scanner 的最大缓冲区大小为 64*1024 字节，这意味着如果你的文件中有任何一行超过 64*1024 的大小，将会出现错误。

```go
bufio.Scanner: token too long
```
