<!--yml

类别：未分类

日期：2024-10-13 06:06:54

-->

# 在 Go (Golang) 中写入文件

> 来源：[`golangbyexample.com/write-to-a-file-go/`](https://golangbyexample.com/write-to-a-file-go/)

在 GO 中写文件有不同的方法。让我们通过不同的示例深入探讨如何逐行写入文件。

目录

**使用 bufio 包的缓冲写入**

    +   示例：默认缓冲区大小为 4096 字节

    +   示例：自定义缓冲区大小为 10 字节

+   使用 file.Write()")

+   使用 ioutil.WriteFile()")

# **使用 bufio 包的缓冲写入**

**bufio** 包可以用于在 Go 中进行缓冲写入。它的默认缓冲区大小为 4096 字节，但也可以指定自定义缓冲区大小。缓冲写入的主要优点是将要写入的数据保存在缓冲区中，从而最小化我们执行 IO 操作的次数。它在

+   **writer.flush()** 被调用

+   缓冲区在下次写入时已满。例如，假设缓冲区中有 3500 字节，缓冲区大小为 4096。当要写入的下一行是 1000 字节时，由于 3500 + 1000 > 4096，将会将部分字节写入文件，以便为下一次写入腾出缓冲区。

如果没有缓冲区，则每一行都会进行 IO 写入。让我们看一个写入文件的示例。我们使用 **writer.Available()** 和 **writer.Buffered()** 分别打印 **可用** 和 **已使用** 的缓冲区大小。

## **示例：默认缓冲区大小为 4096 字节**

它将在当前目录中写入 **temp.txt**。写入发生在调用 **writer.Flush()** 时，因为缓冲区在中间并未真正填满。

```go
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Create("./temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    writer := bufio.NewWriter(file)
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        bytesWritten, err := writer.WriteString(line + "\n")
        if err != nil {
            log.Fatalf("Got error while writing to a file. Err: %s", err.Error())
        }
        fmt.Printf("Bytes Written: %d\n", bytesWritten)
        fmt.Printf("Available: %d\n", writer.Available())
        fmt.Printf("Buffered : %d\n", writer.Buffered())
    }
    writer.Flush()
}
```

**输出：**

```go
Bytes Written: 19
Available: 4077
Buffered : 19
Bytes Written: 12
Available: 4065
Buffered : 31
Bytes Written: 19
Available: 4046
Buffered : 50
Bytes Written: 14
Available: 4032
Buffered : 64
```

## **示例：自定义缓冲区大小为 10 字节**

我们还可以使用 bufio.NewWriterSize() 为我们的写入器创建自定义缓冲区大小。在下面的示例中，我们创建了一个自定义缓冲区大小为 10 字节的写入器。如果你仔细注意输出中的字节 **“可用”** 和 **“缓冲”**，你会发现当缓冲区满时，文件写入仍在进行，并在调用 **writer.Flush()** 时结束。

```go
package main
import (
    "bufio"
    "fmt"
    "log"
    "os"
)

//Write file using bufio writer
func main() {
    file, err := os.Create("./temp  .txt")
    if err != nil {
        log.Fatal(err)
    }
    writer := bufio.NewWriterSize(file, 10)
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        bytesWritten, err := writer.WriteString(line + "\n")
        if err != nil {
            log.Fatalf("Got error while writing to a file. Err: %s", err.Error())
        }
        fmt.Printf("Bytes Written: %d\n", bytesWritten)
        fmt.Printf("Available: %d\n", writer.Available())
        fmt.Printf("Buffered : %d\n", writer.Buffered())
    }
    writer.Flush()
}
```

**输出：**

```go
Bytes Written: 19
Available: 1
Buffered : 9
Bytes Written: 12
Available: 9
Buffered : 1
Bytes Written: 19
Available: 0
Buffered : 10
Bytes Written: 14
Available: 6
Buffered : 4
```

# **使用 file.Write()**

它不维护任何缓冲区，并在调用写入时立即写入文件。请参见下面的示例。

```go
package main

import (
    "log"
    "os"
)

func main() {
    file, err := os.Create("./test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    linesToWrite := []string{"This is an example", "to show how", "to write to a file", "line by line."}
    for _, line := range linesToWrite {
        file.WriteString(line + "\n")
    }
}
```

# **使用 ioutil.WriteFile()**

**ioutil.WriteFile()** 是写入文件的一种快捷方式。它接受三个参数 – (1) 文件 (2) 要写入的字符串 (3) 文件的权限模式。如果文件不存在，第三个参数用于以该权限创建文件。调用 **ioutil.WriteFile()** 后，它将

+   如果文件不存在，则以指定权限创建该文件

+   向文件写入内容

+   关闭文件

参见下面的示例：如果 temp.txt 不存在，则将创建一个权限为 0777 的新 temp.txt 文件

```go
package main

import (
    "io/ioutil"
    "log"
)

func main() {
    linesToWrite := "This is an example to show how to write to file using ioutil"
    err := ioutil.WriteFile("temp.txt", []byte(linesToWrite), 0777)
    if err != nil {
        log.Fatal(err)
    }
}
```

+   [缓冲写入](https://golangbyexample.com/tag/buffered-write/) *   [bufio](https://golangbyexample.com/tag/bufio/) *
