<!--yml

类别：未分类

日期：2024-10-13 06:19:13

-->

# Go（Golang）中映射的长度

> 来源：[https://golangbyexample.com/length-map-golang/](https://golangbyexample.com/length-map-golang/)

Golang 内置函数 **len()** 可用于获取映射的长度，即映射中键值对的数量。以下是使用此函数在映射上操作的格式。

```go
len(mapName)
```

让我们看一个程序

```go
package main

import "fmt"

func main() {
    //Declare
    employeeSalary := make(map[string]int)

    //Adding a key value
    employeeSalary["Tom"] = 2000
    employeeSalary["Sam"] = 1200

    lenOfMap := len(employeeSalary)
    fmt.Println(lenOfMap)
}
```

**输出**

```go
2
```

+   [去](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*   [长度](https://golangbyexample.com/tag/length/)*   [映射](https://golangbyexample.com/tag/map/)
