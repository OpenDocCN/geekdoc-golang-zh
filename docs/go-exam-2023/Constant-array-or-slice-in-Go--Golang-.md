<!--yml

分类：未分类

日期：2024-10-13 06:28:10

-->

# Go (Golang) 中的常量数组或切片

> 来源：[`golangbyexample.com/constant-array-golang/`](https://golangbyexample.com/constant-array-golang/)

目录

+   概述

+   示例

# **概述**

Go 只支持四种常量类型

+   数值（int, int64, float, float64, complex128 等）

+   字符串

+   字符或字符集

+   布尔值

Go 不支持常量数组或切片。这是因为在 Go 中常量值在编译时计算，而数组或切片始终在运行时求值。因此，下面的程序会引发编译错误。

# **示例**

```go
package main
func main() {
	const e = [1]int{1}
}
```

**输出**

```go
const initializer [1]int literal is not a constant
```


