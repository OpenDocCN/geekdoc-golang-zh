<!--yml

分类：未分类

日期：2024-10-13 06:17:27

-->

# 在 Go (Golang) 中复制文件

> 来源：[`golangbyexample.com/copy-file-go/`](https://golangbyexample.com/copy-file-go/)

在本文中，我们将看到两种复制文件的方法。

目录

**   第一种方法

+   第二种方法

## **第一种方法**

**io.Copy()** 可用于将文件从源（src）复制到目标（dest）。成功复制时，将返回 err != nil 而非 err == EOF。

以下是该函数的签名。它返回写入目标的字节数。

```go
func Copy(dst Writer, src Reader) (written int64, err error)
```

**代码**

首先创建一个名为 **“original.txt”** 的文件，并在其中写入一些内容。

```go
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

// Copy a file
func main() {
    // Open original file
    original, err := os.Open("original.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer original.Close()

    // Create new file
    new, err := os.Create("new.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer new.Close()

    //This will copy
    bytesWritten, err := io.Copy(new, original)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Bytes Written: %d\n", bytesWritten)
}
```

**输出：**

```go
Bytes Written: 
```

## **第二种方法**

如果文件内容较少，我们也可以先读取文件内容，然后将所有内容写入新文件。见下面的代码。

在这里同样首先创建一个名为 **“original.txt**” 的文件，并在其中写入一些内容。

```go
package main
import (
    "io/ioutil"
    "log"
)

// Copy a file
func main() {
    //Read all the contents of the  original file
    bytesRead, err := ioutil.ReadFile("original.txt")
    if err != nil {
        log.Fatal(err)
    }

    //Copy all the contents to the desitination file
    err = ioutil.WriteFile("new.txt", bytesRead, 0755)
    if err != nil {
        log.Fatal(err)
    }
}
```

**输出**

```go
new.txt file will have same content as original.txt
```

+   [复制](https://golangbyexample.com/tag/copy/)*   [文件](https://golangbyexample.com/tag/file/)*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
