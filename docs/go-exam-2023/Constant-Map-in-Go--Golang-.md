<!--yml

类别：未分类

日期：2024-10-13 06:28:32

-->

# Go (Golang) 中的常量映射

> 来源：[`golangbyexample.com/constant-map-go/`](https://golangbyexample.com/constant-map-go/)

目录

**   概述

+   示例*  *# **概述**

Go 仅支持四种类型的常量

+   数值（int, int64, float, float64, complex128 等）

+   字符串

+   字符或符文

+   布尔值

Go 不支持常量映射，因此以下程序会引发编译错误

# **示例**

```go
package main

func main() {
	const e = map[string]int{
		"a": 1,
	}
}
```

**输出**

```go
const initializer map[string]int literal is not a constant
```

+   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*
