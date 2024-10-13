<!--yml

分类：未分类

日期：2024-10-13 06:01:35

-->

# 在Go（Golang）中逐行读取大文件

> 来源：[https://golangbyexample.com/read-large-file-line-by-line-go/](https://golangbyexample.com/read-large-file-line-by-line-go/)

当涉及到读取大文件时，显然我们不想将整个文件加载到内存中。Golang中的bufio包在读取大文件时提供了帮助。假设我们有一个名为sample.txt的文件，其内容如下

```
This is an example
to show how
to read file 
line by line.
```

这是程序：

```
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)
func main(){
    LinebyLineScan()
}
func LinebyLineScan() {
    file, err := os.Open("./sample.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }
    if err := scanner.Err(); err != nil {
        log.Fatal(err)
    }
}
```

**输出：**

```
This is an example
to show how 
to read file 
line by line.
```

不过请注意，bufio.Scanner的最大缓冲区大小为64*1024字节，这意味着如果你的文件中有任何一行超过64*1024的大小，那么它将会报错

```
bufio.Scanner: token too long
```

+   [文件](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [大文件](https://golangbyexample.com/tag/largefile/)*   [逐行读取](https://golangbyexample.com/tag/line-by-line/)*   [逐行](https://golangbyexample.com/tag/linebyline/)
