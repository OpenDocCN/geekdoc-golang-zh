<!--yml
category: 未分类
date: 2024-10-13 06:00:11
-->

# Golang Maps: Not safe for concurrent use

> 来源：[https://golangbyexample.com/go-maps-concurrency/](https://golangbyexample.com/go-maps-concurrency/)

If you are a golang web application developer, then you often might encounter a use case of storing some data in memory in a go map for frequent access. If that is the case with you then you need to be careful because go maps are not safe for concurrent use.

**Buggy code:** Below is buggy code. It might result in a crash if concurrent read and write of map happen.

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

Possible Output: fatal error: concurrent map read and map write

**Correct Code:**

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

**Output:** Some Data