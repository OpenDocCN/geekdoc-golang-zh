<!--yml

类别：未分类

日期：2024-10-13 06:16:56

-->

# 在 Go (Golang)中获取主机名

> 来源：[`golangbyexample.com/get-hostname-golang/`](https://golangbyexample.com/get-hostname-golang/)

目录

**   概述

+   代码*  *# **概述**

golang 的**‘os’**包提供了一个**Hostname**函数，可用于获取内核报告的主机名

下面是此方法的签名。如果无法成功获取主机名，则返回错误

```go
func Hostname() (name string, err error)
```

让我们来看一个工作代码

# **代码**

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	hostname, err := os.Hostname()
	if err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
	fmt.Printf("Hostname: %s", hostname)
}
```

**输出**：

```go
Hostname: <hostname of="" your="" machine=""></hostname>
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [主机名](https://golangbyexample.com/tag/hostname/)*   [os](https://golangbyexample.com/tag/os/)*
