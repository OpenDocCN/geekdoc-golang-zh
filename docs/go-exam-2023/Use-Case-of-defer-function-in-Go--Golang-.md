<!--yml

类别：未分类

日期：2024-10-13 06:26:42

-->

# Go（Golang）中的 defer 函数使用案例

> 来源：[`golangbyexample.com/defer-use-case-go/`](https://golangbyexample.com/defer-use-case-go/)

目录

+   概述

+   示例

# **概述**

如名称所示，Defer 用于推迟函数中的清理活动。这些清理活动将在函数结束时执行。这些清理活动将在一个由 defer 调用的不同函数中完成。这个不同的函数在其所在函数返回之前的最后执行。以下是 defer 函数的语法。

```go
defer {function_or_method_call}
```

理解使用场景 defer 函数的一个好例子是查看写入文件的使用场景。打开以进行写入的文件也必须关闭。

# **示例**

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    file.Close()
    return nil
}
```

在上述程序中，在**writeToTempFile**函数中，我们打开一个文件，然后尝试向该文件写入一些内容。写入完内容后，我们关闭文件。在写入操作期间可能会出现错误，从而导致函数在未关闭文件的情况下返回。**Defer**函数可以帮助避免这些问题。**Defer**函数总是在其所在函数返回之前执行。让我们在这里重写上述程序并使用**defer**函数。

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    err := writeToTempFile("Some text")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Write to file succesful")
}

func writeToTempFile(text string) error {
    file, err := os.Open("temp.txt")
    if err != nil {
        return err
    }
    defer file.Close()

    n, err := file.WriteString("Some text")
    if err != nil {
        return err
    }
    fmt.Printf("Number of bytes written: %d", n)
    return nil
}
```

在上述程序中，我们在打开文件后使用**defer file.Close()**。这将确保即使写入文件时出现错误，文件也会被关闭。Defer 函数确保无论函数中有多少个返回语句，文件都将被关闭。


