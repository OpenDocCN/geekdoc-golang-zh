<!--yml
category: 未分类
date: 2024-10-13 06:00:16
-->

# Interface to struct in Go (Golang)

> 来源：[https://golangbyexample.com/interface-to-struct/](https://golangbyexample.com/interface-to-struct/)

We come around a situation sometimes in programming where an empty interface might a struct internally and we have to get the concrete struct out of it. Whoever is not aware of what an empty interface is should read this excellent article: https://research.swtch.com/interfaces.

For conversion of interface{} to a struct, we will use the library – [https://github.com/mitchellh/mapstructure](https://github.com/mitchellh/mapstructure) . Let’s understand how to convert the interface to a struct by an example:

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

**Output:**

```
Event is: {x 082213909101 xyz@gmail.com}
```

*   [convert](https://golangbyexample.com/tag/convert/)*   [go](https://golangbyexample.com/tag/go/)*   [interface](https://golangbyexample.com/tag/interface/)*   [struct](https://golangbyexample.com/tag/struct/)