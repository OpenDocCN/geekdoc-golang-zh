<!--yml

分类：未分类

日期：2024-10-13 06:26:53

-->

# 在 Go (Golang) 中的 defer 参数评估

> 来源：[`golangbyexample.com/defer-arguments-evaluation-go/`](https://golangbyexample.com/defer-arguments-evaluation-go/)

目录

+   概述

+   示例

+   输出

# **概述**

**defer** 参数在 defer 语句被评估时进行计算。

让我们看看一个程序。

# **示例**

```go
package main

import "fmt"

func main() {
	sample := "abc"

	defer fmt.Printf("In defer sample is: %s\n", sample)
	sample = "xyz"
}
```

# **输出**

```go
In defer sample is: abc
```

在上面的程序中，当 defer 语句被评估时，**sample** 变量的值为 **“abc”**。在 defer 函数中，我们打印 sample 变量。在 defer 语句之后，我们将 **sample** 变量的值更改为 **“xyz”**。但是程序输出 **“abc”** 而不是 **“xyz”**，因为当 defer 参数被评估时，sample 变量的值为 **“abc”**。


