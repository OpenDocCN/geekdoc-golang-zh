<!--yml

分类：未分类

日期：2024-10-13 06:00:11

-->

# Golang映射：不安全用于并发使用

> 来源：[https://golangbyexample.com/go-maps-concurrency/](https://golangbyexample.com/go-maps-concurrency/)

如果你是一名Golang网页应用开发者，你可能会遇到将一些数据存储在内存中的用例，以便频繁访问。如果是这种情况，你需要小心，因为Golang映射在并发使用时并不安全。

**有缺陷的代码：** 以下是有缺陷的代码。如果发生映射的并发读写，可能会导致崩溃。

```
package main

var (
   allData = make(map[string]string)
)

func get(key string) string {
	return allData[key]
}

func set(key string, value string) {
	allData[key] = value
}

func main() {
	go set("a", "Some Data 1")
	go set("b", "Some Data 2")
	go get("a")
	go get("b")
	go get("a")
} 
```

可能的输出：致命错误：并发映射读和映射写

**正确代码：**

```
package main

import (
	"fmt"
	"sync"
)

var (
	allData = make(map[string]string)
	rwm     sync.RWMutex
)

func get(key string) string {
	rwm.RLock()
	defer rwm.RUnlock()
	return allData[key]

}

func set(key string, value string) {
	rwm.Lock()
	defer rwm.Unlock()
	allData[key] = value

}

func main() {
	set("a", "Some Data")
	result := get("a")
	fmt.Println(result)
} 
```

**输出：** 一些数据
