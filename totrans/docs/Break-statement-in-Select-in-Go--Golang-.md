<!--yml
category: 未分类
date: 2024-10-13 06:23:58
-->

# Break statement in Select in Go (Golang)

> 来源：[https://golangbyexample.com/break-keword-select-golang/](https://golangbyexample.com/break-keword-select-golang/)

Table of Contents

 **   [Overview](#Overview "Overview")
*   [Code](#Code "Code")*  *# **Overview**

Break keyword can be used in select to terminate the execution of innermost statement similar to switch and for look. Below is the **break** keyword example in select.

# **Code**

```
package main

import "fmt"

func main() {
	ch := make(chan string, 1)
	ch <- "Before break"

	select {
	case msg := <-ch:
		fmt.Println(msg)
		break
		fmt.Println("After break")
	default:
		fmt.Println("Default case")
	}
}
```

**Output**

```
Before break
```

**break** statement will terminate the execution of innermost statement and below line below will never be executed

```
fmt.Println("After break")
```

*   [go](https://golangbyexample.com/tag/go/)*   [golang](https://golangbyexample.com/tag/golang/)*