<!--yml

类别：未分类

日期：2024-10-13 06:00:16

-->

# Go（Golang）中的结构体接口

> 来源：[https://golangbyexample.com/interface-to-struct/](https://golangbyexample.com/interface-to-struct/)

在编程中，我们有时会遇到一个空接口内部可能包含一个结构体的情况，我们需要从中提取出具体的结构体。对于不清楚空接口是什么的人，应该阅读这篇优秀的文章：[https://research.swtch.com/interfaces](https://research.swtch.com/interfaces)。

为了将interface{}转换为结构体，我们将使用这个库 – [https://github.com/mitchellh/mapstructure](https://github.com/mitchellh/mapstructure)。让我们通过一个示例来理解如何将接口转换为结构体：

```
package main

import (
	"fmt"

	"github.com/mitchellh/mapstructure"
)

type NewCustomerEvent struct {
	Name  string
	Phone string
	Email string
}

func main() {
	newCustomer := NewCustomerEvent{Name: "x", Phone: "082213909101", Email: "xyz@gmail.com"}
	convert(newCustomer)
}

func convert(event interface{}) {
	c := NewCustomerEvent{}
	mapstructure.Decode(event, &c)
	fmt.Printf("Event is: %v", c)
}
```

**输出：**

```
Event is: {x 082213909101 xyz@gmail.com}
```

+   [转换](https://golangbyexample.com/tag/convert/)*   [go](https://golangbyexample.com/tag/go/)*   [接口](https://golangbyexample.com/tag/interface/)*   [结构体](https://golangbyexample.com/tag/struct/)
