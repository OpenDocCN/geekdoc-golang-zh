<!--yml

分类: 未分类

日期: 2024-10-13 06:07:29

-->

# 在 Go（Golang）中追加到现有文件

> 来源：[`golangbyexample.com/append-file-golang/`](https://golangbyexample.com/append-file-golang/)

os 包的**os.OpenFile()**函数可以用于以追加模式打开文件，然后写入内容

让我们看一个例子。在下面的程序中：

+   首先，使用**ioutil**包写入文件

+   再次以追加模式打开文件并写入第二行

+   读取文件以验证内容。

```go
package main

import (
    "fmt"
    "io/ioutil"
    "log"
    "os"
)

func main() {
    //Write first line
    err := ioutil.WriteFile("temp.txt", []byte("first line\n"), 0644)
    if err != nil {
        log.Fatal(err)
    }

    //Append second line
    file, err := os.OpenFile("temp.txt", os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        log.Println(err)
    }
    defer file.Close()
    if _, err := file.WriteString("second line"); err != nil {
        log.Fatal(err)
    }

    //Print the contents of the file
    data, err := ioutil.ReadFile("temp.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}
```

**输出：**

```go
first line
second line
```

+   [文件](https://golangbyexample.com/tag/file/) *   [go](https://golangbyexample.com/tag/go/)
