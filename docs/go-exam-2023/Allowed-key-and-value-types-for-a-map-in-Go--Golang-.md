<!--yml

类别：未分类

日期：2024-10-13 06:20:56

-->

# Go（Golang）中地图允许的键和值类型

> 来源：[`golangbyexample.com/allowed-key-and-value-types-golang/`](https://golangbyexample.com/allowed-key-and-value-types-golang/)

地图是 Golang 内置数据类型，类似于哈希表，用于将键映射到值。以下是地图的格式：

```go
map[key_type]value_type
```

**key_type**和**value_type**可以是不同类型或相同类型。在下面的示例中，键类型是**字符串**，值类型是**整型**

```go
map[string]int
```

**地图中允许的键类型**

地图键可以是任何可比较的类型。根据 Go 规范定义的一些可比较类型是

+   布尔值

+   数值

+   字符串，

+   指针

+   通道

+   接口类型

+   结构体 – 如果它的所有字段类型都是可比较的

+   数组 – 如果数组元素的值类型是可比较的

根据 Go 规范，一些不可比较的类型不能作为地图中的键使用。

+   切片

+   地图

+   函数

参考 – [`golang.org/ref/spec#Comparison_operators`](https://golang.org/ref/spec#Comparison_operators)

**地图中允许的值类型**

值可以是地图中的任何类型。


