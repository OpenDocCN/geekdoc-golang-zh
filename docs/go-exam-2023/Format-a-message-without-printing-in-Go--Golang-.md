<!--yml

分类：未分类

日期：2024-10-13 06:26:22

-->

# 在 Go（Golang）中格式化消息而不打印。

> 来源：[`golangbyexample.com/format-message-no-print-golang/`](https://golangbyexample.com/format-message-no-print-golang/)

目录

+   概述

+   示例

# **概述**

**fmt** 包的 **Sprintf** 函数可以用来格式化字符串而不打印。它与 **Printf** 函数类似，唯一的区别在于二者。

+   **Printf** 格式化并打印消息。

+   **Sprintf** 仅格式化消息。

以下是 **Sprintf** 函数的语法。

```go
func Sprintf(format string, a ...interface{}) string
```

**Sprintf** 函数根据格式说明符格式化字符串并返回结果字符串。

# **示例**

让我们看一个程序。

```go
package main

import "fmt"

func main() {
    formattedMessage := fmt.Sprintf("Name is: %s. Age is: %d", "John", 21)
    fmt.Println(formattedMessage)
}
```

**输出**

```go
Name is: John. Age is: 21
```

在上述程序中，**fmt.Sprintf** 正确格式化消息，而 **fmt.Println** 用于打印格式化后的消息。


